---
layout: post
title:  "Home Automation Changes"
date:   2017-01-15
categories: Electronics Home-Automation
---

For years now I've had a group of small battery powered devices that capture temperature from different parts of my house and garden and transmit this using RF to a central server where it is logged. This is a project I've had running in various states for a long time; one of the first iterations used Rabbit micro controllers running 'Dynamic C' and a wired connection to groups of DS18B20 one-wire temperature sensors. The last overhaul took inspiration from JC Wippler at [jeelabs.org](http://jeelabs.org/) and I moved across to using 868Mhz RFM12B radio modules from HopeRF and integrated these with ATTINY84 microprocessors. I made the change primarily because in order to continue to expand the number of connected devices I needed to reduce the cable count and my wife was becoming sick of holes being drilled in the walls!

This setup using the RFM12's has been great and is still running strong years later. The server has changed a few times over the years, starting off with the good old Rabbit processors, it went through a brief mbed phase connected to a MySQL server running on another server, this progressed to a RaspberryPi (v1 - because the Pi was going to save the world). The Pi version ran well for a while and then I became sick of corrupted SD cards due to power losses. It's now running on a small low power PC server with Ubuntu, Node.js and a hacked together USB to serial interface connected to the old Pi interface board which in turn has an RFM12B on in receive only mode.

![][oldinterface]

It's been on the cards for a while but I'm now making a start to change the way things work a little, tidy up some of the hardware and ultimately link things together a little more. Over the next few posts I'm going to document what I'm doing in a little more detail and hopefully come up with some new ideas along the way.

I'm hoping to cover the following topics:

* New infrastructure design
* RF base station
* Integrated power sockets
* New node design
* Server software and data logging
* New ideas yet unknown…

[oldinterface]: /assets/2017-01-15-OldRFInterface.jpg
