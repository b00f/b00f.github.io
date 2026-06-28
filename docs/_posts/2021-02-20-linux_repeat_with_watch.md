---
layout: post
title: "How to repeat"
date: 2021-02-20 00:00:00 +0000
categories: linux
---

Imagine you are going to perform a stress test.
You want to restart a service repeatedly to make sure the whole system is fault-tolerant.
`watch` is your magic command here. You can use the `watch` command to repeat a task.

For example you can restart a Docker service every 10 seconds:

{% highlight bash %}
watch -n 10 docker restart my_service
{% endhighlight %}

The `-d` flag highlights the differences between each refresh, making it easy to spot changes at a glance:

{% highlight bash %}
watch -d -n 10 docker restart my_service
{% endhighlight %}

This is simple but makes your life easier. Give it a try!

You can also pair `watch` with [tmux](https://github.com/tmux/tmux/wiki) to monitor multiple services side by side.
