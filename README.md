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

<p align="center">
Conversation<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions/31028/s265833/IRC%20convo.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T220145Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=3cf8f0b2aa02e9c172ad15514a279e8ca0d88a704e6b6b65505e40ce91fd4498
<br />
<h2>File2544</h2>

After analyzing file2544.exe and file3212.exe in Relyze and ida pro, I can see that there are similarities. The only difference between the code for file2544 is that the password is hidden. I noticed there was a new call, strlen, which hides code. File3212.exe didn’t have this call. In Relyze, file3212.exe has call, releaseinfo, while the new file, file2544.exe does not. On top of that, I found the obfuscated key. File2544.exe had values obfuscated unlike file3212.exe. See photo below.

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions/31028/s265985/key%20obfuscated.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T220805Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=a390b1c2044a8320e089a8d7320fb73647228028302488e2fe3a4a1b1a981294
<br />

Looking at the difference between the files, I was able to find the XOR key that obfuscated the password. The XOR key is 41 hex. After using this key and running it through a XOR decoder, I was able to determine the password is PasswordStillSux.

Xor key=41
Values start at 0x11
password=PasswordStillSux


After running the file, I found this message…
This week's IRC info: irc.somber.net #nymeria
Press [Enter] to continue . . .
It is another chat found in the IRC. However, we would need to get into the chat to see what is being said.




After taking a peek into the irc channel of the new file, I was able to find some suspicious activity between two users, VladThe Destroyer and Alexi. They were discussing dropping an exploit, but that was all I was able to gather because they took note of a new person in the chat. Russian dialogue came after that. I disconnected and exited the irssi. See below for conversation.

I would suggest looking into the link sent from VladTheDestroyer. This would give us a look into the potential exploit or what they are trying to do by looking at an example..

<p align="center">
IRC Chat<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions/31028/s266058/new%20irc.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221217Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=aab2327a114ef9d5111769b8d71884dafe1db844d1882d07386c50862cb57067
<br />

Static Analysis

Start by looking into the strings window. Here, you can find human text that can give hints to what the assembly is trying to accomplish.This file looks to be dealing with password input in order to access IRC information.

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544.exe%20static%20analysis.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=5540fba9242958fec21cff42ec4648b1403fcf5958fb2e10ef9fca3353a6b2af
<br />

After clicking on the highlighted enter password string, it takes us to the specific subroutine involved. This would be sub_401340. Clicking on the specific subroutine will show the hex-a graph of the process. 

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544%20sub%20password.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=782c9494b0cef6f1f4cb0d5a60e1e6d883a3cbb0eab938d19d1059f33fe30d72
<br />

Using call printf, fgets and getchar are responsible for prompting the enter passwords. Call sub_401394 is responsible for the xor password. Call sub_401607 is responsible for testuserpassword.  

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544%20enter%20pass.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=c189f6c4fdb4942155a724621c6492db7ea66505e6a035db3a43128bcbc19cc3
<br />
  
The password is encrypted using an xor key. The key can be found in eax Local_0x33

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544%20xor.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=dd5f6c408216524a49643d3c4bf30f2ee4367986d02bf1647521122414ebd514
<br />
  
By following the flow of the graph, we can then determine that the password is located in sub_401394.

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2455.exe%20password%20values.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=3704aa7fd1738a8fb2fd37f8d7bbf66b0372d908acdf65fbc6432e39730fcaa1
<br />
  
This graph shows values stored in eax. Using the call strlen with value 10h in the cmp, we can guess that the password contains 16 characters since 10h is equal to 16 characters. Any password that is not 16 characters is automatically given the wrong password and taken back to the beginning of the process

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544%2016characters.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=c69224d1d45dc59a50faec35760aba79816e7d84b5e8feec6bdc048dcbb1750f
<br />
  
<h2>File3666</h2>

<b1>Static Analysis File3666</b1>

This assembly code for file3666.exe is very similar to the one we looked at in file2544.exe. 

Start by looking into the strings window. Here, you can find human text that can give hints to what the assembly is trying to accomplish.This file looks to be dealing with password input in order to access FTP information. 

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file2544.exe%20static%20analysis.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=5540fba9242958fec21cff42ec4648b1403fcf5958fb2e10ef9fca3353a6b2af
<br />

After clicking on the highlighted enter password string, it takes us to the specific subroutine involved. This would be sub_401340. Clicking on the specific subroutine will show the hex-a graph of the process. 

Using call printf, fgets and getchar are responsible for prompting characters for enter in passwords. The password is encrypted using an xor key. The key can be found in eax Local_0x19. Call sub_40168C is responsible for the test user password. Call sub_401394 is responsible for the xor password


<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/file3666%20calls.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=e5056f0e35ee01cf015206166bc2e569275930b4d71d9fcd018be42a086624a3
<br />
<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/3666%20xor.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=5aa9566d3c6864b60383a260fe83ac012a2e5d8b6de50571abba58cdeab75353
<br />

By following the flow of the graph, we can then determine that the password is located in sub_401394. However, unlike file 2544.exe, we are unable to determine the length of the password using the call strlen. This assembly code lacks this call, which does not automatically reject a password if it is not the proper length.  The branch now has to do a lot more work before accepting or rejecting the password.


<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/3666%20different.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=b4f02e8e9fe5af7e23171580e0fb2a2e0cc1f670b2cd74ee0e1563b2086a87ca
<br />
<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/3666%20password.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=144a3b6c0de7740d80ad33792605c802e14e4ef834be5c5f893e083a88b0f89b
<br />
  
The password seems to be obfuscated again in sub_40144F. This subroutine involves values moving all over. I think this is where the second xor occurs.


<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266142/second%20xor%20file3666.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=de1e9296bd41fd4418505a4ff3236f81d36abd2f0a072db8f3389a2fe6402246
<br />

If the correct password is imputed, then the "This week’s FTP info" will show. Whatever is hidden here can be shown by the values in Sub_4014AD. Like the password, the message is encrypted using an xor. The key is saved in the edx Local_0x50, but subtracted the value of 1 (Sub edx 0x1)

<p align="center">
Assembly<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266110/3666%20ftp.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T221437Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=ae621cd0605927128a19b09e1602ead9908ef257b944cb409512da500d201d8e
<br />

Relyze confirmed my findings. Relyze for file3666.exe showed that in comparison there was no strlen call. This made me realize the code only had two branches it could use. The correct or the incorrect password. The code would go through more work for accepting the password. However, I am slightly confused what happens with the xor key of the ftp values with sub edx, 0x1.

After running the file through Ida's debugger feature, I was able to find the section of assembly responsible for hiding the password. The password was obfuscated twice. Adding a breakpoint to the cmp call in the second obfuscation showed me the password. The password is SuxPasswordStill.

After running the program i found this message:


<p align="center">
FTP Chat<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/266295/ftp.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240229T222727Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240229%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=2ecdb815642237a3f23ecda1f3b2ef5c08c67695510ff2ac8c6d9b7b60b1574b
