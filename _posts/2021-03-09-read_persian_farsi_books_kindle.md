---
layout: post
title: "How to read Persian books in Kindle"
date: 2021-03-09 00:00:00 +0000
categories: kindle
---

## Problem

If you have a Kindle and want to read a Persian, Arabic or Hebrew eBooks, you have probably noticed that there are lots of issues with decoding the book. If you are lucky, the words are decoded correctly, but the sentences are displayed in the wrong order, particularly the sentences are shown from Left-To-Right (LTR).

## Solution

It looks like the standards like mobi and ePub have issues with Right-To-Left (RTL) direction[^1]. One solution is converting them to AZW3 format[^2]. Install [Calibre](https://calibre-ebook.com/) and convert your book to AZW3 format. Make sure you have deleted other formats like mobi. Then upload the book to your Kindle. Hopefully this works for you.

References:

[^1]: [Problems with RTL texts](http://www.mobileread.mobi/forums/showthread.php?t=118874)
[^2]: [Conversion of hebrew text from epub to mobi is backwards](https://bugs.launchpad.net/calibre/+bug/1073414)
