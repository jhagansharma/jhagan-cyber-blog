---

title: "# Super SSH.md"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description
Using a Secure Shell (SSH) is going to be pretty important.
Can you ssh as ctf-player to titan.picoctf.net at port 52017 to get the flag?
You'll also need the password 6dd28e9b. If asked, accept the fingerprint with yes.

# solution
```
 ssh -p 52017 ctf-player@titan.picoctf.net
```
- ssh → Secure Shell, used to remotely log into another computer/server.
- -p 52017 → Specifies a non-default port (52017 instead of the usual 22).
- ctf-player@titan.picoctf.net → Says:
- username: ctf-player
- server: titan.picoctf.net

after enter psd 
```
┌──(root㉿Harsh)-[/home/jhagan/h/drop-in]
└─#  ssh -p 52017 ctf-player@titan.picoctf.net
The authenticity of host '[titan.picoctf.net]:52017 ([3.139.174.234]:52017)' can't be established.
ED25519 key fingerprint is SHA256:4S9EbTSSRZm32I+cdM5TyzthpQryv5kudRP9PIKT7XQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[titan.picoctf.net]:52017' (ED25519) to the list of known hosts.
ctf-player@titan.picoctf.net's password:
Welcome ctf-player, here's your flag: picoCTF{s3cur3_c0nn3ct10n_5d09a462}
Connection to titan.picoctf.net closed.
```

flag: picoCTF{s3cur3_c0nn3ct10n_5d09a462}
