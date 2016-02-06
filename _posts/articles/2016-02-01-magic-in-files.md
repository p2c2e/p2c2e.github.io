---
layout: post
title: "There is magic in them files..."
excerpt: "Back in olden days, we used to use a command called 'file'."
categories: articles
tags: [sample-post, readability, test]
author: sudharsan
comments: true
share: true
image:
  feature: so-simple-sample-image-7.jpg
---

Back in olden days, we used to use a command called "file" in *nix and related systems. While the upstart OS2, DOS and Windows operating systems encouraged file extensions by design, *nix always let the user decide what to name each file. A file name ending with ".exe" need not mean it can/cannot be executed. Given the more heterogenous environments and files being copied back-n-forth with total disregard, we had to check whether the file is really what it seems to be... To take a trivial case, text files copied back and forth between DOS and *nix...


~~~~~
$ file README
README: ASCII Text

$ file OTHERFILE.TXT
OTHERFILE.TXT: ASCII text, with CRLF line terminators
~~~~~

The file command was magical. It could not only tell you a file contains text, but whether it is shell script of a particular type etc. If it was an executable/binary, how it was linked, what target architecture it was meant for etc.

What made the command so smart? Turns out that among other things it uses the concept of "Magic Numbers". This is a frequently used technique that involves adding a header section to a file or block to identify the type of the file/data. By reading the first few bytes, if not a bit more, we can guess what a file is meant to contain. 

So, imagine my surprise when reading up on "BitCoin" and the underlying block-chains. As the name suggests, BitCoin and other crypto currencies that rely on distributed ledger mechanisms, use 'blocks' of data. And lo and behold, the first 4 bytes of each block is always "0xD9B4BEF9". Does that mean anything?

In the early days, the magic numbers were selected by geeks to represent something meaningful to them. Here is a sampling ...


| BitCoin Block | 0xD9 B4 BE F9 |
| Java .class files  |  0xCA FE BA BE |
| NxtSTEP - Motorola | 0xFE ED FA CE |
| NxtSTEP - Intel | 0xCA FE BA BE (Nxt Obj-C bears similarity to Java - and common folks)|
| PalmOS calendar arch | 0xBE BA FE CA (oddly reverse ordering of Java)|
| Hist. Uninitialized mem | 0xDE AD BE EF |
| iOS Crash files | 0xDE AD FA 11 |
| Nintendo 'Normal' boot code | 0xD1 5E A5 E* |

Even now-a-days, I occasionlly have to check the leading part of some files when I encounter a corrupt PDF (maybe it is a zip/rar or a postscript file...).