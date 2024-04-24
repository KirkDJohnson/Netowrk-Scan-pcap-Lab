<h1>Wireshark Local Network Scan Lab</h1>

<br />
<h2>Description</h2>
In this lab, I analyzed a packet capture file (referred to as ".pcap" for the rest of the lab) within Wireshark. The first thing I usually do when analyzing a .pcap file is to look at how many packets are in the capture and how long the capture took place. Then I shift my focus to statistics and look at the IPv4 traffic in conversations. It was clear that most of the traffic was between 10.251.96.4 and 10.251.96.5. Then I switched over to TCP traffic, and it was evident that the .4 address was scanning the .5 host using a TCP SYN scan. The obvious next step is to discover the type or types of scanning/reconnaissance done. So, in Wireshark, I filtered for the source address of the scanner and http.user_agent because when a host attempts to connect to a host or website, it will leave a trace of the program or application that made the connection. This uncovered the two scanners used by the threat actor, gobuster, and sqlmap. I then checked the HTTP POST requests, which is how a user would upload something in HTTP, and found the uploaded file "dbfunctions.php". This file allowed the threat actor to execute commands as "cmd". Then they quickly uploaded a Python script for a reverse shell with a Bash shell. After finding these artifacts, the next steps would be to block the IP of the reverse shell 210[.]251[.]96[.]4, which was communicating on port 4422. Look for and delete the file that was uploaded, and implement IDS/IPS to identify port scanning to block the attacker before they get a foothold or find an open port.

<h2>Utility Used</h2>

- <b>Wireshark</b> 

<h2>Environment Used </h2>

- <b>Kali Linux through Oracle Virtual Box

<h2>Lab Overview:</h2>

<p align="center">
Opened the .pcap file in wireshark, the capture lasted 900 seconds or 15 minutes with 17508 packets.<br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/32fcc69f-94c9-4872-be13-35bf812d0b10" alt="Wireshark Scan Analysis"/>
<br />
<br />
Opended statistics -> conversations. Examining IPv4 shows the vast majority of traffic between two local addresses.<br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/9ddb7e2a-89d9-4bfb-90a1-587ef5cb14b4" alt="Wireshark Scan Analysis"/>
<br />
<br />
With TCP traffic, it is clear that the .4 address conducted a port scan of the well known ports (1-1024) against the .5 host.<br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/d4ecb423-d3ae-4a5f-8d59-51b5c7ce3b38" alt="Wireshark Scan Analysis"/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/8af8be6b-2616-4269-a744-302a2e37a2db" alt="Wireshark Scan Analysis"/>
<br />
<br />
When filtering for the for the 210[.]251[.]96[.]4 host it is the evidence is clear it is TCP SYN scan. Typically a stealther way to scan a host because the full TCP handshake doesn't occur that established a connection. <br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/40d5e845-9f96-4610-baab-f4eba7336ec7" alt="Wireshark Scan Analysis"/>
<br />
<br />
Now it is established that the scanning occured and the .4 address focused on port 80, I filtered for that source IP addresses along with http.user_agent to try and find the tools used to scan or enumerate the .5 address. I found that the attacker used gobuster 3.0.1 and sqlmap 1.4.7. <br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/cbcc18e8-6e09-4ead-8982-9d3e1c4309bc" alt="Wireshark Scan Analysis"/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/983d420b-44a6-4870-a799-0d06b8153cb6" alt="Wireshark Scan Analysis"/>
<br />
<br />
I then pivoted to examine if the attacker uploaded any files by filtering for http.request.method POST and found a post under the directory /uploads.php. I clicked on the packet and followed the TCP stream and discovered the name of the file to be "dbfunctions.php" which granted the attacker the ability to execute commands using as "cmd" <br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/14fb9ba7-b2b6-4c1b-896e-bbc6e68a4876" alt="Wireshark Scan Analysis"/>
  <img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/0e519305-b6a6-4384-96a9-0c820fd53f2d" alt="Wireshark Scan Analysis"/>
<br />
<br />
The threat actor was seen using two commands as cmd: "whoami" and a command to import a reverse shell using pything with the listening address 210[.]251[.]96[.]4 on port 4422. <br/>
<img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/0829c449-e0c4-4ed5-b13a-6d44dd4385a9" alt="Wireshark Scan Analysis"/>
  <img src="https://github.com/KirkDJohnson/Network-Scan-pcap-Lab/assets/164972007/6c4e5222-80e1-4bef-ad20-c63a73c32724" alt="Wireshark Scan Analysis"/>
<br />
<br />


<h2>Thoughts</h2>
This lab provided excellent practice with Wireshark. Wireshark is quickly becoming one of my favorite tools to work with, right alongside Splunk, due to the wealth of raw data available for examination. Granted, this .pcap was using HTTP, so it wasn't encrypted, making it much easier to view and investigate events. I've heard in the past that even local addresses shouldn't be trusted, and this scenario serves as a perfect example of why a zero-trust model is necessary. Additionally, port scans and tools like gobuster generate a significant amount of traffic. While it's unlikely for a security analyst to be monitoring the wire every second to identify such activities in real-time, it's quite apparent in the packet logs what was happening. This underscores the importance for organizations to adopt a defense-in-depth strategy to prevent incidents like this. It could have been prevented at various stages: initial port scanning could have been blocked by a firewall, directory enumeration could have triggered an alert from an IPS detecting gobuster, or input sanitization could have prevented attackers from uploading scripts. Overall, this lab was highly beneficial as it brought me in the mindset of how such incidents could be remediated and prevented.
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
