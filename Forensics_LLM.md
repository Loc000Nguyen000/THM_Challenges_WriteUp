### Task 24:
+ Looking at the PCAP and following the NFS stream suggested in the task, you find the a secrets archive was transferred.

+ Extracting the bytes of it you find a password protected .zip archive.

+ Running strings on the PCAP you find an md5 hash named "Archive password"

+ Putting the md5 into https://crackstation.net/ returns avengers as the password.

+ Once unzipped you get a PNG of a QR code which shows the flag when scanned.
<img width="2182" height="1596" alt="image" src="https://github.com/user-attachments/assets/3bc76701-79a9-4cc7-835d-8a85edda632f" />

<img width="1288" height="100" alt="image-1" src="https://github.com/user-attachments/assets/dd6cee9c-43a1-48a0-a9af-3831c4152ee1" />

<img width="2410" height="1062" alt="image-2" src="https://github.com/user-attachments/assets/2fb6d8ad-a6d5-4511-9886-d654e7488731" />

<img width="200" height="200" alt="image-3" src="https://github.com/user-attachments/assets/bb524a60-df57-4a20-b4da-0ecbadce519c" />


### Task 25:
+ We found the web shell in /var/www/html/CMSsite-master/img/images.php
+ Decodifing the log of apache in other_vhost
<img width="1881" height="694" alt="image-4" src="https://github.com/user-attachments/assets/55d427db-9266-4a74-881b-ef6ac0d1b259" />

```
echo "d2hvYW1pCg==" | base64 -d   # whoami
echo "bHMK" | base64 -d           # ls
echo "ZWNobyAnVEhNe3N1cDNyXzM0c3lfdzNic2gzbGx9Jwo=" | base64 -d # echo 'THM{sup3r_34sy_w3bsh3ll}'
echo "aWZjb25maWcK" | base64 -d   # ifconfig
echo "Y2F0IC9ldGMvcGFzc3dkCg==" | base64 -d # cat /etc/passwd
echo "aWQK" | base64 -d           # id
```

### Task 26:
<img width="947" height="418" alt="image-5" src="https://github.com/user-attachments/assets/9c4ef415-ab91-404b-9331-21c3a2666c3b" />

--> File write data to `/proc/cipher_bd`

+ Seeing the strings of module `spatch.ko` that i saw cipher module charges:

<img width="935" height="546" alt="image-6" src="https://github.com/user-attachments/assets/ea3bd89a-75d9-4f5e-bb8f-e3ae6938eff0" />

+ if we convert this secret of hex decoder:

<img width="856" height="622" alt="image-7" src="https://github.com/user-attachments/assets/a56a3ca9-9940-4cc3-a84b-ecd4fc95bfda" />

### Task 27:
+ We can see some cronjobs about vnc and creating persistence:
<img width="914" height="602" alt="image-8" src="https://github.com/user-attachments/assets/1b721376-1d35-4ad7-bbc7-d1590338ee41" />

<img width="1693" height="2536" alt="image-10" src="https://github.com/user-attachments/assets/ac1856dc-2914-4f89-8100-7914114c5a56" />

--> Get the flag `d0wn.h1dd3nd00r.n3t`

### Task 21:
+ Run command but notice with flag.txt priv "root" so we will run "sudo cat" to read it

<img width="1920" height="1080" alt="image-11" src="https://github.com/user-attachments/assets/6dd94659-af25-43f5-8034-2134224bb9d1" />

### Task 22:
+ Just conplemted this by trying simple prompt injection on tricking the AI to not adhere to it's rules.

<img width="1992" height="2590" alt="image-13" src="https://github.com/user-attachments/assets/2e34eb61-c183-4490-b992-b1e5d0950083" />
