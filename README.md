<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this project, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Command Line Interface (CLI)
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
</p>
<p>
• Welcome to my tutorial on Network Security Groups and Inspecting Network Protocols. To get started, you'll need to create two virtual machines (VMs) on Azure. One of these machines should run Linux, while the other should run Windows 10. Both VMs should have dual CPUs, and they must be placed within the same Virtual Network (VNET). Once you've completed this setup, proceed to the Windows machine and download Wireshark by clicking on the following link: Wireshark Download. 
  </p>
  
• After installing Wireshark, launch the application and apply a filter to capture only ICMP traffic. ICMP, which operates at the network layer, is responsible for transmitting messages related to network connectivity issues. It is commonly used by the ping command to test connectivity between hosts. By filtering Wireshark to capture only ICMP packets and then pinging the private IP address of our Linux machine, you will be able to visualize the packets in Wireshark.



</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
• We can inspect each individual packet and see the actual data that is being sent in each ping, as demonstrated in the picture below
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
• Next, we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on its firewall. Once we do that we will stop receiving echo replies from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can then re-allow the traffic by allowing ICMP in the Network Security Groups page in Azure.
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
• Next, we will use our Windows machine to SSH into the Linux machine. SSH will give us access to the machine's CLI. We will also set the Wireshark filter to capture SSH packets only. When we SSH into the Linux machine with the command "ssh labuser@10.0.0.5" (private IP address) we can see that Wireshark starts to immediately capture SSH packets, as shown in the picture below.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
• Now we will use Wireshark to filter for the Dynamic Host Configuration Protocol (DHCP). This operates on UDP ports 67 and 68. It is used to assign IP addresses to client machines. We will request a new IP address with the command "ipconfig /renew". You can see this traffic pictured below.
 
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
• Now we'll analyze DNS (Domain Name Server) traffic by filtering it on Wireshark the same way. We will initiate DNS traffic by typing in the command "nslookup www.google.com." This command essentially asks our DNS server what is Google's IP address. DNS traffic utilizes port number 53. 
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
• Lastly, we will filter for RDP (Remote Desktop Protocol) traffic, which operates on port 3389. When we enter tcp.port==3389 traffic is spammed nonstop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

