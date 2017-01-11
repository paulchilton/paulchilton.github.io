---
layout: post
title:  "Samba File Share on Raspberry Pi"
date:   2013-03-30
categories: RaspberryPi Linux
---

Setting up a simple Samba file server on the Raspberry Pi so that you can copy files across or develop code on a Windows machine.

To most familiar with Linux this is not rocket science but I wanted to document the simple setup I use to allow non-restricted file sharing on the RPi.

This isn't designed to be a secure file server setup as it doesn't have any user authentication but for a local development node it allows quick and easy access.

First ensure Samba is installed:

{% highlight Bash %}
apt-get install samba
{% endhighlight %}

Edit the Samba configuration file:

{% highlight Bash %}
nano /etc/samba/smb.conf
{% endhighlight %}

Replace the contents with the below snippet:

{% highlight Bash %}
[global]
workgroup = workgroup
server string = %h
wins support = no
dns proxy = no
security = share
encrypt passwords = yes
panic action = /usr/share/samba/panic-action %d

[sites]
path = /sites
writeable = yes
browseable = yes
read only = no
guest ok = yes
force user = root
{% endhighlight %}

Finally restart Samba and test the file share

{% highlight Bash %}
/etc/init.d/samba restart
{% endhighlight %}