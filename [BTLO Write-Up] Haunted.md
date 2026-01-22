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

https://miro.medium.com/v2/resize:fit:640/format:webp/0*ovPe5Gv2x_WjyHNn.png
















































