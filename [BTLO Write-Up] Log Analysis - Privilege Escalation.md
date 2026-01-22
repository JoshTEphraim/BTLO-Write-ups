# Log Analysis - Privilege Escalation 

*Tools - Text Editor*

*Difficulty - Easy*

*OS - Windows/Linux*

## Scenario

***A server with sensitive data was accessed by an attacker and the files were posted on an underground forum. This data was only available to a privileged user, 
in this case the ‘root’ account. Responders say ‘www-data’ would be the logged in user if the server was remotely accessed, and this user doesn’t have access to the data. 
The developer stated that the server is hosting a PHP-based website and that proper filtering is in place to prevent php file uploads to gain malicious code execution. 
The bash history is provided to you but the recorded commands don’t appear to be related to the attack. Can you find what actually happened?***

**QUESTIONS**

1. ***What user (other than ‘root’) is present on the server?***

After we unzip the downloaded file we see a file by the name `bash_history` this is a file that holds all the commands a user has entered in the terminal. Openning it we need 
to look for any user apart from root 

<img width="620" height="161" alt="image" src="https://github.com/user-attachments/assets/95a17dc8-1ce9-45b3-b040-715da097e20f" />

We find a change directory `cd` command leading to the home of the user **Daniel**

*ANS: `Daniel`*

2. ***What script did the attacker try to download to the server?***

If a user wants to download anything from the internet on the terminal, the command to be used is mostly `wget` (World Wide Web Get) which is a free command-line
utility for non-interactivr downloading of files from the internet. We see this command in the bash_history file

<img width="781" height="191" alt="image" src="https://github.com/user-attachments/assets/c9c414e3-7e50-4c71-b261-d0ea3ab94fa4" />

We can see a script was downloaded and to be able to tell its a script, they all end with the `.sh` extension

*Ans: `linux-exploit-suggester.sh`*

3. ***What packet analyzer tool did the attacker try to use?***

A packet analyzer which ia also known as a *Packet Sniffer* are tools used to analyze network problems in order to detect network intrusion attempts, detect network misuse by internal or external users. Examples include *WireShark, TCPDUMP, Scapy...etc*

Looking at the bash_history we can only see one command used to execute a packet sniffer

<img width="704" height="183" alt="image" src="https://github.com/user-attachments/assets/4a098252-94f0-4c46-8ddc-909b8751e57c" />

*ANS: `tcpdump `*

4. ***What file extension did the attacker use to bypass the file upload filter implemented by the developer?***

Looking on the very last command we can see the attacker tried to remove a file `x.phtml`. This is possible the extension the attacker used to bypass the upload filter
as we can assume the developer might have blocked certain extensions as `.php` and by using `.phtml` this might slip through the resriction and the server will still have 
executed it as a **PHP** file and after that the attacker deleted the file.

<img width="631" height="170" alt="image" src="https://github.com/user-attachments/assets/ab298ddb-eee1-4994-a822-5cebe58aaedf" />

*ANS: `.phtml`*

5. ***Based on the commands run by the attacker before removing the php shell, what misconfiguration was exploited in the ‘python’ binary to gain root-level access? 1- Reverse Shell ; 2- File Upload ; 3- File Write ; 4- SUID ; 5- Library load***

The attacker exploited a misconfigured SUID bit on the python binary. The command giveaway is `./usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'`The SUID bit means a program runs with the file owner’s privileges (here: root), not the attacker’s.Because python was SUID-root, running it allowed the attacker to spawn a root shell using /bin/sh -p.

<img width="768" height="184" alt="image" src="https://github.com/user-attachments/assets/3ac9a887-b199-48f3-942e-084c4fedacfa" />

*ANS: `4`*






















