# Network Analysis - Web Shell

*Tools* - Wireshark, TCPDump, TShark

*Difficulty* - Easy

## Scenario
*The SOC received an alert in their SIEM for ‘Local to Local Port Scanning’ where an internal private IP began scanning another internal system.
Can you investigate and determine if this activity is malicious or not? You have been provided a PCAP, investigate using any tools you wish.*

**Questions**
1. ***What is the IP responsible for conducting the port scan activity?***
 
     Port Scanning is a reconnaissance activity where multiple ports of a host are scanned to check if there are any open, closed or filtered ports. Tools that can be used for this can be          `nmap`, `Angry Ip Scanner`, `Masscan` and `Zenmap` We can proceed in the statistics tab in Wireshark to check on the high talkers in the conversations sub-menu. 

   
