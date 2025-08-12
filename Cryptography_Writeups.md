### Task 12:
+ The message is encrypted using a repeating-key XOR cipher, which means each byte of plaintext is XORed with a key that repeats over the length of the message.

+ We also know that every message starts with:

`ORDER:`

+ This gives us a huge advantage because we can use it to determine the encryption key.

+ Convert everything to bytes:
`1c 1c 01 04 19 63 73 0f 31 35 2a 3a 38 6e 24 35 6b 3d 32 39 2b 6f 6b 0d 32 3c 22 24 3f 63 73
1a 0d 0c 30 2d 3b 2b 1a 29 2a 3a 38 28 2c 2f 22 2d 2a 11 2d 28 2c 31 20 2d 2d 2e 24 35 2e 60`

+ Extract key:
`ORDER`:  →  `4F 52 44 45 52 3A`

+ Since the cipher is XOR-based, we can retrieve the key by XORing the ciphertext bytes with the plaintext bytes.

+ Let’s take the first six ciphertext bytes:
`1c 1c 01 04 19 63`

+ And XOR them with “ORDER:” in hex:
`4F 52 44 45 52 3A`

`1C ⊕ 4F = 53
1C ⊕ 52 = 4E
01 ⊕ 44 = 45
04 ⊕ 45 = 41
19 ⊕ 52 = 4B
63 ⊕3A = 59`

+ So the repeating key is `“SNEAKY”` (53 4E 45 41 4B 59 in hex).

+ Decrypt the message:
Now that we have the key "SNEAKY", we XOR it with the entire encrypted message in repeating cycles.

+ By applying XOR decryption across the full message using the repeating key, we get:
`ORDER: Attack at dawn`. Target: `THM{the_hackfinity_highschool}`. 

### Task 30:

```
def decrypt(ciphertext):
    decrypted = []
    for i, c in enumerate(ciphertext):
        if c.isalpha():
            base = ord('A') if c.isupper() else ord('a')
            decrypted.append(chr((ord(c) - base - i) % 26 + base))
        else:
            decrypted.append(c)  # Keep non-alphabetic characters unchanged

    return "".join(decrypted)

# Given encrypted message
ciphertext = "a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm"

# Decrypt it
decrypted_message = decrypt(ciphertext)
decrypted_message
```

--> `THM{a_sm4ll_crypt0_message_to_st4rt_with_THM_cracks}`

### Task 31:

```
import math
from Crypto.Util.number import inverse, long_to_bytes

# Given values
n = 15956250162063169819282947443743274370048643274416742655348817823973383829364700573954709256391245826513107784713930378963551647706777479778285473302665664446406061485616884195924631582130633137574953293367927991283669562895956699807156958071540818023122362163066253240925121801013767660074748021238790391454429710804497432783852601549399523002968004989537717283440868312648042676103745061431799927120153523260328285953425136675794192604406865878795209326998767174918642599709728617452705492122243853548109914399185369813289827342294084203933615645390728890698153490318636544474714700796569746488209438597446475170891
e = 0x10001
c = 3591116664311986976882299385598135447435246460706500887241769555088416359682787844532414943573794993699976035504884662834956846849863199643104254423886040489307177240200877443325036469020737734735252009890203860703565467027494906178455257487560902599823364571072627673274663460167258994444999732164163413069705603918912918029341906731249618390560631294516460072060282096338188363218018310558256333502075481132593474784272529318141983016684762611853350058135420177436511646593703541994904632405891675848987355444490338162636360806437862679321612136147437578799696630631933277767263530526354532898655937702383789647510

# Step 1: Factorizing n using Fermat's method (since p and q are close)
a = math.isqrt(n)
if a * a < n:
    a += 1

while True:
    x2 = a * a - n
    x = math.isqrt(x2)
    if x * x == x2:
        break
    a += 1

p = a - x
q = a + x

print(f"p = {p}")
print(f"q = {q}")

# Step 2: Compute private key d
phi = (p - 1) * (q - 1)
d = inverse(e, phi)

print(f"d = {d}")

# Step 3: Decrypt the ciphertext
flag_number = pow(c, d, n)
flag = long_to_bytes(flag_number).decode()

# Step 4: Format the flag correctly
print("\nDecrypted Flag:", flag)
```
--> `THM{Just_s0m3_small_amount_of_RSA!}`

