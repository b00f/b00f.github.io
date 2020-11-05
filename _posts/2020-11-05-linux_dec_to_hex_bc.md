---
layout: post
title:  "Linux: How to make life easier - Part 1"
date:   2020-11-04 00:00:00 +0000
categories: linux, life-is-too-short
---


* Linux is fun and life is too short!
I am going to talk about some simple tweaks in linux that male life easier *

** Converting Decimals to Hex **

Concerting Hex to Binary is quite easy. You just need a paper and a pen.
But sometimes you have a *sequence* of decimal numbers and you want to get their hex values. `bc` command makes you life easier.


In this example I need to know what is the hax value for [170 187 204 221 238 255]:
{% highlight bash %}
$ echo "obase=16; 170;187;204;221;238;255" | bc
AA
BB
CC
DD
EE
FF
{% endhighlight %}


You can play with `obase` and `ibase` settings. example:
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
