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
To stop it ppress control-C.
This configuration demonstrates how NSG rules can effectively control and restrict specific traffic, such as ICMP, ensuring tighter security for your VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/hBieJBW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Capture SSH Traffic in Wireshark and Connect to the Ubuntu VM:
Open Wireshark:

Start Wireshark on your Windows 10 VM.
Click on the Shark Fin icon (top left) to start capturing packets.
Filter for SSH Traffic:

In the Filter Bar, type:
bash
Copy
Edit
ssh
Press Enter to apply the filter. This will only display SSH-related packets.
Initiate an SSH Connection to the Ubuntu VM:

Open PowerShell on the Windows 10 VM.
Type the following command:
powershell
Copy
Edit
ssh labuser@<Private-IP>
Replace <Private-IP> with the private IP address of your Ubuntu VM.
Authenticate the SSH Connection:

When prompted with:
Are you sure you want to continue connecting (yes/no)?
Type:
yes
and press Enter.

Enter the Ubuntu VM password (it won’t be visible while typing for security reasons).

Press Enter to log in.

Observe SSH Traffic in Wireshark:

Go back to Wireshark and watch the captured packets.
You should see an increase in SSH packets as the encrypted connection is established and data is transmitted.
Exit the SSH Session:

In PowerShell, type:
exit
Press Enter to close the SSH session.
Review SSH Traffic in Wireshark:

The traffic should decrease after disconnecting.
This confirms that SSH packets were being captured and logged in Wireshark.
This demonstrates how SSH traffic appears in Wireshark and how you can monitor secure remote connections to your Ubuntu VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/HA94h5F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Capture DHCP Traffic in Wireshark and Use a Batch Script to Release & Renew IP:
Open Wireshark:

Start Wireshark on your Windows 10 VM.
Click the Shark Fin icon (top left) to start capturing packets.
Filter for DHCP Traffic:

In the Wireshark Filter Bar, type:
dhcp
Press Enter to apply the filter.
This will show only DHCP (Dynamic Host Configuration Protocol) packets.
Create the DHCP Release/Renew Script:

Open Notepad.
Type the following commands:
ipconfig /release
ipconfig /renew
Click File > Save As.
Navigate to:
C:\ProgramData
Change "Save as type" to "All Files".
Name the file:
dhcp.bat
Click Save and close Notepad.
Run the Batch Script in PowerShell:

Open PowerShell as Administrator.
Navigate to C:\ProgramData:
cd C:\ProgramData
Execute the script:
.\dhcp.bat
Your network connection may briefly disconnect and reconnect as the IP is released and renewed.
Observe DHCP Traffic in Wireshark:

Look for DHCP Discover, Offer, Request, and Acknowledgment packets.
These packets show the process of the DHCP server assigning a new IP.
By following these steps, you successfully triggered and observed DHCP traffic in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/YXRUmh7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Capturing DNS Traffic in Wireshark Using nslookup:
Open Wireshark:

Start Wireshark on your Windows 10 VM.
Click the Shark Fin icon (top left) to start capturing packets.
Filter for DNS Traffic:

In the Wireshark Filter Bar, type:
dns
Press Enter to apply the filter.
This will show only DNS (Domain Name System) packets.
Restart Packet Capture:

Click the green shark fin (top left) to restart the capture.
Use nslookup in PowerShell:

Open PowerShell.
Run the following commands:
nslookup google.com
nslookup disney.com
This will send DNS queries to resolve the IP addresses of google.com and disney.com.
Observe DNS Traffic in Wireshark:

Look for DNS Query packets (sent from your machine to the DNS server).
Look for DNS Response packets (returned from the DNS server with the resolved IP).
You should see the IP addresses of google.com and disney.com in the responses.
Now you've successfully captured DNS resolution requests and responses in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/XmaGe0w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Capturing RDP Traffic in Wireshark:
Open Wireshark:

Start Wireshark on your Windows 10 VM.
Click the Shark Fin icon (top left) to start capturing packets.
Filter for RDP Traffic:

In the Wireshark Filter Bar, type:
yaml
Copy
Edit
tcp.port == 3389
Press Enter to apply the filter.
This will show only Remote Desktop Protocol (RDP) packets.
Observe Non-Stop Traffic:

Since RDP constantly streams the desktop session from one machine to another, you should see continuous traffic between the client and the server.
This happens because RDP needs to send screen updates, keyboard inputs, and mouse movements in real-time.
Now you've successfully captured and analyzed RDP traffic in Wireshark!
</p>
<br />
