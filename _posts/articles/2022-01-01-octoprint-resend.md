---
layout: post
title: "3D Printing : Octoprint woes"
excerpt: "How to reduce 'Resend Ratio' in your Raspberry/Octopi setup for 3D prints"
categories: articles
tags: [technology, macos, 3dprinting, octoprint, raspberrypi, rfi, usb]
author: sudharsan
comments: true
share: true
image:
  feature: so-simple-sample-image-6.jpg
---

![Adiyogi Lithophane](/images/adiyogi_pic.jpg)

Every once in a while, I get carried away with a new hobby. The latest one in the series is 3D printing. I have been looking at this space since 2015 and neither the cost nor the interest level were enough for me to take the plunge. Then things changed in Dec-2021. What triggered this is when a small plastic part broke that required a replacement of the entire product + my search for a right-size enclosure of a DIY power supply.

I bought an Ender-3-Max. One of the many mods that I have chosen to do is install and run an Octoprint server on a Raspberry PI 3B+ ; If you are into 3D Printing this is a definite upgrade/mod you want to do - it will pay dividends within days. See [Octoprint Site](https://octoprint.org/) for more details.

The point of this post is a bit different. Many (esp. low cost) printers when connected to a Raspberry PI suffer from a 'backpower' issues. That is, power to the motherboard / display on the printer if fed from RPi (in addition to the regular/proper power supply). This has potential to damage both the Pi as well as the printer. 

One way this manifests itself is through the high amount of "Resend"s that the RPi needs to do since the messages get garbled. I used to have as much as 10% or more resends - at some point, Octoprint issues warnings about this.

I was hoping for a simple software fix of disabling USB power output from RPi - Alas, it looks like the support for addressing individual USB ports varies greatly between different Pi models. Folks have suggested all kinds of hardware solutions including 'duct-taping' the 5V power ping in the USB plug to powering the Raspberry from the power supply of the printer (this last idea does not solve the problem of backpowering - but instaead make sure the supply shared the same GND).

After many tries, what worked for me were two changes:
1. Cut the Power and GND wires of a perfectly good USB cable and use it for connecting the RPi to the printer PLUS
1. If the USB cable has a metal shield (good ones do), connect the shielding back - you will see three solders in picture below.. red/white that are disconnected are the power and ground
1. Add a RFI Suppressor on the power cable feeding into the RPi
1. Just to be safe, run the power cable for the RPi about a foot away from the motherboard + display section of the printer.

![USB rewire](/images/octoprint_usb_wire.jpg)

For what cables to cut in USB - See [Wikipedia](https://en.wikipedia.org/wiki/USB) - the pinout diagrams on the right of the page. Essentially, you need to keep the center two (D+/-) cables intact and cut the two that on the outer edges.

Result: Voila - Almost no resends during even long prints...
![Low Resends](/images/octoprint-resend-screen.png)

Since the last month I have had the printer, I have made multiple mods that I will try to share my lessons from:
- Installed a CRTouch Automatic Bed Level sensor
- Upgraded the latest Marlin firmware (Steps are @ [GitHub](https://github.com/p2c2e/Ender3Max-CRTouch)) 
Will share more details on these experiments and more

Useful links:
[Ender 3 Max](https://www.creality.com/goods-detail/creality-ender-3-max-3d-printer)\
[Octoprint](https://octoprint.org/) \
[Marlin](https://marlinfw.org/meta/download/) 


