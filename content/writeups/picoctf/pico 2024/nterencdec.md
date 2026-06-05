---

title: "# neterencdec"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

# Description
Can you get the real meaning from this file.
Download the file here.

# solution
Base64-decoding

```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2024/Cryptography/interencdec]
└─$ cat enc_flag               
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==
The padding characters (=) at the end reveals that this is likely base64-encoded data.
```
Let's decode it with base64:
```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2024/Cryptography/interencdec]
└─$ cat enc_flag | base64 -d
```
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ=='  

Still base64-endoded but in python byte-format.  
Another round of decoding:  
```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2024/Cryptography/interencdec]
└─$ echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ==" | base64 -d
```
wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}  

Now this looks like a rotation cipher like Caesar or ROT13. The caesar cipher rotates 3 positions whereas ROT13 rotates 13 positions.

Get the flag - caesar tool solution  
We can try to bruteforce the cipher with the caesar tool from the bsdgames package.  
The tool uses English letter frequency statistics to crack the cipher. Install it with `sudo apt install bsdgames` if needed.  
```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2024/Cryptography/interencdec]
└─$ echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ==" | base64 -d | caesar
```
picoCTF{caesar_d3cr9pt3d_f0212758}

Get the flag - python script solution



## The caesar tool tries all 25 possible shifts, using English letter frequencies to guess the correct one.