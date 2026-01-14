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
  
   
