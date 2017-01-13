---
layout: post
title:  "Office 365 - Assigning licenses from powershell"
date:   2017-01-13
categories: Office365
---

When you add new users to your domain it is useful to be able to assign licenses using PowerShell to help automate administrating the domain.

First ensure you have downloaded and installed the AdministrationConfig tool from [here](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Open Powershell and run the following commands to connect to 365, enter your credentials when prompted.

{% highlight PowerShell %}
Import-Module MSOnline
$credential = get-credential
Connect-MsolService -Credential $credential
{% endhighlight %}

Ensure the user's location is set before you assign licenses, replace the email address with the one for the user you are updating. Set the location code to your location, the example shows the GB code for Great Britain.

{% highlight PowerShell %}
Set-MsolUser -UserPrincipalName user@yourdomain.com -UsageLocation GB
{% endhighlight %}

View a list of all of the available licenses and how many remain unassigned.

{% highlight PowerShell %}
Get-MsolAccountSku
{% endhighlight %}

You can add or remove licenses as required. The following example removes an essentials license and adds a premium license to the user.

{% highlight PowerShell %}
Set-MsolUserLicense -UserPrincipalName user@yourdomain.com -RemoveLicenses yourdomain:O365_BUSINESS_ESSENTIALS
Set-MsolUserLicense -UserPrincipalName user@yourdomain.com -AddLicenses yourdomain:O365_BUSINESS_PREMIUM
{% endhighlight %}
