<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this home lab, I observed different networking activities and traffic using multiple ports such as ICMP, SSH, RDP, and DNS through the use of Wireshark and PowerShell on Windows and Linux VMs.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<h3>Part 1: Create Our Resources</h3>
1. Create a Resource Group.
2. Create a Windows 10 Virtual Machine (VM).
3. While creating the VM, select the previously created Resource Group:  

   ![image](https://github.com/user-attachments/assets/d56883e4-c66d-449f-bdba-e254278f6024)
4. Allow the creation of a new Virtual Network (Vnet) and Subnet:  
  
   ![image](https://github.com/user-attachments/assets/18ab6b7b-96ba-40ba-aefc-add4a1b3d6e7)
5. Create a Linux (Ubuntu) VM:  
  
   ![image](https://github.com/user-attachments/assets/833dc5db-1a5a-4b67-b06b-579c83f29e73)
6. While creating the VM, select the previously created Resource Group and Vnet:  
 
   ![image](https://github.com/user-attachments/assets/d147e00c-8903-404b-bae1-9b9502d7b350)
  
   ![image](https://github.com/user-attachments/assets/b2b7a01d-8534-4875-9283-58caa5ea9993)
7. Observe your Virtual Network within Network Watcher.

<h3>Part 2: Observe ICMP Traffic</h3>
1. Use Remote Desktop to connect to your Windows 10 Virtual Machine.
  
  ![image](https://github.com/user-attachments/assets/97bc51a1-f178-4f27-8a2e-6490566c7611)
  
2. Install Wireshark on the Windows 10 VM.
 
   ![image](https://github.com/user-attachments/assets/3bd9a65f-3ad2-4f50-8c8d-b39bc5bddd21)

3. Open Wireshark and filter for ICMP traffic only.  
  
  ![image](https://github.com/user-attachments/assets/23932ef4-f750-43af-a744-f5324c175954)

4. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM.
   - Observe ping requests and replies within Wireshark.  
 
   ![image](https://github.com/user-attachments/assets/a58b7eec-c74a-4285-a371-7731ac64be96)

5. From the Windows 10 VM, open Command Line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in Wireshark.  
 
   ![image](https://github.com/user-attachments/assets/b61edee2-59e7-40cc-b7d9-63e73de8b6ac)

6. Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM.  

   ![image](https://github.com/user-attachments/assets/fdb70a6d-af5a-4905-8268-143c289985c3)

   - Open the Network Security Group (NSG) associated with your Ubuntu VM and disable incoming (inbound) ICMP traffic.  
  
   ![image](https://github.com/user-attachments/assets/76cf1943-2b53-4b0c-8c75-7ea9ff0c65af)

   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity.
   - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using.
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity (it should resume).
   - Stop the ping activity.

<h3>Part 3: Observe SSH Traffic</h3>
1. In Wireshark, filter for SSH traffic only.
2. From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address).
   - Type commands (username, password, etc.) into the Linux SSH connection and observe SSH traffic in Wireshark.
   - Exit the SSH connection by typing 'exit' and pressing [Enter].  
  
   ![image](https://github.com/user-attachments/assets/f97df1ab-7055-4fa4-aec1-71ad3cd27d82)

<h3>Part 4: Observe DHCP Traffic</h3>
1. In Wireshark, filter for DHCP traffic only.
2. From your Windows 10 VM, attempt to issue your VM a new IP address using the command `ipconfig /renew` and observe the DHCP traffic in Wireshark.  

   ![image](https://github.com/user-attachments/assets/b414d309-322d-42c3-aad9-3e81ff9fe56f)

  
<h3>Part 5: Observe DNS Traffic</h3>
1. In Wireshark, filter for DNS traffic only.
2. From your Windows 10 VM, within the Command Line, use `nslookup` to resolve a website's IP address and observe the DNS traffic in Wireshark.  
  
   ![image](https://github.com/user-attachments/assets/755ee9e2-26b9-4b66-acfb-ba5ea87027d0)

<h3>Part 6: Observe RDP Traffic</h3>
1. In Wireshark, filter for RDP traffic only (`tcp.port == 3389`).
2. Observe the immediate non-stop traffic. Why is it non-stop versus only showing traffic when an activity occurs?
   
   RDP traffic is constant because it streams the live display of one computer to another. Unlike SSH, where traffic is generated only when keystrokes are sent, RDP transmits a constant stream of data for the remote session.  

   ![image](https://github.com/user-attachments/assets/baeb7c35-52b3-40ef-9e3b-8966b9632c3c)

<h2>Takeaways and Key Skills Developed</h2>
In this lab, I explored network security and traffic monitoring between Azure VMs using various protocols and tools such as Wireshark and PowerShell. I created both Windows and Ubuntu VMs, observing traffic for ICMP, SSH, DNS, RDP, and DHCP. By filtering traffic in Wireshark, I was able to understand how different protocols behave, such as viewing ICMP traffic during ping tests, SSH traffic during remote connections, and RDP traffic for remote desktop sessions. I also configured Network Security Groups (NSGs) to control traffic, such as blocking ICMP and monitoring its impact in real-time. This lab enhanced my understanding of network security practices, traffic analysis, and the configuration of security measures in a cloud environment.
