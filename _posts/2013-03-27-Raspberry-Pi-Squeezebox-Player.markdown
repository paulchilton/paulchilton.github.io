---
layout: post
title:  "Setting up Raspberry Pi as a Squeezebox Player"
date:   2013-03-27
categories: RaspberryPi Squeezebox
---

In a previous article I detailed the steps I used to setup a Squeezebox server using a RPi. To continue on from this I want to setup a multi-room music system using the Pis. You can locate a RPi in each room in the house and connect a pair a speakers and create an easy multi-room Sonos-like music system.

You can install the player (squeezeslave) on both the server RPi and/or seperate Pi's.

Start by downloading, extracting and renaming the squeezeslave application:

{% highlight Bash %}
cd /usr
sudo mkdir squeezeslavesrc
cd squeezeslavesrc
wget http://squeezeslave.googlecode.com/files/squeezeslave-1.2-367-armhf-lnx31.tar.gz
tar -xvf squeezeslave-1.2-367-armhf-lnx31.tar.gz
mv squeezeslave-1.2-367 squeezeslave
{% endhighlight %}

Ensure you have BZip2 installed:

{% highlight Bash %}
sudo apt-get install bzip2 -y
{% endhighlight %}

Download the following script and setup the initialisation script to run the player when the RPi boots:

{% highlight Bash %}
wget http://sourceforge.net/projects/softsqueeze/files/squeezeslave/squeezeslave-1.2.311/squeezeslave-1.2-311-src.tar.bz2
tar -xjvf squeezeslave-1.2-311-src.tar.bz2
sudo cp squeezeslave /usr/bin
sudo cp squeezeslave-1.2-311/config/squeezeslave.init.debian /etc/init.d/squeezeslave
sudo chmod 755 /etc/init.d/squeezeslave
sudo update-rc.d squeezeslave defaults
{% endhighlight %}

Get the MAC address from the network information:

{% highlight Bash %}
ifconfig
{% endhighlight %}

Copy the MAC address of eth0 from above and paste in the next snippet. This ensures that multiple slaves can all be used on the same network.
Note that any additional configuration required to be applied to the squeezeslave executable should be included in this file, these options are passed to the executable when it is run.

{% highlight Bash %}
sudo echo "SBSHOST=\"-F -mac 00:00:00:00:00:00\"" &gt; defaultsqueezeslave
sudo cp defaultsqueezeslave /etc/default/squeezeslave
{% endhighlight %}

Start the slave player:

{% highlight Bash %}
sudo /etc/init.d/squeezeslave start
{% endhighlight %}

Open the sound mixer application:

{% highlight Bash %}
sudo alsamixer
{% endhighlight %}

Use the up arrow to increase the maximum volume output from the RPi. Press Esc to save and exit.

You can now open up the Squeezebox Server web admin page in your browser. You should see a slave device in the player drop-down.

To change the player name from 'squeezeslave' to something sensible like 'Living Room', open up the Settings from the server webpage, select the player tab and change the description.

Credits go to [DABDig](http://dabdig.blogspot.co.uk/2012/05/running-squeezeslave-on-raspberry-pi.html) for some of the original instructions.