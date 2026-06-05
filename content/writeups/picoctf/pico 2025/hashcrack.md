---

title: "# hashcrack"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description
A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?
Access the server using nc verbal-sleep.picoctf.net 57819

# solution

```
┌──(root㉿Harsh)-[/home/jhagan]
└─# nc verbal-sleep.picoctf.net 57819
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash: password123
Correct! You've cracked the MD5 hash with no secret found!

Flag is yet to be revealed!! Crack this hash: b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
Enter the password for the identified hash: letmein
Correct! You've cracked the SHA-1 hash with no secret found!

Almost there!! Crack this hash: 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
Enter the password for the identified hash: qwerty098
Correct! You've cracked the SHA-256 hash with a secret found.
The flag is: picoCTF{UseStr0nG_h@shEs_&PaSswDs!_869e658e}
```

- crack the hashes by  [CrackStation](https://crackstation.net/)

# 🔍 1. Length of the Hash
Each hashing algorithm produces a fixed-length output, typically in hexadecimal.  

Hash Type	Length (Hex)	Length (Bits)  
MD5	32 characters	128 bits  
SHA-1	40 characters	160 bits  
SHA-256	64 characters	256 bits  
SHA-384	96 characters	384 bits  
SHA-512	128 characters	512 bits  
NTLM	32 characters	128 bits  
CRC32	8 characters	32 bits  

