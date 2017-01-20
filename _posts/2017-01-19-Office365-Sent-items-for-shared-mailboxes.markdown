---
layout: post
title:  "Office 365 - Emails to go into sent items in a shared mailbox"
date:   2017-01-19
categories: Office365
---

By default emails sent from or on-behalf-of a shared mailbox do not get copied into the sent items folder within that mailbox, instead they only go in your personal sent items folder. Shared mailboxes are really useful for groups of people to collaborate as a team and it is often essential that all members see those sent items. Do not fear though as this can be enabled again (although not from the GUI!).

Open Powershell.

First you need to create a connection to Office 365; enter the following command, this will prompt you to authenticate with Office so enter your credentials when asked. 

{% highlight PowerShell %}
$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri "https://outlook.office365.com/powershell-liveid/" -Credential $cred -Authentication Basic -AllowRedirection
{% endhighlight %}

And then import the session that has been created. The get-mailbox command shows a list of all of the available mailboxes that you can change.

{% highlight PowerShell %}
Import-PSSession $session
get-mailbox
{% endhighlight %}

If you get an error importing the session then you may need to allow remotely signed scripts to run by executing the following command:

{% highlight PowerShell %}
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
{% endhighlight %}

Next run the following command to enable emails sent as the shared mailbox to be copied into the sent items folder:

{% highlight PowerShell %}
set-mailbox <mailboxalias> -MessageCopyForSentAsEnabled $True
{% endhighlight %}

And run the following command to enable emails sent on-behalf-of the shared mailbox to be copied in.
{% highlight PowerShell %}
set-mailbox <mailboxalias> -MessageCopyForSendOnBehalfEnabled $True
{% endhighlight %}
