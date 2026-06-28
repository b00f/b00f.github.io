---
layout: post
title: "How to read Persian books in Kindle"
date: 2021-03-09 00:00:00 +0000
categories: kindle
---

## Problem

If you have a Kindle and want to read Persian, Arabic, or Hebrew eBooks,
you have probably noticed that there are lots of issues with decoding the book.
If you're lucky, the words are decoded correctly, but the sentences are displayed in the wrong order.
This happens because the sentences are shown from left to right (LTR), rather than right to left.

## Solution

It seems that formats like MOBI and ePub have issues with the right-to-left (RTL) direction[^1].
One solution is converting them to the AZW3 format[^2]. Install [Calibre](https://calibre-ebook.com/),
add your book, then use **Convert books** to convert it to AZW3.
Make sure you delete other formats, such as MOBI, from your Kindle before uploading the AZW3 version.
Then, upload the book to your Kindle. This should fix the issue.

---

[^1]: [Problems with RTL texts](http://www.mobileread.mobi/forums/showthread.php?t=118874)
[^2]: [Conversion of hebrew text from epub to mobi is backwards](https://bugs.launchpad.net/calibre/+bug/1073414)
