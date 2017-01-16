---
layout: post
title:  "Office 365 - Allow external mail to be delivered to public folders"
date:   2017-01-16
categories: Office365
---

In Office 365 Exchange we found it very useful to setup Public mail-enabled folders that all users could access and have a copy of incoming mail delivered to these mailboxes as well as their normal destination. Mail Flow rules are used to direct incoming email approprietly for the scenario you require (simply BCC to the email address of the public folder). You then need to assign user permissions for the folder in the public folders screen, this provides access to those you want to allow to delete etc. The only problem is that external users do not have permission to create email in the public folder and that stops the BCC rule from sending email to that folder. You simply need to allow anonymous access to the folder, easy apart from the browser admin doesn't provide this option. Enter powershell again!

Open Powershell.

First you need to create a connection to Office 365; enter the following command, this will prompt your to authenticate with Office so enter your credentials when asked. 

{% highlight PowerShell %}
$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri "https://outlook.office365.com/powershell-liveid/" -Credential $cred -Authentication Basic -AllowRedirection
Import-PSSession $session
{% endhighlight %}

And then run the following command to assign permissions to the public folders.

{% highlight PowerShell %}
get-publicfolder -recurse | Add-PublicFolderClientPermission -User Anonymous -AccessRights CreateItems
{% endhighlight %}

If for any reason you need to re-run the command for new folders or permissions for Anonymous user have already been created then you may need to run this command first to remove existing permissions.

{% highlight PowerShell %}
get-publicfolder -recurse | Remove-PublicFolderClientPermission -User Anonymous
{% endhighlight %}