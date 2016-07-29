---
layout: post
title: "Userscripts in the browser"
modified:
categories: blog
excerpt: "Recently, someone asked me why and when I learnt Javascript. This post explains how learnt about UserScripts"
tags: [technology, javascript, greasemonkey, userscripts, qif]
comments: true
image:
  feature:
date: 2016-07-29
---

Recently, someone asked me why and when I learnt Javascript. In fact, one of the things I have been using Javascript for a long time is something called 'userscripts'. I realize that many folks still haven't heard about this cool utility - hence this post.

# Firefox to Chrome - GreaseMonkey to TamperMonkey

For the past few years, I have whole heartedly adopted Chrome as my main browser. Prior to that, I was a Firefox fanboy primarily due to the extensions.

One particular extension that I used very frequently was "GreaseMonkey". Until I found that an equivalent for that called TamperMonkey was available on Chrome, I was reluctant to switchover.

For those who have not heard of GreaseMonkey, it is addon that allows you to run your own Javascript code within the context of any page from the net. This allows you to modify the page contents and post/pre process to your heart's content. So, the question then is, why would anyone want to modify webpages that they normally browse.

Here are 3 examples where I used GM scripts:

1. Generate QIF (Quicken Interchange Format) of my bank account statements
1. Enable sections of page / form-elements that are otherwise locked down
1. Add new informational links between sites.

I am sharing more details and code below for those who might want to learn more.

NOTE: For those wanting to run some simple processing of webpages, the ideal solution would be 'bookmarklets'.

For the userscripts cited below, visit my [github.com repo here](https://github.com/p2c2e/tampermonkey-scripts). The README in the repo allows you to install the scripts. You will need to have either GreaseMonkey or TamperMonkey depending on the browser you use. Users of other browsers: Sorry!

# QIF bank details

The userscripts scrape the online banking page when we are visiting and adds a textarea to the bottom of the page with the QIF formatted details. This can be copied to a file and imported into a compatible accounting system.

In my case, I used to use GnuCash for a long time. With multiple bank accounts and Credit card statements, this came in very handy. This use case was also the reason I started delving into jQuery.


# Enhancing pages with additional information

FlipKart is a popular Amazon alternative marketplace based in India. On occasion, the prices of books on Flipkart could be lower. When browsing through their bookstore, I invariably want to read the reviews and the price and Amazon.

To allow this, I have a user script that scans the page for ISBN-like numbers and adds a link to Amazon website.
The code is available on github. For illustration, I am including the code below. It scans the page for 10 digit numbers that are present within specific elements (to avoid clobbering numbers in scripts etc). It then replaces each occurence with a link to the amazon search page with the given ISBN.

You can install the script by clicking [here](https://github.com/p2c2e/tampermonkey-scripts/raw/master/LinkISBN.user.js)

~~~ javascript
(function () {
    var isbnRegex = /\b([0-9]{10,10})\b/ig;

    var allowedParents = [
        "abbr", "acronym", "address", "applet", "b", "bdo", "big", "blockquote", "body",
        "caption", "center", "cite", "code", "dd", "del", "div", "dfn", "dt", "em",
        "fieldset", "font", "form", "h1", "h2", "h3", "h4", "h5", "h6", "i", "iframe",
        "ins", "kdb", "li", "object", "pre", "p", "q", "samp", "small", "span", "strike",
        "s", "strong", "sub", "sup", "td", "th", "tt", "u", "var"
        ];

    var xpath = "//text()[(parent::" + allowedParents.join(" or parent::") + ")]";

    var candidates = document.evaluate(xpath, document, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
    var amznPrefix = 'http://www.amazon.com/gp/search/ref=sr_adv_b/?field-isbn=';

    for (var cand = null, i = 0; (cand = candidates.snapshotItem(i)); i++) {
        if (isbnRegex.test(cand.nodeValue)) {
            var span = document.createElement("span");
            var source = cand.nodeValue;

            cand.parentNode.replaceChild(span, cand);

            isbnRegex.lastIndex = 0;
            for (var match = null, lastLastIndex = 0; (match = isbnRegex.exec(source)); ) {
                span.appendChild(document.createTextNode(source.substring(lastLastIndex, match.index)));

                var a = document.createElement("a");
                a.setAttribute("href", amznPrefix+match[0]);
                a.appendChild(document.createTextNode(match[0]));
                span.appendChild(a);

                lastLastIndex = isbnRegex.lastIndex;
            }

            span.appendChild(document.createTextNode(source.substring(lastLastIndex)));
            span.normalize();
        }
    }
})();
~~~
