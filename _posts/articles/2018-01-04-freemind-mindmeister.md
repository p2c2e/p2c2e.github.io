---
layout: post
title: "Utility to convert between Freemind (.mm) and MindMeister (.mind) formats"
excerpt: "How to convert from .mind to .mm file format. Utility code in GoLang"
categories: articles
tags: [technology, golang, mindmeister, freemind, mindmap, utilities]
author: sudharsan
comments: true
share: true
image:
  feature: mindmap.jpg
---

I used to be a subscriber for [MindMeister](http://mindmeister.com) Pro (strongly recommend it, if you can afford it). In addition to the cloud storage part of it, the mindmaps look cleaner/slicker than what you would get in FreeMind + there is the Presentation Mode that I love.

As I alluded earlier, I no longer subscribe to MindMeister. Which means, I am out of luck with the already created mind maps + my ability to edit/view mindmaps shared by others.

The Free version of MindMeister still allows you to export to it's own format (.mind)

Yesterday, I wrote a small piece of GoLang code to convert bi-directionally between .mind <> .mm ([Freemind](http://freemind.sourceforge.net/wiki/index.php/Main_Page) format). 

Keep in mind:
- There is data loss during conversion. Only the raw mindmap details - relationships and the text move over
- Embedded images, links to media etc. will be lost

Usage:
```
mind2mm -h 
```
(for help information)
```
mind2mm -in <filename.mind> -out <filename.mm> 
```
(converts from .mind to .mm format)

```
mind2mm -j2m=false -in <filename.mm> -out <filename.mind> 
```
(converts from .mm to .mind format)

Links to binaries:
- [Windows binary](assets/files/mind2mm.exe)
- [MacOS Binary](assets/files/mind2mm)

