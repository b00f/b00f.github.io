---
layout: post
title: "Converting Decimals to Hex"
date: 2020-11-04 00:00:00 +0000
categories: linux
---

_In these series I am going to talk about some simple tweaks in linux that makes life easier_

## Converting Decimals to Hex

[Converting Hex to Binary](https://www.binaryhexconverter.com/hex-to-binary-converter) is quite easy. You just need a paper and a pen.
But sometimes you have a _sequence_ of decimal numbers and you want to calculate their hex values.
`bc` command is your magic tool. With one example I am going to show you how `bc` command works.

In this example we are going to find out the hex values for: `[170 187 204 221 238 255]`:
{% highlight bash %}
$ echo "obase=16; 170;187;204;221;238;255" | bc
AA
BB
CC
DD
EE
FF
{% endhighlight %}

You can play with `obase` and `ibase` options. For instance you can convert hexadecimals to decimals like this:
{% highlight bash %}
$ echo "ibase=16; AA;BB;CC;DD;EE;FF" | bc
170
187
204
221
238
255
{% endhighlight %}

You want more fun, try this command by yourself:
{% highlight bash %}
$ echo "ibase=10;obase=2; 170;187;204;221;238;255" | bc
{% endhighlight %}
