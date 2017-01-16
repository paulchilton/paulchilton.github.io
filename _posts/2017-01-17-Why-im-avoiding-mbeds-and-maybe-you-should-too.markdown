---
layout: post
title:  "Why I'm avoiding mbeds and maybe you should too..."
date:   2017-01-17
categories: Electronics
---

For those that haven't heard of [mbed](http://www.mbed.com), they provide a cloud based compiler for ARM processors with a large codebase and a growing community.

I started using mbed's at work for various projects in 2012 (maybe earlier) and at first their ease of use, huge resource library (even back then) and relatively cheap module considering it had an easy to integrate Ethernet PHY on-board made them a clear winner for many dozen development and production projects. My main interest at the start was being able to utilise the on-board PHY in our systems are provide easy to distribute modules through our systems. If you're interested in what I do then take a look at our website: [Labman Automation Ltd](http://www.labman.co.uk).

![LPC1768 mbed](/assets/2017-01-15-mbed.jpg)

Right back at the start I had some reservations about all my code being in a remote compiler yet it seemed robust enough, I took backup copies of everything and all was good with the world. I then started doing quite a bit of work with the Ethernet stack and using the mbed as a controllable device over TCP/IP from a control PC; this quickly revealed that their Ethernet library was far from robust and  it often crashed (sometime very quickly). This was well documentation at the time with many people having the same issues; after some research I decided to setup the lwip ethernet stack for the mbed and after quite a bit of hard work got an extremely reliable connection. This has been used in production systems for over 4 years now without issues.

Unfortunately mbed have recently decided to make changes to their compiler which has meant that dozens of projects now fail to compile! I always knew this could happen but ignored it. I know that I could work through, debug and fix the compiler problems but that is hardly the point; I shouldn't have to!

![LPC1768 mbed](/assets/2017-01-16-mbed-Error.png)

For this reason I can no longer justify using mbed's for new designs or on live projects, it just doesn't make sense to not maintain full control over the source code and compiler tool chain.
