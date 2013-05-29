---
layout: post
title: "Virtually the only way to go"
date: 2013-05-29 12:37
comments: true
categories: [development, virtual machines, vagrant, virtualbox, aws]
---

Many developers put a lot of work into getting their projects to run locally on their own machines. While running natively locally may seem easier initially, I really feel like you're setting yourself up for failure.

If you're a Linux user, you're not as likely to encounter some issues, but that likely will just give you a false sense of security. Mac users are slightly worse off since they aren't too far removed from Unix, but there are some definite pitfalls that will trip you up. And Windows developers, I honestly have no idea how you manage to jump through as many hoops as you do to get everything running at all.

Most of this doesn't apply if you're doing mobile app development (Android or iOS), building desktop apps, or working within Microsoft's development environment. By all means, develop as close to your target environment as possible.

And that's exactly the point. Your personal machine is not a server. The absolute easiest way to mimic your target environment is to run a virtual machine on your desktop. With the advent of [Amazon Web Services (AWS)](http://aws.amazon.com), there's really no reason to manage your own hardware. AWS allows you to run a server from an image, and a wise developer will use that exact same image locally.

Sure, it takes a bit more resources locally, but running a virtual machine completely isolates changes to your local system from your development environment, allows you to share an exact environment with other developers, and if something goes wrong, you can always rebuild a new box with ease.

Sounds like a daunting task? It's actually quite easy to get started... and its completely free! [VirtualBox](http://www.virtualbox.org) is free software, provided by Oracle (by way of their Sun acquisition, and they haven't managed to ruin this at all). It provides the layer that reserves resources on your local machine (the host) and provides it to your virtual machine (the guest).

You could use VirtualBox directly, but there's a few rough spots. I highly recommend another layer on top of VirtualBox that will limit your interactions with VirtualBox to installing it, and applying updates. That layer is [Vagrant](http://www.vagrantup.com) which has become an indispensable tool and is widely used.

The combination of VirtualBox and Vagrant is a powerful one. You can run it on Linux, Macs or Windows with ease. Give yourself a couple hours to install and get familiar with them and you'll never look back. It truly is a paradigm shift and you may wonder how you ever got anything done before.

In future posts, I'll outline installing both and getting a simple application running.
