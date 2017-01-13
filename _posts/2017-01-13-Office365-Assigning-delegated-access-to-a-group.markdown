---
layout: post
title:  "Office 365 - Assigning delegate access to a group"
date:   2017-01-13
categories: Office365
---

In Office 365 in an organisation it is often useful to be able to assign delegated mailbox access to an administrator so that they can maintain or monitor emails. To do this from the user interface web interface can be very time consuming as you have to assign the role to each mailbox one-by-one, if you want to give multiple people access then it is even more time consuming. A nice solution to this is to create a mail-enabled security distribution list that contains as members those whom you would like to give access to. Depending on your setup you should create a distribution group either on your on-premises active directory or using the online web interface; ensure this group has an email address assigned.

Open Powershell.

First you need to create a connection to Office 365; enter the following command, this will prompt your to authenticate with Office so enter your credentials when asked. 

{% highlight PowerShell %}
$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri "https://outlook.office365.com/powershell-liveid/" -Credential $cred -Authentication Basic -AllowRedirection
{% endhighlight %}

And then import the session that has been created. The get-mailbox command can be used to test the connection is working as expected.

{% highlight PowerShell %}
Import-PSSession $session
get-mailbox
{% endhighlight %}

Next you can run the following command to assign the required access rights to the newly created administrators group. Simply replace the email address below with that of your new group.

{% highlight PowerShell %}
Get-Mailbox -ResultSize unlimited -Filter {(RecipientTypeDetails -eq 'UserMailbox')} | Add-MailboxPermission -User yourgroupemailaddress@yourcompany.com -AccessRights FullAccess -InheritanceType all
{% endhighlight %}

If you add new email accounts to the domain then you will need to re-run the script to update those accounts with the permissions.