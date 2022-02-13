---
layout: post
title: "Setting up a second disk in TrueNAS mirror mode"
excerpt: "After a custom PC build, I wanted to use my older PC as a NAS server using TrueNAS"
categories: articles
tags: [technology, macos, truenas, storage, nas]
author: sudharsan
comments: true
share: true
image:
  feature: so-simple-sample-image-3.jpg
---

## The Context 
Recently I decided to replace my old custom-build desktop with a newer setup. You may wonder, who uses a desktop anymore? I have been using mine to install licensed software that would be used by everyone in the house (like Adobe, Ableton Live etc.) as well as a local backup of important files. Usually I keep a desktop 'alive' by upgrading components over-time and on-average they are with me for 5-10 years each.

Old-setup - Intel Core i3, Gigabyte MB, SSD boot + 2x 4TB Seagate HDDs, 8GB RAM 
New-setup - AMD Ryzen 7 5800x, MSI MB with 6x SATA, 16G NVMe, SSD Boot, Old HDDs, 16GB RAM

First hurdle to cross : Was the migration of the Windows 10 Pro license from the old SSD to the newer/larger SSD. Thankfully, this was done with no issues. I ensured that the older system license was Digitally activated AND linked to my MS account. Then I created a Restore USB for safety. I had an older copy of Seagate Disktools and I used it to clone the SSD to the newer SSD. 
On using the cloned Disk to boot, I was impressed how resilient Windows 10 was. With a different MB, CPU and monitor, it managed to boot and subsequently download all the newer drivers etc. As expected, Windows 10 reported that it wasn't able to activate windows on the new machine. I used "Change Product Key" and re-entered my key from the earlier machine - and voila - it worked!

## The core of this post - What do I do with the older machine?

I decided to try my hand at setting up a local NAS system. Any 4 drive NAS enclosure (without disk) was costing 50,000 INR or more. Hence this was worth a shot.
After some research, I decided on a few things:
1. Software : There was FreeNAS, TrueNAS and Unraid. Found out that FreeNAS is now TrueNAS Core - and still Free. Unraid is very easy to setup but limited in config options. Settled on [TrueNAS Core](https://www.truenas.com/download-truenas-core/)
1. Hardware : I wanted to understand how to handle upgrade/degrades of disks. So, started by installing TrueNAS on the older SSD + ONE HDD of smaller size (2TB) 

With 1 HDD, there is not much safety and as part of the initial setup, that a single disk 'stripe' was risky and I had to select "Force". This gave me the 1.8Tb of storage. No redundancy. After setting up the share and testing on a Mac, I decided to add another disk.

## Adding the Second Disk - of a different/higher capacity

Adding a second disk - I had a choice of striping - and hence still do redundancy (since I would get larger storage + more speed, but data spread over both drives) OR do a mirror.
I decided to go the Mirror route. After much head-banging I found that there is no way in the WebUI to add a Second Disk to an existing pool with Mirror mode. Hence we needed to get into the shell to do stuff. See here for interesting info on [performance impact of the choices](https://icesquare.com/wordpress/zfs-performance-mirror-vs-raidz-vs-raidz2-vs-raidz3-vs-striped/). Mirroring limits the storage space to the min size amongst all the disks AND slows down performance - but has high reliability.

Steps to setup a Second Disk with Mirroring in an existing Pool

Basically, there are 4 steps
1. Physically install the new drive 
1. Wipe the drive from the WebUI
1. Partition the drive as appropriate
1. Attach the new partitions to the existing ZFS pool and wait for Resilvering to complete (hopefully successful)

I am pasting the 3rd and 4th steps below, in the hopes that it will help others.

(
In my setup, ada0 was the first HDD, ada1 was the Boot SSD and the newly added drive was ada2
pool01 is the name of the storage/data pool with the single ada0 drive - it had two partition - swap and the actual data
I realized that I don't need the swap 
)

(3) 
```
root@truenas[~]#gpart add -i 1 -t freebsd-zfs /dev/ada2
```

(If you want a swap partition as well.. you would do two steps: - subsequent commands will have to reference the right ada2p?
```
gpart add -i 1 -b 128 -t freebsd-swap -s 2g /dev/ada2 # 2G for swap
gpart add -i 2 -t freebsd-zfs /dev/ada2 # For data
```
)

```
root@truenas[~]# glabel status
                                      Name  Status  Components
gptid/45ae1680-8a9b-11ec-b2ce-408d5ceadf9f     N/A  ada0p2 
gptid/890e9a43-8ac7-11ec-8bb9-408d5ceadf9f     N/A  ada1p1
gptid/45a1f4c3-8a9b-11ec-b2ce-408d5ceadf9f     N/A  ada0p1
gptid/785eca5d-8bc9-11ec-8e07-408d5ceadf9f     N/A  ada2p1
```

(4)

```
root@truenas[~]# zpool attach pool01 /dev/gptid/45ae1680-8a9b-11ec-b2ce-408d5ceadf9f /dev/gptid/785eca5d-8bc9-11ec-8e07-408d5ceadf9f
root@truenas[~]# zpool status
  pool: boot-pool
 state: ONLINE
config:

        NAME        STATE     READ WRITE CKSUM
        boot-pool   ONLINE       0     0     0
          ada1p2    ONLINE       0     0     0

errors: No known data errors

 pool: pool01
 state: ONLINE
  scan: resilvered 1.22G in 00:00:11 with 0 errors on Fri Feb 11 22:39:06 2022
config:

        NAME                                            STATE     READ WRITE CKSUM
        pool01                                          ONLINE       0     0     0
          mirror-0                                      ONLINE       0     0     0
            gptid/45ae1680-8a9b-11ec-b2ce-408d5ceadf9f  ONLINE       0     0     0
            gptid/785eca5d-8bc9-11ec-8e07-408d5ceadf9f  ONLINE       0     0     0

```

Notice the "scan:" line above in the status. It shows "resilvered" - i.e. the data has been mirrored. Also, you can see the mirror-0 in the pool. It is also worth noting that the first drive was 2TB and the second drive was 4TB. Since it is setup for mirroring, the capacity is the lowest of all the drives - in our case, it is 2TB. As part of testing, I had 1.22G worth of data, which is mentioend in the scan line.

## Adding a third drive - just for kicks

I was curious to see if I can add an external drive to the setup. I have a few 1TB Passport USB drives lying around. I tried the above flow to get it added to the same pool01.
As is to be expected, I could not attach the drive to the pool, since it is smaller than the smallest drive currently in the pool. i.e. We can mirror all the existing data (though I had not used the storage space yet).

## What is next?

I want to randomly pull out/unplug the drives to see how the UI responds and whether I am able to import and enable disks etc. This has to be done for both the Mirror setup as well as - I would like to try atleast the RAIDZ1 and RAIDZ2. For this, I plan to get a few more 4TB HDDs. Let us see...

