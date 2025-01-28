<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Configuring a Firewall [Network Security Group
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/qHGokiQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Monitor ICMP Traffic in Wireshark:
Install Wireshark on the Windows 10 Virtual Machine:

Download Wireshark from https://www.wireshark.org.
Install it with default settings.
Start Wireshark Packet Capture:

Open Wireshark.
Click the shark fin icon at the top left to start capturing packets on the active network interface.
Filter for ICMP Traffic:

In the filter bar at the top, type icmp and press Enter.
This ensures that only ICMP (ping) traffic is displayed in the capture.
Ping the Ubuntu Virtual Machine:

Open Command Prompt or PowerShell on the Windows 10 VM.
Type the command:
ping <Ubuntu-Private-IP>
Replace <Ubuntu-Private-IP> with the private IP address of the Ubuntu VM.
Observe the ICMP Echo Request and Reply packets in Wireshark.
Ping a Public Website:

In the same Command Prompt or PowerShell window, type:
ping www.google.com
Watch the ICMP packets in Wireshark as the ping requests are sent to www.google.com and the replies are received.
Observations in Wireshark:
Ping to Ubuntu VM:

You will see a series of Echo Request packets sent from the Windows 10 VM to the Ubuntu VM's IP address.
Corresponding Echo Reply packets will come from the Ubuntu VM back to the Windows 10 VM.
Ping to Public Website:

Similar traffic with Echo Requests and Replies will appear, but the destination will be the public IP address of www.google.com.
Additional DNS traffic may be observed if you haven't resolved Google's IP beforehand.
This demonstrates ICMP traffic filtering and helps understand how ping requests and responses operate at the network level.
</p>
<br />

<p>
<img src="https://i.imgur.com/TAxXaE9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Configure the Firewall to Block ICMPv4 Ping Traffic:
Open the Ubuntu Virtual Machine in Microsoft Azure:

Log in to the Azure portal.
Navigate to your Ubuntu VM.
Access the Network Security Group (NSG):

On the Ubuntu VM overview page, look for the Networking section.
Click on the text next to Network security group to open the associated NSG.
Add a New Inbound Security Rule:

In the NSG settings, expand Settings on the left menu and click Inbound security rules.
Click on + Add to create a new rule.
Configure the Rule Settings:

Source: Leave as Any.
Destination: Leave as Any.
Destination Port Ranges: Change to * (asterisk).
Protocol: Set to ICMPv4.
Action: Select Deny.
Priority: Set to 290 (ensures it is processed before default rules with a lower priority).
Name: Optionally name the rule, e.g., Deny-ICMP-Traffic.
Save the Rule:

Click Add to save the rule. This will now block all ICMPv4 traffic to the Ubuntu VM.
Verifying the Configuration:
Initiate a Continuous Ping:

On the Windows 10 VM, open Command Prompt or PowerShell.
Run the following command to initiate a non-stop ping to the Ubuntu VM:
ping -t <Ubuntu-Private-IP>
Replace <Ubuntu-Private-IP> with the private IP address of the Ubuntu VM.
Observe the Ping Behavior:

Initially, you will see normal ping responses (Reply from <IP>).
After the rule is applied, the responses will change to Request timed out because ICMP traffic is now blocked.
This configuration demonstrates how NSG rules can effectively control and restrict specific traffic, such as ICMP, ensuring tighter security for your VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
