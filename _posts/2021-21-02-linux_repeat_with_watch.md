---
layout: post
title: "How linux make our life easier - Part 2"
date: 2021-02-20 00:00:00 +0000
categories: linux
---

_In these series I am going to talk about some simple tweaks in linux that makes life easier_

## How to repeat

Imagine you are going to perform a stress test.
You want to restart a service repeatably to make sure the whole system is fault tolerant.
`watch` is you magic command here. You can use `watch` command to repeat a task.

For example you can restart a docker service every 10 seconds:
{% highlight bash %}
wathc -n 10 docker restart node_1
{% endhighlight %}

This is simple but makes your life easier. Trust me!

You use [tmux](https://github.com/tmux/tmux/wiki) to restart a service in one pane and watch another service in another pane. Keep tmux open for a day, it's fun!
