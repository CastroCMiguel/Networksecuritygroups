<p align="center">
<img src="https://i.imgur.com/iH4DbuD.png" height="80%" width="80%" alt="img"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this lab, we will examine different network traffic going to and from Azure Virtual Machines using Wireshark. Additionally, we will explore the functionality of Network Security Groups. The video will visually walk you through the necessary steps, and afterward, you will find notes and screenshots highlighting key aspects of the material.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools 
- Various Network Protocols (SSH, RDH, DNS, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 
- Ubuntu 20.04

<h2>High-Level Steps</h2>

To begin, I created two virtual machines: one running Windows 10 and the other Ubuntu. I established a Remote Desktop connection to the Windows 10 VM in order to proceed with the lab. Inside the Windows 10 VM, I installed Wireshark and concurrently launched Windows PowerShell. I initiated a continuous ping from the Windows 10 VM to the Ubuntu VM, monitoring the ICMP traffic within Wireshark. To assess the impact, I introduced an inbound rule in the Network Security Group of the Ubuntu VM to block ICMP traffic, which resulted in the failure of the ping requests. Subsequently, I re-enabled ICMP traffic within the Ubuntu VM's Network Security Group, thereby restoring the functionality of the ping requests. I then concluded the ping activity.

Moving on, I SSHed into the Ubuntu VM and observed the SSH traffic within Wireshark. After completing this observation, I terminated the SSH session. Back in the Windows 10 VM, I endeavored to acquire a new IP address by executing the command "ipconfig /renew," all the while monitoring DHCP traffic in Wireshark. Additionally, I employed the nslookup command to inspect DNS traffic as captured by Wireshark. Finally, I monitored the ongoing RDP (Remote Desktop Protocol) traffic in Wireshark. As a concluding step, I deleted the Resource Groups within Azure.

<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com/watch?v=ZbaXeEcC1Ww)

<h2>Synopsis</h2>

<p align="center">
<img src="https://i.imgur.com/BSelWTf.png" height="80%" width="80%" alt="img"/>
</p>

To initiate this lab, I set up two virtual machines: one running Windows 10 and the other Ubuntu. It was crucial to ensure that both VMs were within the same virtual network (vnet) and subnet. In this context, VM1 was designated as the Windows 10 VM, while VM2 was designated as the Ubuntu VM.

<p align="center">
<img src="https://i.imgur.com/4Aymy2t.png" height="80%" width="80%" alt="img"/>
</p>

In the top right corner of the VM1 page on Azure, a public IP address was displayed. I copied this address and employed Remote Desktop to establish a connection with VM1 for the lab.

<p align="center">
<img src="https://i.imgur.com/22XxVZp.png" height="80%" width="80%" alt="img"/>
</p>

Within VM1, I installed Wireshark through Microsoft Edge. Once Wireshark and Windows PowerShell were open, I applied a filter to isolate ICMP (ping) packets. To carry out the lab exercises, I commenced a continuous ping to VM2 using its private IP address. Wireshark captured each ping request and reply exchanged between VM1 and VM2, offering a comprehensive view of the ICMP traffic.

<p align="center">
<img src="https://i.imgur.com/fa37EP2.png" height="80%" width="80%" alt="img"/>
</p>

After confirming the successful ping, I accessed VM2's Network Security Group and implemented an inbound rule to block ICMP traffic. This configuration adjustment effectively halted VM2 from receiving ping requests.

<p align="center">
<img src="https://i.imgur.com/DhgckTb.png" height="80%" width="80%" alt="img"/>
</p>

As anticipated, following the blockade of ICMP traffic, the ping requests from VM1 to VM2 began to time out. In Wireshark's "Info" section, it was evident that VM1 continued to send requests, but there were no responses from VM2, accompanied by a "no response found!" message.

<p align="center">
<img src="https://i.imgur.com/1rWkdeR.png" height="80%" width="80%" alt="img"/>
</p>

Subsequently, I revisited VM2's Network Security Group and located the rule I had previously created. By selecting the "Allow" option under the "Action" category, I reinstated VM2's capability to accept incoming ICMP traffic.

<p align="center">
<img src="https://i.imgur.com/XrA3nkj.png" height="80%" width="80%" alt="img"/>
</p>

With ICMP traffic allowed once more, both the PowerShell terminal and Wireshark indicated that VM2 was transmitting reply packets. To conclude the ICMP observation, I terminated the ping activity in PowerShell.

<p align="center">
<img src="https://i.imgur.com/yVP9QxC.png" height="80%" width="80%" alt="img"/>
</p>

To investigate DHCP traffic, I filtered Wireshark to display only DHCP packets and then attempted to assign a new IP address to VM1 using the "ipconfig /renew" command. Despite the private IP address remaining unaltered, Wireshark recorded the DHCP request and acknowledgment, demonstrating the presence of DHCP traffic.

<p align="center">
<img src="https://i.imgur.com/ZjnaDiv.png" height="80%" width="80%" alt="img"/>
</p>

To examine SSH traffic, I established a remote connection from VM1 to VM2 through the command line. This allowed me to observe SSH traffic in Wireshark.

<p align="center">
<img src="https://i.imgur.com/sArb50p.png" height="80%" width="80%" alt="img"/>
</p>

Following that, I applied a filter in Wireshark to isolate DNS traffic and executed the "nslookup" command for google.com. The terminal displayed both IPv4 and IPv6 addresses, while Wireshark's "Info" section disclosed the existence of A and AAAA records corresponding to the respective IP address types.

<p align="center">
<img src="https://i.imgur.com/GjxMwzd.png" height="80%" width="80%" alt="img"/>
</p>

Finally, I employed Wireshark to monitor RDP (Remote Desktop Protocol) traffic. Since RDP traffic was already being generated by my physical computer's connection to the VM, I did not need to rely on the PowerShell terminal. With a continuous stream of RDP traffic, Wireshark presented each packet transmitted in real-time. Although I usually filtered protocols by name in Wireshark, on this occasion, I opted to filter RDP traffic using its port number (tcp.port == 3389), ensuring the precise capture of the packets.

And with that, we've completed this lab! It's now time to delete the resources we've used to prevent any unnecessary charges from Azure.
