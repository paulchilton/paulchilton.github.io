---
layout: post
title:  "Installing Jekyll on Windows"
date:   2017-01-10
categories: jekyll
---

Summarising the instructions from the [Jekyllrb Site][jekyll-windowsinstall] these are the steps to install Jekyll on Windows and run Jekyll to serve pages from the local copy of a Github Pages site.

Open Powershell as an administrator.

{% highlight PowerShell %}
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
Answer yes when prompted

iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
choco install ruby -y
exit
{% endhighlight %}

Reopen Powershell as as administrator.

{% highlight PowerShell %}
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
Answer yes when prompted

gem --version
{% endhighlight %}

If the version is less than 2.6 than you need to follow these instructions to update the package otherwise you can get SSL errors when running gem install.
Download the rubygems update: https://rubygems.org/downloads/rubygems-update-2.6.7.gem

{% highlight PowerShell %}
gem install --local C:\rubygems-update-2.6.7.gem
update_rubygems --no-ri --no-rdoc
gem --version
Check the above call gives the correct version
gem uninstall rubygems-update -x
{% endhighlight %}

{% highlight PowerShell %}
gem install jekyll
choco install ruby2.devkit

cd C:\tools\DevKit2
ruby dk.rb init
{% endhighlight %}

Edit C:\Tools\Devkit2.yml and include the path to Ruby "- C:\tools\ruby23"

{% highlight PowerShell %}
choco install libxml2 -Source "https://www.nuget.org/api/v2/"
choco install libxslt -Source "https://www.nuget.org/api/v2/"
choco install libiconv -Source "https://www.nuget.org/api/v2/"

gem install nokogiri --with-xml2-include=C:\Chocolatey\lib\libxml2.2.7.8.7\build\native\include --with-xml2-lib=C:\Chocolatey\lib\libxml2.redist.2.7.8.7\build\native\bin\v110\x64\Release\dynamic\cdecl --with-iconv-include=C:\Chocolatey\lib\libiconv.1.14.0.11\build\native\include --with-iconv-lib=C:\Chocolatey\lib\libiconv.redist.1.14.0.11\build\native\bin\v110\x64\Release\dynamic\cdecl  --with-xslt-include=C:\Chocolatey\lib\libxslt.1.1.28.0\build\native\include --with-xslt-lib=C:\Chocolatey\lib\libxslt.redist.1.1.28.0\build\native\bin\v110\x64\Release\dynamic
   
gem install bundler

Navigate to your blog root folder and run:
bundle update
bundle install
bundle exec jekyll s
{% endhighlight %}



[jekyll-windowsinstall]: https://jekyllrb.com/docs/windows/
