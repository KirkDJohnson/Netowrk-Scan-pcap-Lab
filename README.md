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
Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
  Text <br/>
<img src="" alt="Wireshark Scan Analysis"/>
<br />
<br />
<h2>Thoughts</h2>
This lab provided hands on experience analyzing a packet capture (.pcapng file) rather than live analysis on the wire. Obtaining a real packet capture of an attempted malicious attack and analyzing it was incredibly interesting, especially because it was in clear text through http port 80, rather than encrypted. While I have used VirusTotal and CyberChef in previous labs and walkthroughs, knowing that the artifacts pulled from the pcap were real made me we even more curious to dive into resources that others in the cybersecurity community have put together surrounding the attacking IP address and the exploit itself. Moreover, I have used Wireshark previously, but this was the first time I configured Max Mind's GEOIP database to resolve locations from IP addresses and then be able to export as a map, which was another highlight from this lab. 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
