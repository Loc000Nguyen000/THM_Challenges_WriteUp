### Task 14:
+ By checking the mail and try to send a pdf, we could see that cipher only accept docx or docm file.
+ We can generate a malicious docm file using this module of msf :

![alt text](image.png)

+ Then setting up the handler and send the malicious docm, we get a callback :

![alt text](image-1.png)

+ We can see we are admin:
![alt text](image-2.png)

+ Then go on the admin desktop and catch the flag:
![alt text](image-3.png)

### Task 15:
+ First I check the hashes i can extract from the mimikatz extract (we want NTLM hashes):

```
DUMP\ByteReaper
43034346035d7a24b1eaa1c82acaef3e

DUMP\NullPhantom
75b216fc8d12fbd0124d0c0865118e4d

DUMP\specter
2c3e48bac1b65c52fc0fb0cd70eaf3aa

DUMP\Cipher
6786c31df90f9568d4ca2480affeea9a

DUMP\Administrator
2dfe3378335d43f9764e581b856a662a

DUMP\ShadowPacket
5fe2eb8c75c48179bce6025e70859e47

DUMP\GhostTrace
4820e4687d0ed2845328aa492dc0b33c

DUMP\DarkInjector
3a659a65d9c16d40a28684995782458f
```

+ Then run nmap to check for winrm:
![alt text](image-4.png)

+ Then try to connect with evilwinrm, Administrator and Cipher can’t use winrm but others can.
+ Checked all groups and privs for the available users. And DarkInjector is in the admin group. 

![alt text](image-5.png)

+ So I can just go into the Administrator folder and grab the flag :
![alt text](image-6.png)

### Task 16:
+ First of all we take the informations given in the mail:

![alt text](image-7.png)

+ The only important thing here is the w10 x64 exe format, we’ll use these information with venom to craft a payload:

![alt text](image-8.png)

+ Then setup the handler in msf :

![alt text](image-9.png)

+ Then send the malicious exe, wait a bit and we got an admin shell:

![alt text](image-10.png)

+ We can just go check the flag from now:

![alt text](image-11.png)