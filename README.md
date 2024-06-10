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

<h2>Actions and Observations</h2>

<img width="1535" alt="Screen Shot 2024-05-19 at 6 30 45 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/b7f16b5a-b641-4380-86ea-1c1eb575b0b3">

Step 1. In Azure, Setup a Resource Groups 

<img width="1529" alt="Screen Shot 2024-05-19 at 6 31 35 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/6715bfb6-69ba-43d7-89c8-fc8cfed22568">
<img width="1536" alt="Screen Shot 2024-05-19 at 6 33 39 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/a21c9777-4d51-4c55-8362-602217c7733f">
<img width="1537" alt="Screen Shot 2024-05-19 at 6 34 21 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/74624ef3-bd75-46f2-8b7c-90959259fdff">
<img width="1536" alt="Screen Shot 2024-05-19 at 6 42 32 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/c36358e7-d71b-41cf-9f43-755b54534ed4">


Step 2. Create 2 Virtual Machines 
 - Create a Windows 10 Virtual Machine (VM-1)
 - While creating the VM, select the previously created Resource Group
 - While creating the VM, create a new Virtual Network (Vnet) 
 - Create a Linux (Ubuntu) Virtual Machine (VM-2)
 - While create the VM, select the previously created Resource Group and Vnet
 - Observe Your Virtual Network within Network Watcher

Note: Wait for VM-1 to finish Deploying before creating VM-2. If not, the Vnet you created on VM-1 wont show up when creating VM-2.

<img width="2306" alt="Screen Shot 2024-05-19 at 6 47 30 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/2d16cdc1-f2c4-416e-a7de-cabd1f1e8424">

Step 3. Open Microsoft Remote Desktop (If on Mac)
 - Click Add PC
 - for PC name, use VM-1's Public IP Address
 - Open VM-1 on the Microsoft Remote Desktop after adding the PC

<img width="1376" alt="Screen Shot 2024-05-20 at 2 57 55 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/8f7d99bb-15b9-4e52-bbca-500cee05319e">

Part 4. Install Software Within Windows 10 VM-1
 - Install WireShark
 - 
 - Open Wireshark filter for ICMP traffic only by hitting the blue shark icon and typing in "ICMP"

<img width="2545" alt="Screen Shot 2024-05-20 at 3 01 21 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/efa8b0ca-c3af-428a-9db5-cea35dd4e69e">
<img width="1351" alt="Screen Shot 2024-05-20 at 3 03 47 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/d02e482d-d96b-4e8b-9be1-93a305a8e683">

 - Retrieve the private IP address of the Ubuntu VM-2 and attempt to ping it from within the Windows 10 VM-1
 - Observe ping request and replies within wireshark

<img width="1344" alt="Screen Shot 2024-05-20 at 3 13 03 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/610d0a79-8ea8-46a2-8c98-b63cc3e930f9">



Part 5. Initiate a perpetual/non-stop ping from your Windows 10 VM-1 to your Ubuntu VM-2
 - On PowerShell, type in the private IP again along with a nonspot pin "-t"
 - Observe the ICMP traffic in WireShark and the command line Ping activity

<img width="1520" alt="Screen Shot 2024-05-20 at 3 16 02 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/0ac0d1f3-366c-4c82-8284-ab5c7d7911df">

 - Open the Network Security Group your Ubuntu VM-2 is using and disable incoming (inbound) ICMP traffic
 - Network Security group -> VM-2-nsg -? Inbound security rules -> Add
 - Back in the Windows 10 VM-1, observe the ICMP traffic in WireShark and the command line Ping activity after adding a new rule

<img width="1350" alt="Screen Shot 2024-05-20 at 3 19 26 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/e1a71e54-efe2-4efb-a6d5-039f32ef5349">
 - "Request timed out"
 - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM-2 is using
 - Back in the Windows 10 VM-1, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
 - Stop the ping activity with shift+control+C

<img width="1350" alt="Screen Shot 2024-05-20 at 3 30 58 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/9eb773c9-3134-4184-88cc-2d89407091db">
<img width="1351" alt="Screen Shot 2024-05-20 at 3 33 33 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/a5fedd0e-28b8-47d5-80b3-82178f2c10d3">

 Part 6. Back in Wireshark, filter for SSH traffic only
  - From your Windows 10 VM-1, “SSH" into your Ubuntu Virtual Machine (via its private IP address)
  - Exit the SSH connection by typing ‘exit’ and pressing [Enter]


<img width="1351" alt="Screen Shot 2024-05-20 at 3 43 46 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/54867d2b-b649-4041-b61a-9b7ea3f0144e">

Part 7. Filter for DHCP traffic only 
 - From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
 - Observe the DHCP traffic appearing in WireShark


<img width="1350" alt="Screen Shot 2024-05-20 at 3 50 03 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/11d2db84-2d77-4c8a-b5d4-864ae2424481">
<img width="1351" alt="Screen Shot 2024-05-20 at 3 52 02 PM" src="https://github.com/DereHz/azure-network-protocols/assets/169094076/bfd3e7e5-2400-4964-a998-efc35c2b645a">


Part 8. Filter for DNS traffic only 
 - From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are
 - Observe the DNS traffic being show in WireShark
