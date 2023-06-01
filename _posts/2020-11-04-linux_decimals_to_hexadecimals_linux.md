---
layout: post
title: "Converting Decimals to Hex"
date: 2020-11-04 00:00:00 +0000
categories: linux
---

[Converting Hex to Binary](https://www.binaryhexconverter.com/hex-to-binary-converter) is quite easy.
You just need paper and a pen.
But sometimes you have a sequence of decimal numbers, and you want to calculate their hexadecimal values.
`bc` command is your magic tool.
The `bc` command in Linux is a command-line utility that stands for "basic calculator".
It provides a simple way to perform mathematical calculations within the terminal.
Here's an example of how to use `bc` command for simple mathematic calculation:

{% highlight bash %}
$ echo "2 + 2" | bc
4
{% endhighlight %}

We can use the `bc` command to convert decimal numbers to their hexadecimal equivalent.
For example, we can use `bc` to obtain the hexadecimal values for: `[170 187 204 221 238 255]`:

{% highlight bash %}
$ echo "obase=16; 170;187;204;221;238;255" | bc
AA
BB
CC
DD
EE
FF
{% endhighlight %}

You can play with the `obase` and `ibase` options. For instance you can convert hexadecimals to decimals like this:
{% highlight bash %}
$ echo "ibase=16; AA;BB;CC;DD;EE;FF" | bc
170
187
204
221
238
255
{% endhighlight %}

If you want more fun, try this command by yourself:

{% highlight bash %}
$ echo "ibase=10;obase=2; 170;187;204;221;238;255" | bc
{% endhighlight %}
