# Reverse-Engineering

<h2>Description</h2>
Project consists of multiple reverse engineering tasks looking at various files through IDA pro and RELYZE 
<br />

<h2>Languages and Utilities Used</h2>

- <b>IDA pro</b>
- <b>RELYZE</b> 


<h2>Environments Used </h2>

- <b>Amazon WorkSpace</b>

<h2>File1732</h2>

After looking at the code for file1732.exe, we can assume the code starts by running the program. It then prompts the user to enter a password using call, printf, which is later followed by fgets and getchar. If the user inputs the proper password, they will be transferred to this week's internet relay chat (IRC) info. What is in the IRC could be found in the assembly where the string was located. From looking at the string, we were able to assume the password potentially could be ThisPasswordSux. I later found a call putchar, which I can assume is the reason we are able to see the strings in human language. 

After running the file on my virtual machine, I can confirm that the password is ThisPasswordSux!. The password produced a statement, ""This week's IRC infor: icr.somber.net #ghost" and then disappeared once you pressed enter

After using puTTy to look into the IRC channel, I was able to observe a conversation between two users, @mama and @f8. @mama appears to have use a Russian term in a section of the conversation, but they are mainly speaking in English. The two users are having a discussion on targets in the US, money exchange, and wiping computers. See full discussion below. I believe that this exchange is discussing a potential cyber attack on potential targets, but we would need more information to be sure. Based on the conversation, the potential target is Chicago. The discuss a "Chicago Lncd n park payd." 



<h2></h2>
