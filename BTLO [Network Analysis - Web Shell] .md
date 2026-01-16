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

6. ***What is the name of the web shell that the attacker uploaded?***

   From the same stream from the previous question `tcp.stream eq 1270` we see that the referer `http://10.251.96.5/editprofile.php` is a form-data which is information submitted from an HTML form, and this contains an uploaded document which is submitted to the server which is `upload.php` that decides what to do with the file received,by this we can see the file which was uploaded on `editprofile.php` to be by the name `filetoupload` and the filename as `dbfunctions.php`

<img width="1283" height="777" alt="image" src="https://github.com/user-attachments/assets/d750d608-375f-4a68-a34f-a17d3ad45699" />

*ANS: `dbfunctions.php`*

7. ***What is the parameter used in the web shell for executing commands?***

   Looking at the content of `dbfunctions.php` on the same stream `tcp.stream eq 1270` we can see a PHP server-side script

<img width="1257" height="495" alt="image" src="https://github.com/user-attachments/assets/8997303c-8b76-45a9-8ae6-ecab8e3037f5" />

The script contains a parameter `cmd` where the server will execute its values entered as operating system commands and return the output. This is the **Web-Shell**

*ANS: `cmd`*

8. ***What is the first command executed by the attacker?***

   Now that we have the file uploaded and we know its a web-shell containing the parameter `cmd` we can query the attackers Ip whic we know to be `10.251.96.4` and look at **GET** requests using the query `ip.src==10.251.96.4 && http.request.method==GET` but found this to have brought in 4645 packets which are too many to sift through and so i opted to use the query `frame contains "cmd"` 

<img width="1465" height="843" alt="image" src="https://github.com/user-attachments/assets/add8c54b-3f9f-4967-bcd3-fde859fed458" />

We can clearly see that once the script was uploaded the attackers first command was `id` which checks  for the user and group identification. Attackers use this to confirm if the script works and also this leaves minimal footprint.

*ANS: `id`*

9. ***What is the type of shell connection the attacker obtains through command execution?***

    Sticking to the same query from the previous question, we see that the attacker after confirming the *id,whoami* commands he executes a command but looks to be URL encoded, by following 	the stream we can copy the command and drop it on [CyberChef](https://gchq.github.io/CyberChef/#recipe=URL_Decode(true)&input=SGVsbG8lMkMlMjB3b3JsZCUyMQ) an open source tool known as the 	swiss army knife for encryption, encoding,compression etc.

<img width="1915" height="234" alt="image" src="https://github.com/user-attachments/assets/458ef8fd-b8da-433f-88da-07cada500db2" />

<img width="1283" height="333" alt="image" src="https://github.com/user-attachments/assets/94e8ce44-931a-4802-a5ff-3cc0b2763897" />

Once we copy the commmand and upload it to CyberChef we will use the Recipe *URL Decode* to decode the encoded command

<img width="1541" height="737" alt="image" src="https://github.com/user-attachments/assets/31d2b1b6-d7ce-47ec-8df4-91e94ab7bbff" />

The output shows us that the command is telling the server to open a live remote command session back to the attacker this is called *Reverse Shell*

<img width="828" height="220" alt="image" src="https://github.com/user-attachments/assets/1f763144-345d-45d2-9673-df058165f2b3" />

*ANS: `Reverse`*

10. ***What is the port he uses for the shell connection?***

On the same decoded output from the above question we can see that the connection is going back to `10.251.96.4:4422`

<img width="960" height="169" alt="image" src="https://github.com/user-attachments/assets/2a787649-4865-4f71-9f1b-94311d27cf92" />

*ANS: `4422`*

**CONCLUSION**
***This investigation confirmed that the attacker successfully compromised the web application by abusing a file upload functionality. A PHP web shell (dbfunctions.php) was uploaded through a legitimate profile edit feature and stored in a web-accessible directory where server-side code execution was permitted.

Network traffic analysis revealed that the attacker verified command execution using basic system reconnaissance commands before escalating to an interactive remote shell. This was achieved by issuing commands through the web shell that caused the server to initiate an outbound connection, granting the attacker real-time control of the system.

Overall, the incident demonstrates how insufficient file upload validation and improper execution permissions can lead to full server compromise. From a defensive perspective, this highlights the importance of strict file type validation, disabling script execution in upload directories, and monitoring outbound connections from web servers as indicators of post-exploitation activity.***





















