### Task 14:
+ By checking the mail and try to send a pdf, we could see that cipher only accept docx or docm file.
+ We can generate a malicious docm file using this module of msf :

<img width="860" height="445" alt="image" src="https://github.com/user-attachments/assets/43540a31-8ed9-44aa-8c71-969106f42971" />


+ Then setting up the handler and send the malicious docm, we get a callback :

<img width="866" height="560" alt="image-1" src="https://github.com/user-attachments/assets/76284d1b-3993-4691-879d-badcf5144aad" />


+ We can see we are admin:
<img width="337" height="52" alt="image-2" src="https://github.com/user-attachments/assets/71a0934a-21c8-42c1-a790-58e9fc42f3b0" />


+ Then go on the admin desktop and catch the flag:
<img width="816" height="226" alt="image-3" src="https://github.com/user-attachments/assets/74913add-aa33-406e-adbf-e2e317a6b313" />


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
<img width="736" height="113" alt="image-4" src="https://github.com/user-attachments/assets/3646c206-9543-486f-b1f1-6743d01352cd" />


+ Then try to connect with evilwinrm, Administrator and Cipher can’t use winrm but others can.
+ Checked all groups and privs for the available users. And DarkInjector is in the admin group. 

<img width="820" height="580" alt="image-5" src="https://github.com/user-attachments/assets/543e5118-19a7-4b8e-b8b8-ff8ba7765768" />


+ So I can just go into the Administrator folder and grab the flag :
<img width="729" height="311" alt="image-6" src="https://github.com/user-attachments/assets/a855a033-5a60-4b55-9a5c-8c4f7e846cb6" />


### Task 16:
+ First of all we take the informations given in the mail:

<img width="465" height="163" alt="image-7" src="https://github.com/user-attachments/assets/9d0c11d8-0dad-48ae-adc0-9215f3f93c71" />


+ The only important thing here is the w10 x64 exe format, we’ll use these information with venom to craft a payload:

<img width="811" height="196" alt="image-8" src="https://github.com/user-attachments/assets/83cd8e9d-4c41-42c7-b0ef-280dee64c939" />


+ Then setup the handler in msf :

<img width="829" height="548" alt="image-9" src="https://github.com/user-attachments/assets/b61ac2c3-13ce-4a5d-bd3f-25c1a289e4cb" />


+ Then send the malicious exe, wait a bit and we got an admin shell:

<img width="811" height="125" alt="image-10" src="https://github.com/user-attachments/assets/1dd7ab79-4c35-4bb7-8c14-0db69f160905" />


+ We can just go check the flag from now:

<img width="819" height="537" alt="image-11" src="https://github.com/user-attachments/assets/a25e68db-71cb-41bb-b9dc-e27e36b7f773" />

### Task 37:
+ First the nmap scan reveal ssh and http
<img width="374" height="40" alt="image" src="https://github.com/user-attachments/assets/4401f0bf-27d0-4de2-97fe-0be1bdb0ef11" />
<img width="1900" height="526" alt="image" src="https://github.com/user-attachments/assets/da24b710-7f23-48ee-9695-3161792762b6" />
+ The landing page didn’t give anything interesting
<img width="1919" height="830" alt="image" src="https://github.com/user-attachments/assets/f9e84f34-92e2-4d5a-b3d7-35af1ed63de1" />
+ We found various directories, admin and backup are interesting.
<img width="969" height="626" alt="image" src="https://github.com/user-attachments/assets/b3f2c5f7-8a2d-440d-ab90-87bf651ac790" />
<img width="1916" height="976" alt="image" src="https://github.com/user-attachments/assets/5bb951a4-f9df-4224-9e83-bf9f490f4b2f" />
<img width="854" height="424" alt="image" src="https://github.com/user-attachments/assets/fb9c0c63-0507-49bb-b7c1-9c6c4e329787" />
+ Archive is password protected, tried to crack it with fcrackzip
<img width="1459" height="853" alt="image" src="https://github.com/user-attachments/assets/38f38ca7-7241-407d-88e8-36aa730f576d" />
<img width="651" height="109" alt="image" src="https://github.com/user-attachments/assets/85979c5e-7729-4102-be1e-fe56a8e58641" />
<img width="956" height="519" alt="image" src="https://github.com/user-attachments/assets/acf218e0-6db4-426b-a24c-3c0c32ef290c" />


