# EXPOSED

*Tools - GitTools, Nuclei, T1213.003*

*Difficulty - Easy*

## SCENARIO

***You got a mail.***

***“Congratulations on progressing to the next stage of the selection process. The next stage will re-quire you to solve the following test as part of our technical interview. What are these websites exposing?”***
***- Hiring Manager***

***Website A : http://notwp.sbt***
***Website B : http://wp.sbt***
***

**QUESTIONS**

1. ***Use GitTools, where is the git directory located in website A?***

To begin we need to understand what *GitTools* is, this is a collection of tools that a penetration tester or a defender can use to reconstruct sodurce code from an exposed `.git` folder. On the labs desktop under GutTools we see that we are given 3 tools *Dumper - which is used for dumping a git repository from a webservers which do not have directory listing enabled*, *Extractor - used to extract results from the dumper*, *Finder - Used to locate git repository of the web application*

*Going over to the *Dumper* folder we see the `gitdumper.sh` script and a *README.md which will gie use instructions of using the tool

<img width="790" height="787" alt="image" src="https://github.com/user-attachments/assets/3b833a3c-bcc3-4f97-8cfc-5922780c8689" />

Now we need to open our teminal and go to the folder that contains the `gitdumper.sh` script and run the script using *Website A* from the description which will be

`bash gitdumper.sh bash gitdumper.sh http://notwp.sbt/.git/ dest-dir` 
where; - `bash gitdumper.sh` - runs the script as its not executable but that can be done if we use `chmod +x gitdumper.sh`
       -  `http://notwp.sbt/ .git/` - This is the target website and we are adding `.git` to exploit and see if it's available.
       - `dest-dir` - This is a random name for a folder where the repository will be dropped

<img width="983" height="611" alt="image" src="https://github.com/user-attachments/assets/bee8f0da-71cd-4cdf-99f3-1042b0529eec" />

We can noe see that we have succefully found an exposed `.git repository` and the script is proceeding to download it on our folder.Once done we need to navigate using some Linux fu to the `.git` folder and there we will have found the dumped files and folders of the repository

<img width="736" height="468" alt="image" src="https://github.com/user-attachments/assets/1dbe5a53-a700-4b95-80c9-dce916f0e96a" />

*ANS: `/.git`*

2. ***What is the springboot password?***

In order to the password we are going to use the other tool which is the *Extractor*. Following the steps as above question we also find a *README.md* file which contains instructions of the usage

<img width="747" height="773" alt="image" src="https://github.com/user-attachments/assets/6bafa07a-e86a-43b1-b54a-19549f15c898" />

The code to use will be as follows `bash extractor.sh /home/ubuntu/Desktop/GitTools/Dumper/dest-dir/ /home/ubuntu/Desktop/GitTools/Extra` where we need to run the script in the folder containing the `.git` folder which we just dumped and direct it to our folder of choice so we can extract the remaining incomplete repositories

<img width="1055" height="452" alt="image" src="https://github.com/user-attachments/assets/a20b2370-9ac7-4c14-bc4e-5b1aa1e2e52e" />

Once the repo is dumped on our folder in this case is `repo-dump` we will see 3 folders inside, going in on the in this sequence *exrtracted_folder -> src -> main -> resources ->* we come by a file in *ASCII Text* where by using the `cat` command we see information of the user and password.

<img width="1480" height="565" alt="image" src="https://github.com/user-attachments/assets/4ac0c824-209c-4d0b-bbf0-cbcafbe1f334" />

Repeating the same in all 3 folders we can come to a comclusion that the springboot password is `noP@sX3211`

*ANS: `noP@sX3211`*

3. ***Website B is using Wordpress. Which nuclei template is useful in detecting exposed git directory?***

On the desktop is a folder called *nuclei* buut what is it? 



































































