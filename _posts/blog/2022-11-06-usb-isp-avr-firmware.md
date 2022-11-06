---
layout: post
title: "Rabbithole from TinyML to Arduino to USB ISP Firmware flashing"
modified:
categories: blog
excerpt: "How to get cheap chinese USB-ISP programmer clones with ATmega88 chips inside to work on MacOS"
tags: [technology, arduino, nano, usbasp, usbisp, tinyml, zhifengsoft]
comments: true
image:
  feature:
date: 2022-11-06
---

# Introduction - Getting USB ISP Programmer (clones) to work on MacOS

What started as an attempt to do a bit TinyML led to a day long exercise in getting a USB-ISP V2.0 programmer to work. These are cheap programmer boards used to flash programs to ATmega, ATtiny microcontrollers from Atmel. These micro-controllers usually very easy to use since most folks use them via Boards like Arduino UNO, Nano, Pro Micro and the like that have USB-to-Serial built-in.

If you need to program the chips and use the bare minimum components, you will have to move away from the Arduino boards to the bare-essentials.

BUT, to do that, you will need to somehow flash firmware to the chips. Enter: USB-ISP Programmers.

I bought a very cheap one for around 4 USD only to find out that it does not work (on Mac Monterey atleast). 
Rather than return the device, I tried to figure it out and benefited from the lessons from many folks who had already made the journey.

To save time for others:
- Summarized the symptoms 
- Modified and compiled the source code
- Written a small Readme section for folks to follow

You can check out the repository at https://github.com/p2c2e/usbasp.2011-05-28-zhifengsoft-USB-ISP-Clone.git

Device that I bought: https://robocraze.com/products/usb-isp-programmer-version-2-0
Image: ![How the device looks like](https://cdn.shopify.com/s/files/1/0262/6564/9240/products/HatchnHack_Makerspace_HNH_Cart_Components-46_72fc1b49-2b22-429e-bd43-a8e6d2adb7b3_800x.jpg?v=1648799456)
