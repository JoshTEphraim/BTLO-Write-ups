# HAUNTED

***

*Tools - exiftool, officemalscanner, CyberChef*

*Category - Threat Intelligence*

*Difficulty - Easy*

***

## Scenario

***Haunted Company Inc., a long-established Credit Reporting Agency, has been successfully operating in major financial hubs such as New York, London, and Tokyo. As a privately owned entity without external investors, the company has maintained consistent client satisfaction and steady earnings reports. With plans for expansion, the management has decided to take the company public, and the Initial Public Offering (IPO) is scheduled to occur within the next few days.***

***However, a crisis emerged just as the IPO date approaches. One of the company's websites has been defaced, raising alarms. Shortly after, it is discovered that the company's Tokyo server has come under attack. Concerned about the timing and the potential damage to the company’s reputation, the management is determined to identify the threat actor behind this attack and understand the breach mechanism to create detection before the IPO.***

***As a Threat Intelligence Analyst, you are tasked with collaborating with other analysts to uncover the identity of the adversary and assess the situation.***

***Available External and Internal Threat Intelligence:***

***New York(External: Business Commonality): Report on the 2017 GenX Breach, a major cyber attack on a leading Credit Reporting Agency. London(Internal Intelligence: Adversary Analysis): Analysis report for Haunted Company Inc., including Asset-Threat Mapping and adversary analysis featuring FIN7, APT27, Twisted Spider, and TG-3390, all of which are known to target the finance sector. Tokyo(Cyber Activity Attribution): Malware analysis from the compromised server, providing critical insights into the tools used during the attack.***

**QUESTIONS**

1. ***In 2017, a well-known company was attacked. What is the name of the company, its country of origin, and its business model?***

  To begin our investogation we need to start by reading the *README.txt* file given under the folder *Investogation*. We are given the scenario and a link which we will proceed to open it.

<img width="1140" height="583" alt="image" src="https://github.com/user-attachments/assets/2a11ded4-0bec-42bb-a6bd-844bea22ee16" />

Opening the link we find a website were we are asked to decode a *base64* line but we cannot see the whole code fully, we can get it by looking at the *page source* or under the *Investigaion* folder we are given a file named *DecodeME.txt* which we will proceed to open and copy the encoded text and paste it on CyberChef to decode the text given

<img width="1144" height="605" alt="image" src="https://github.com/user-attachments/assets/2371eebd-acf8-45e1-8e72-b8c3c3bf4daa" />

<img width="1535" height="680" alt="image" src="https://github.com/user-attachments/assets/6a67c93e-4000-4295-b5e7-5eb9472406f0" />

Once decoded we can copy the output and paste it on the box on the website we had been given a link to

After submitting we will see 3 icons of which by clicking them we will begin to download the intel from the three countries *London, New York and Tokyo*. Once we done downloading and extracting we find that one of the files *Tokyo.IOC.Zip* is password protected, so we will keep this for later

<img width="892" height="262" alt="image" src="https://github.com/user-attachments/assets/6f9c11a7-2b6f-485f-97d4-748a70f95563" />

Going back to our question we are asked of a year an attacked happened, from the files extracted we can see a *.pdf* report

<img width="1303" height="495" alt="image" src="https://github.com/user-attachments/assets/a4856f51-a0f9-49c1-962d-dadae4ef9646" />

Opening it we find an attack happenned on a *GenX Financial Companyy* on 2017 which is a *Credit Reporting Agency* from the *US* 

<img width="651" height="263" alt="image" src="https://github.com/user-attachments/assets/9971d690-cf54-4d8c-8cd3-aa877bb3081e" />

*ANS: `GenX Financial, US, Credit Reporting Agency`*

2. ***According to the data breach summary, one of their critical assets was compromised, and they later discovered a vulnerability in one of their public-facing applications. What type of weakness was exploited to breach their network? ***

From the report we can see the vulnerability was on an *APache Struts Web Server* which was unpatched *CVE-2017-5638*. Using the CVE to look it up in google to better understand it , we come across an article by [Black Duck](https://www.blackduck.com/blog/cve-2017-5638-apache-struts-vulnerability-explained.html) 

<img width="1449" height="587" alt="image" src="https://github.com/user-attachments/assets/fe401222-5822-4acc-b47b-0a6f486f04ad" />

The article explains that the Struts is an open source Web Application that had problems intergrating to other frameworks making patching the application difficult. By this we can conclude that this was an ***Application vulnerability***

*ANS: `Application Vulnerability`*

3. ***How long did this breach go undetected? What was the Mean Time to Detect (MTTD)?***

From the *Incident Scope* we can see that the breah had lasted for *76 days* where the attackers infiltrated 48 databases

<img width="636" height="262" alt="image" src="https://github.com/user-attachments/assets/b2143300-a7ef-4f58-8ce7-4908f6fc90fb" />

*ANS: `76 days`*

4. ***What application was targeted by the attacker? What vulnerability was exploited, and where is this application located within the network?***

We already know that the Vulnerability was on *Apache Struts* and the *CVE* ;but going firther down on the report we see the location of the application

<img width="601" height="415" alt="image" src="https://github.com/user-attachments/assets/e971567f-458c-4d51-858e-eb1dbbef7c27" />

*ANS: ` Aoche Struts, CVE-2017-5638, ACIS`*

5. ***The attackers exfiltrated millions of records. How many consumer details were estimated to be exposed? How, and through which channel, was the data exfiltrated from the premises? ***

<img width="652" height="213" alt="image" src="https://github.com/user-attachments/assets/38970c8a-a1e9-46d6-9b60-1f7fe3e55d1d" />

We an aproximation of 148 million records exploited by the attacker.

<img width="653" height="202" alt="image" src="https://github.com/user-attachments/assets/9fa34abf-8fbf-42aa-9a14-383bc144b61f" />

The channels used by the attacker to exfiltrate the data stolen came from *encrypted chennels*

*ANS: `148 million, encypted`*

6. ***Later, during the investigation, a flaw was discovered in their ACIS code rendering system. What were these flaws?***

<img width="652" height="356" alt="image" src="https://github.com/user-attachments/assets/b23f5025-fd7c-4652-9aef-94bfdb2db117" />

From Incident Scope, it was mentioned that ACIS code has several vulnerabilities and that include IDOR and SQL Injection

*ANS: `SQL injection, Insecure Direct Object Reference`*

7. *** What file was inserted during the attack, and which country did the attack originate from?***

<img width="649" height="562" alt="image" src="https://github.com/user-attachments/assets/e1fddec1-887d-4d54-9c5e-1914466a9d65" />

We can see that a suspicious IP was found originating from *China* and due to the *SQL Injection* vulnerability the team discovered an unexpected file placed on the ACIS application

*ANS: `JSP, China`*

8. *** It is said that if a specific network security technique had been properly implemented, the attacker likely would have failed to accomplish their mission. What is this technique called?***

<img width="654" height="331" alt="image" src="https://github.com/user-attachments/assets/5c51d862-2a0c-4842-b437-80b78cac6c6d" />

































