# The Planet's Prestige

***Tools: Email Client, Text Editor***

***Difficulty: Easy***

## Scenario

***CoCanDa, a planet known as 'The Heaven of the Universe' has been having a bad year. A series of riots have taken place across the planet due to the frequent abduction of citizens, known as CoCanDians, by a mysterious force. CoCanDa’s Planetary President arranged a war-room with the best brains and military leaders to work on a solution. After the meeting concluded the President was informed his daughter had disappeared. CoCanDa agents spread across multiple planets were working day and night to locate her. Two days later and there’s no update on the situation, no demand for ransom, not even a single clue regarding the whereabouts of the missing people. On the third day a CoCanDa representative, an Army Major on Earth, received an email.***

**QUESTIONS**

1. ***What is the email service used by the malicious actor?***

To get to find the email service used we need to use a tool called [Sublime Text](https://www.sublimetext.com/) which is a text and source editor and has the advantage of syntax highlighting enabling use to clearly see the email headers. You may ask what an Email Header is, well this is a block of hidden metadata which contains technical details of an emails journey.

Once opened we need to go to the header caller `Received` this header adds automatically after an SMTP (Simple Mail Transfer Protocol) server accepts an email. This shows all servers the email passed though before reaching its final destination.

<img width="858" height="225" alt="image" src="https://github.com/user-attachments/assets/54200c83-91da-4017-9a5a-5fa5e46171a2" />

Looking at the header we see only one received header comming from a local host `emkei.cz` of IP `93.99.104.210`. This confirms the service that the malicious actor used.

*ANS:`emkei.cz`*

2. ***What is the Reply-To email address?***

The Reply-to header contains the address to which the receipient will respond to.

<img width="864" height="146" alt="image" src="https://github.com/user-attachments/assets/b3fed138-3ce7-4b4d-8e2a-f1f01f79c6a6" />

*ANS: ` negeja3921@pashter.com`*

3. ***What is the filetype of the received attachment which helped to continue the investigation?***

Attackers tend to manipulate filetypes by hidding malicious scripts that get executed when opened. 

Looking at our email, we see a file at the bottom named   `PuzzleToCoCanDa.pdf`. Now we need to be careful while handling malicious files, so we need to right click and hit save so we can have it on our device without executing it. Once saved we go ahead and use a tool called **ExifTool** which is an open source software used to extract all metadata of a file

On the terminal where the document is we can execute exiftool by using `exiftool PuzzleToCoCanDa.pdf`

<img width="975" height="374" alt="image" src="https://github.com/user-attachments/assets/229de7d2-a8c9-49c1-b469-dda355e76c3d" />

And there we see that the file is not a PDF but a ZIP file.

*ANS: `.ZIP`*

3. ***What is the name of the malicious actor?***

Now that we know the file is a `.zip` we can go ahead and unzip the file 

<img width="796" height="114" alt="image" src="https://github.com/user-attachments/assets/22797e14-ec1a-47d4-a6e6-eeb11310629c" />

This reveals to use 3 files, going over each we can find a hint in the second file **GoodJobMayor**

<img width="1169" height="685" alt="image" src="https://github.com/user-attachments/assets/1571e119-c977-4230-93d9-46c9be1c7ae8" />

So what we need to do is run *exiftool* on the file itself and after that we can see the name of the malicous actor on the metadata 

<img width="654" height="295" alt="image" src="https://github.com/user-attachments/assets/561d60ca-0afe-4a64-aa2c-cf8f08f97aaf" />

*ANS: `Pestero Negeja`*

4. ***What is the location of the attacker in this Universe?***

This was a tricky one, on the files that we unzipped one gave a hint

<img width="905" height="641" alt="image" src="https://github.com/user-attachments/assets/643f93c4-dc06-434d-a6bf-d57ffec35e77" />

Openning the document **Money.xlsx** we can see some info we are given but the trick is looking down we can see 2 sheets, we click on the sheet named **Sheet 3** and we can see nothing. To be able to solve this we need to select the whole cells and click on the color highligher and choose any color, this reveals a hidden message in a cell

<img width="1383" height="839" alt="image" src="https://github.com/user-attachments/assets/c655524c-9573-4b51-a609-638442ba7479" />

Copying the base64 and pasting it on CyberChef to decode it we then are able to find the location.

<img width="1400" height="679" alt="image" src="https://github.com/user-attachments/assets/56d03af0-d77a-4bad-bfc7-3e3970483120" />

*ANS: `The Martian Colony, Beside Interplanetary Spaceport.`*

5. ***What could be the probable C&C domain to control the attacker’s autonomous bots?***

The C&C here stands for Comand and Control, this could be a server that the attacker uses to store hidden data or download commands, or even communicating with a dropped malware. Here we g back to *Sublime Text* and whe need to know where the replies are going to and looking at the **Reply-To** header we see a mail `negeja3921@pashter.com` and from this we can then confirm that the attackers C2 domain is `pashter.com`

<img width="670" height="154" alt="image" src="https://github.com/user-attachments/assets/4f8730e5-1d6c-4be6-9856-14089e421b23" />

*ANS: `pashter.com`

















   
