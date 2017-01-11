---
layout: post
title:  "Setting up Raspberry Pi as a Squeezebox Server"
date:   2013-03-27
categories: RaspberryPi Squeezebox
---

This guide describes how to setup a Raspberry Pi as a Squeezebox server. It assumes a clean installation of Rasbian is setup and ready on the RPi. This will allow you to stream music from a central server to one or more players via your network. You can combine this with a wireless network and power your RPi from a battery to make this a truely portable music system.

Player clients can be either 'proper' Squeezebox products or you can use software players together in the same system. I will detail in a seperate post how to setup the RPi to be a Squeezebox client, you can have the player running on the same device as the server or you can have it running on a seperate RPi. You can also choose to synchronise multiple devices to the same music source and therefore stream the same music to multiple rooms!

Start by updating libraries and installing some dependencies:

{% highlight Bash %}
sudo apt-get update; sudo apt-get dist-upgrade
sudo apt-get install libjpeg8 libpng12-0 libgif4 libexif12 libswscale2 libavcodec53
{% endhighlight %}

Download and install the Logitech Media Server:

{% highlight Bash %}
wget http://downloads.slimdevices.com/LogitechMediaServer_v7.7.2/logitechmediaserver_7.7.2_all.deb
sudo dpkg -i logitechmediaserver_7.7.2_all.deb
sudo service logitechmediaserver stop
{% endhighlight %}

Download some helpful scripts from All Things Pi:

{% highlight Bash %}
wget http://allthingspi.webspace.virginmedia.com/files/lms-rpi-raspbian.tar.gz
tar -zxvf lms-rpi-raspbian.tar.gz
{% endhighlight %}

Patch the Perl bootstrap module:

{% highlight Bash %}
sudo patch /usr/share/perl5/Slim/bootstrap.pm lms-rpi-bootstrap.patch
{% endhighlight %}

Copy files to the correct locations for running and create some symbolic links:

{% highlight Bash %}
sudo mv arm-linux-gnueabihf-thread-multi-64int /usr/share/squeezeboxserver/CPAN/arch/5.14/
sudo mv libmediascan.so.0.0.0 libfaad.so.2.0.0 /usr/local/lib
sudo mv /usr/share/squeezeboxserver/Bin/arm-linux/faad /usr/share/squeezeboxserver/Bin/arm-linux/faad.old
sudo mv faad /usr/share/squeezeboxserver/Bin/arm-linux
sudo ln -s /usr/local/lib/libmediascan.so.0.0.0 /usr/local/lib/libmediascan.so
sudo ln -s /usr/local/lib/libmediascan.so.0.0.0 /usr/local/lib/libmediascan.so.0
sudo ln -s /usr/local/lib/libfaad.so.2.0.0 /usr/local/lib/libfaad.so
sudo ln -s /usr/local/lib/libfaad.so.2.0.0 /usr/local/lib/libfaad.so.2
sudo ldconfig
{% endhighlight %}

Set permissions for the server files:

{% highlight Bash %}
sudo chown -R squeezeboxserver:nogroup /usr/share/squeezeboxserver/
{% endhighlight %}

Start the server running:

{% highlight Bash %}
sudo service logitechmediaserver start
{% endhighlight %}

Navigate to http://10.0.0.31:9000 (where 10.0.0.31 is the IP of your RPi) and configure the Squeezebox server.