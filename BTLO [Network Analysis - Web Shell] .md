# Network Analysis - Web Shell

*Tools* - Wireshark, TCPDump, TShark

*Difficulty* - Easy

## Scenario
*The SOC received an alert in their SIEM for ‘Local to Local Port Scanning’ where an internal private IP began scanning another internal system.
Can you investigate and determine if this activity is malicious or not? You have been provided a PCAP, investigate using any tools you wish.*

**Questions**
1. ***What is the IP responsible for conducting the port scan activity?***
 
   Port Scanning is a reconnaissance activity where multiple ports of a host are scanned to check if there are any open, closed or filtered ports. Tools that can be used for this can be          `nmap`, `Angry Ip Scanner`, `Masscan` and `Zenmap` We can proceed in the statistics tab in Wireshark to check on the high talkers in the conversations sub-menu. 

   <img width="1360" height="560" alt="image" src="https://github.com/user-attachments/assets/2c8bb20e-8f8e-4a0e-bea9-e03ee3ed5f85" />

  Filtering by the highest byte size we can see two IPs sharing a lot of bytes this can be indicative of possible data exfiltration though its a small size this can be compressed files,         lateral movement or malware stagging. Filtering the two conversation we will be able to see a clear picture. There =fore i proceeded wth the filter `ip.src==10.251.96.4 && ip.dst==10.251.96.5`

<img width="1555" height="414" alt="image" src="https://github.com/user-attachments/assets/3d2f4ee5-6747-464d-a0d5-937e585cf9cf" />

we immediately notice a high volume of port scanning done by IP `10.251.96.4` to `10.251.96.5` this is evidence of port scanning activity
	
 *ANS: `10.251.96.4`*
  
2. ***What is the port range scanned by the suspicious host?***

   We will look at the *Endpoints* tab under *Statistics* and filter the *Port* from the least to the highest ***Conversations --> Endpoints --> TCP

<img width="1369" height="560" alt="image" src="https://github.com/user-attachments/assets/eadc69e2-98ab-4cc9-b179-1239223e7984" />

<img width="1368" height="557" alt="image" src="https://github.com/user-attachments/assets/088e19af-2342-43e0-ab65-ca50a9034f51" />

*ANS: `1-1024`*

3. ***What is the type of port scan conducted?***

   When we look at the two IPs we see alot of `SYN` scan being sent to the host IP `10.251.96.5` using the protocol `TCP` and hence this is a `TCP SYN`scan

   <img width="681" height="224" alt="image" src="https://github.com/user-attachments/assets/c79ee231-ed6d-4aee-83d6-2d60a253721c" />

*ANS: `TCP SYN*

4. ***Two more tools were used to perform reconnaissance against open ports, what were they?***

   Here we need to filter out the ***User Agent Strings*** which often leave a signature of the tools used using the filter `ip.addr == 10.251.96.5 && http.user_agent ` Looking though we see a tool `gobuster` which is often used for ***Directory Brute-Forcing*** . So now we have our first tool `gobuster 3.0.1`.

<img width="1572" height="806" alt="image" src="https://github.com/user-attachments/assets/1b606070-5d31-400e-8fac-d1475ca57393" />

Going further down while paying attention on the `user-agent` field we come across another tool `sqlmap` which is an open source tool used to automate the process of detecting SQL injection vulnerabilities in websites.

<img width="1575" height="776" alt="image" src="https://github.com/user-attachments/assets/a1ce1d54-2ebf-47e0-81c4-f72bf4977a26" />

*ANS: `Gobuster 3.0.1, sqlmap 1.4.7`*

5. ***What is the name of the php file through which the attacker uploaded a web shell?***

   We need to a understand what a webshell is, this is a malicious script uploaded to a compromised web server that helps the attacker gain remote control and execute commands. Since this is being uploaded we are going to look at the **POST** requests using the query `http.request.method==POST`. We come across an `upload.php` on packet No. 16102, following the stream we are going to focus on the header of the client which is in red. 

<img width="1588" height="785" alt="image" src="https://github.com/user-attachments/assets/6b7a64a7-5ff6-42d4-919d-e6cab4ba65a6" />

From the image looking at the referer of `upload.php` which means where it was directed from and we can see the original upload was directed from `http://10.251.96.5/editprofile.php` . This confirms that the file which was uploaded was `editprofile.php`

*ANS: `editprofile.php`*







