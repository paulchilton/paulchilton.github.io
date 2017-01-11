---
layout: post
title:  "VPN Setup with L2TP & IPSEC"
date:   2011-11-13
categories: Windows
---

<p>For anyone who's tried to setup VPN it is without a doubt a "black art". The information given by most sites on the net is incomplete, incorrect and sometimes just plain stupid. We setup VPN a while back at work with PPTP and after an initial learning curve managed it relatively easily. We quickly learnt that PPTP is not a very secure method to use and started to setup a L2TP/IPSec connection, this is where the fun really started!<br />
<span id="more-224"></span><br />
Here I will explain the difficulties and solutions we found for setting up PPTP and L2TP/IPsec VPN on Windows 2003 Server. We have a Windows 2003 server setup with two network adapters, one connected to a local private network, the other connected to a firewall and an ADSL internet connection. The server has NAT setup to translate packets for the internal network.</p>
<h2>PPTP</h2>
<p>First of all there was PPTP...</p>
<p>PPTP was fairly easy to setup but not without its own difficulties!</p>
<h3>The first step was to setup the server:</h3>
<ul>
<li>Add the Routing and Remote Access Role with VPN.</li>
<li> In RAS (Routing and Remote Access), add a Policy to allow VPN access by adding a rule for Virtual (VPN) Port and granting the policy access. We also added a condition that limited the users able to logon to a specific "VPN Users" group.</li>
<li> Set the authentication methods you wish to use, we selected only MS-CHAP v2. Do this both in the VPN policy you just created and in the properties of the server in RAS (security tab).</li>
<li> In ports, select PPTP. Enable and enter a number of ports to allow at any one time.</li>
</ul>
<h3>Next setup the firewall/router to allow VPN traffic trough:</h3>
<p>Open the following ports on the firewall:</p>
<ul>
<li>TCP 1701 (PPTP)</li>
<li> IP Protocol 47 (GRE) - Note: Protocol NOT UDP or TCP</li>
</ul>
<h3>Setup the client PC to connect:</h3>
<ul>
<li>Create a VPN connection.</li>
<li> Enter the IP to connect to.</li>
<li> In TCP/IP properties within the VPN properties, go to Advanced and UN-check "Use default gateway on remote network", otherwise all your internet traffic will be routed through the VPN connection and there is no need for it (normally).</li>
<li> Enter the domain user credentials of someone with access to connect to VPN.</li>
<li> Windows XP users may need to start off with the Windows firewall disabled.</li>
</ul>
<p>Connect... Done! (maybe...)</p>
<p>This is where we came across our major head-scratcher with PPTP... It didn't work externally! Our first thought... the firewall! It had to be the firewall! After days and days of playing with different combinations and configurations we decided to swap to a different ADSL modem. Straight away, connected! The moral: don't eliminate anything; don't presume the modem will allow VPN traffic!</p>
<p>Tip: Always test the VPN connection from an internal connection (i.e. not through the router/modem) where possible before testing externally. If it doesn't work locally then you stand no chance from the outside world!</p>
<h2>L2TP</h2>
<p>Next there way L2TP...</p>
<p>L2TP with IPSec is much more secure than PPTP and is a better choice, however there is more to go wrong!</p>
<h3>Setup the server (presuming you already have PPTP working):</h3>
<ul>
<li> In the security tab (RAS, Server Properties) check the 'Allow custom IPSEC policy for L2TP connection' box and enter a passkey.</li>
<li> In ports select L2TP, enable it and enter the number of ports you would like to use.</li>
</ul>
<h3>Setup the firewall:</h3>
<p>Open the following ports on the firewall:</p>
<ul>
<li>IP Protocol 50</li>
<li> IP Protocol 51</li>
<li> UDP 4500 (NAT-T traffic)</li>
<li> UDP 500</li>
<li> UDP 1701 (L2TP)</li>
</ul>
<h3>Setup the client:</h3>
<ul>
<li>Copy your existing (working I hope) PPTP connection and edit it.</li>
<li> In the networking tab, select L2TP.</li>
<li> In IPSec setting, enter the pre-shared key you chose on the server.</li>
</ul>
<p>Connect... Does it work? Yes - Great, go home and take a break! No - Read on...</p>
<h3>Microsoft Windows Registry Fix</h3>
<p>Firstly there is the thing that Microsoft forgot to explain properly that you might have come across but weren't sure whether you needed to do it. In XP and Vista you need to add a key to the registry! Yes it's strange but true! This is where we wasted hours not getting things up and running.</p>
<ul>
<li> Open the registry editor (ask for help if you are not sure at this point!)</li>
<li> Navigate down to \Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IPSec\ (on XP)</li>
<li> Navigate down to \Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\PolicyAgent\ (on Vista)</li>
<li> Enter the following as a new DWORD value
<ul>
<li> AssumeUDPEncapsulationContextOnSendRule</li>
<li> Set the value to 2</li>
</ul>
</li>
<li> Reboot the computer (you must reboot for the change to take affect)</li>
</ul>
<p>Try connecting again, it may just work.</p>
<h2>Debugging</h2>
<p>If not then there are a few debugging tips that may help you figure out where the problem is. First you need to establish where the problem is.</p>
<h3>VPN Connection Stages</h3>
<p>Although the following isn't a complete list of stages that are performed when VPN connects it outlines the basic stages of connecting for these purposes:</p>
<ul>
<li>Host is looked up (DNS).</li>
<li> Client connects to server to establish an IPSec security association (SA) through Internet Key Exchange (IKE) on port 500.</li>
<li> IPSec Phase 1 (Main Mode) is established with IKE.</li>
<li> IPSec Phase 2 (Quick Mode) is established with IKE</li>
<li> Establish Encapsulating Security Payload (ESP) on protocol 50.</li>
<li> L2TP tunnel is established on UDP port 1701 between the SA endpoints.</li>
</ul>
<p>The first check is that the server IP address is correct, confirm by connecting via PPTP again (if this is working). Ping the server (if ping is let through the firewall). Confirm manually the IP address of the server.<br />
Next, evidence of the VPN connection (if it is reaching the server) can be seen in the system security event log. On the server open up the windows event log and clear the security log. Try connecting from the client and then refresh the log and look for a RemoteAccess entry. Check the details, this might be your connection.</p>
<p>If the connection is not seen in the eventlog check the firewall settings (at both ends). Check the RAS VPN policy and settings are correct.</p>
<p>If the connection is in the event log then it is reaching the server at least. There are now a few things that can be checked: Enable IKE logging, Check client SA is being established and debug packets being sent/received on the client and server.</p>
<h3>Enable IKE Logging</h3>
<p>On the Windows 2003 server, run the following to disable logging and free access to the log file:<br />
netsh ipsec dynamic set config ikelogging 0</p>
<p>Navigate to C:\WINDOWS\Debug\ and delete the Oakley.log file</p>
<p>Run:<br />
netsh ipsec dynamic set config ikelogging 1</p>
<p>Attempt to connect from the client.</p>
<p>Run:<br />
netsh ipsec dynamic set config ikelogging 0</p>
<p>Open the Oakley.log file in the C:\WINDOWS\Debug\ directory.</p>
<p>If there is no entries in the file (or it does not exist) then an IPSec connection request hasn't even started. If there is log entries then read through carefully and look for the two phases of IPSec (Main Mode and Quick Mode). If all is working, there should be entries stating that Phase 1 SA is accepted and Phase 2 SA is accepted. If there are errors in the log then these need to be looked up and fixed.</p>
<h3>Check client is establishing IPSec over IKE</h3>
<p>On the client open up MMC (run: mmc.exe). Add a snap-in and add the IP Security Monitor. Expand the Main mode and Quick mode tree to see the two SA folders. Open the properties for the client in the MMC console and set the auto-refresh to every 1 second. Now start to connect via VPN, watch to see if an entry appears in the Main Mode SA, if it does, check the Quick Mode SA. If an entry appears in here then IPSec is being established and probably isn't the problem. If not then check those IKE logs on the server again, something is not working here.</p>
<h3>Packet debugging</h3>
<p>Packet debugging can give a good indication of what is and what isn't happening during the VPN connection sequence. There are a few good packet analysis applications about but I use &lt;a href='http://www.wireshark.org/' target='_blank'&gt;Wireshark&lt;/a&gt;.</p>
<p>Install on one or both machines and start off listening on the network adapater that VPN is connecting out of on that machine. I also recomend filtering the packets based on the destination of the VPN server otherwise you'll get loads of extra packets and things get confusing fast!<br />
Enter: (ip.src == 1.2.3.4) || (ip.dst == 1.2.3.4) into the filter box (where 1.2.3.4 is the IP of the VPN server. If running Wireshark on the server, enter the IP of the host trying to connect (public IP if connecting across WAN).<br />
Then attempt to connect VPN... there should be at least some packets showing up, if not check the filter entered is correct.</p>
<p>What you should see may vary slightly with each setup but the general flow is as follows:</p>
<ul>
<li>ISAKMP - Identity Protection (Main Mode)  =  There should be 5 or 6 of these as 'Main Mode' (phase 1) is established.</li>
<li> ISAKMP - Quick Mode  =  There should be about 4 of these as 'Quick Mode' (phase 2) is established.</li>
<li> ESP packets  =  If connected, plenty of these should come flooding in.</li>
<li> UDPENCAP  =  UDP Encapsulation packets - these are UDP packets that are encapsulated for passing through NAT.</li>
</ul>
<p>Note: Look out for packets only showing in one direction, each stage there should be packets back and forth from/to the server.<br />
If you see all of the above but it is still not connecting, check your security settings - username/password, pre-shared key, turn off any firewalls (including Windows firewall) and check the &lt;a href="vpn.html#registryfix"&gt;Windows registry hack&lt;/a&gt; has been performed and the PC rebooted.</p>