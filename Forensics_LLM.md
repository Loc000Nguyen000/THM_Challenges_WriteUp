### Task 24:
+ Looking at the PCAP and following the NFS stream suggested in the task, you find the a secrets archive was transferred.

+ Extracting the bytes of it you find a password protected .zip archive.

+ Running strings on the PCAP you find an md5 hash named "Archive password"

+ Putting the md5 into https://crackstation.net/ returns avengers as the password.

+ Once unzipped you get a PNG of a QR code which shows the flag when scanned.
![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)

### Task 25:
+ We found the web shell in /var/www/html/CMSsite-master/img/images.php
+ Decodifing the log of apache in other_vhost
![alt text](image-4.png)

```
echo "d2hvYW1pCg==" | base64 -d   # whoami
echo "bHMK" | base64 -d           # ls
echo "ZWNobyAnVEhNe3N1cDNyXzM0c3lfdzNic2gzbGx9Jwo=" | base64 -d # echo 'THM{sup3r_34sy_w3bsh3ll}'
echo "aWZjb25maWcK" | base64 -d   # ifconfig
echo "Y2F0IC9ldGMvcGFzc3dkCg==" | base64 -d # cat /etc/passwd
echo "aWQK" | base64 -d           # id
```

### Task 26:
![alt text](image-5.png)

--> File write data to `/proc/cipher_bd`

+ Seeing the strings of module `spatch.ko` that i saw cipher module charges:

![alt text](image-6.png)

+ if we convert this secret of hex decoder:

![alt text](image-7.png)

### Task 27:
+ We can see some cronjobs about vnc and creating persistence:
![alt text](image-8.png)

![alt text](image-10.png)

--> Get the flag `d0wn.h1dd3nd00r.n3t`

### Task 21:
+ Run command but notice with flag.txt priv "root" so we will run "sudo cat" to read it

![alt text](image-11.png)

### Task 22:
+ Just conplemted this by trying simple prompt injection on tricking the AI to not adhere to it's rules.
![alt text](image-13.png)