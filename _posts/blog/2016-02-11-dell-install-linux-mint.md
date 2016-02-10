---
layout: post
title: "A decent developer laptop running on Linux"
modified:
categories: blog
excerpt: "Steps that I took to build a new developer laptop"
tags: [linux, dell, technology, windows]
image:
  feature:
date: 2016-02-11T15:39:55-04:00
---

After a *long* hiatus, I recently decided to follow developments on the technology front. Most of my recent work had not given me either the time nor the need to do this.

For starters, I figured I would not want to mess with our family's shared desktop and buy myself a new laptop (What better excuse could I come up with?).

Choice of laptops:

## Basic specifications 
i5+, 8G+, 500G+ - without Windows preferably (to keep the costs low). No point paying for Windows and then instlling Limux anyway.
## Brands considered 

Dell, Asus, Lenovo, Acer 

These brands were listed on ease of availability in India/locally, cost, non-windows options and finally my prioir experience with them.

Acer: We could get an i7 at the cost of i5 from the rest of the brands. Cons: Lack of availability and pre-sales response - imagine what would happen after getting stuck with them!

Lenovo: Pros: I still think of them as ThinkPad. Cons: Cost and the feedback on build quality + service in recent times

Asus: Pros: Hardware and in some cases, the build Cons: Availability in India + servicing (my most recent laptop was an Asus bought in US and I had issues trying to upgrde the RAM)

Dell: Pros: Prior exp (my desktop is 8 yrs old - it was bleeding edge when I bought it. Now, other thn the motherboard everything is changed. It still keeps chugging on...) Cons: Cost - Compared to Acers of the world. Though they technically have Ubuntu versions in the line-up, none are available easily (really sad to see this).

## Decision:

Went with the Dell due to sweet spot of price, availability and prior experience.
Dell 5558 - i5 - 15" - non-HD version w/ Windows 8.1

## Prepping the machine:
Since I had paid the Windows tax, I might as well ensure that I case use it if required. So, first step was to complete the install and activate 8.1

Next: Upgrade to Windows 10 while the free-upgrade offer exists
I wanted to note down the Serial details in case I needed to reinstall. Here are some tips:
The serials listed by various tools might all be slightly different. Not sure what is the 'correct' one. I made note of everything once in 8.1 and then once again in Window 10.
From what I understand once Windows 10 is activted using "Digital Entitlement", we can re-install Window 10 anytime in the future by just clicking "Skip" in the serial screen. Once the system boots up, it will connect to MS servers and reactivate based on some set of system parameters.

## Choice of Linux flavors:
I have always been partial to Fedora - I have been using some flavor of RedHat on my personal machines since 1998 (when the my other main option of interest was Slackware - that will give you some perspective)
Finally decided to go with one of either Fedora or Limux Mint 
While Fedora would have given me bleeding edge stuff and helped me with tinkering, I want to tke this opp to learn something new. With yum given way to dnf, I might as well learn apt-get/aptitude!
Linux Mint Cinnamon was the choice

## Install Mint on 5558 
After a flawless install (custom layout), I found the machine was not booting up! Something about not being able to find a bootable partition. After racking my brain about active/primry partition, layout etc. I decided to peek into the BIOS settings.
Dell ships with Windows w/ UEFI enabled. I suspected that some kind of security check was causing the issues. 
Changed BIOS settings as followws and installed / booted without issues:
 Disable UEFI and enable Legacy Mode for accessing the disks
 Ubuntu that ships on Dell supposedy has tweaks to get things working. So, it is not same as the one we would download from the ubuntu/mint sites.
Back in the days - I remember grappling nVidia (free/non-free) driver issues all the time. I had to have custom patches to get things to work. Pleasantly surprised to see the screen at full resolution off the bat!

## Teething issues:
I have been noticing that 'suspend' seems to work, 'resume' works only occasionally. On resumption, the screen displays the last view (whether it was the lock screen or desktop) and there is no cursor, any screen refreshes.
Based on a hunch and some googling, I have since changed the "Administration -> Login Window -> Options -> Default Session" to have "Cinnamon (Software Rendering)". I have since not seen issues for the past couple of resumes.
IF you try the last tip, you should logout and log-back into the system after the change.

## Cool parts:
Around 2006 is when I last had a primary Linux machine. THere was lot of discussion on cgroups at the time when I was with qemu, kvm and virtual box. I did not quite understand what the use case was.
Glad to note that I was wrong. The whole *docker* ecosystem would not be possible without cgroups. Will jot down notes on how to set things in layman terms

## Finding your Windows serial number:
If you have a Certified Windows sticker on your machine, you are good to go1
If not, you may want to try one of these tools: Speccy, Belarc Advisor ; Many of the tools "feel" shady. 
There are some 'generic' serial numbers that are floating around. Not sure what they are meant for.
My serial number reported by tools before/after the Windows 10 upgrade were different. 
