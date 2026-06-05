---

title: "# time machine"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description
What was I last working on? I remember writing a note to help me remember...
You can download the challenge files here:
challenge.zip

# solution
```
wget https://artifacts.picoctf.net/c_titan/68/challenge.zip
unzip challenge.zip
cd drop-in/
cat message.txt
```
finally
```
┌──(root㉿Harsh)-[/home/jhagan/h/drop-in]
└─# git log
commit 705ff639b7846418603a3272ab54536e01e3dc43 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Sat Mar 9 21:10:36 2024 +0000

    picoCTF{t1m3m@ch1n3_b476ca06}
```

## conclusion  
### 1. Git keeps history — even if the file is changed or deleted  
- Git records every commit (change) with:  
- A unique commit hash  
- An author and timestamp  
- A commit message  

Even if the file with the sensitive information is later removed or modified, the data can still be recovered by checking older commits.  

### 2. git log can reveal secrets in commit messages  
- Commit messages should ideally describe the change, but in this challenge, the flag was stored in the commit message itself.  
- In real life, developers sometimes accidentally commit passwords, API keys, or secrets in commit messages — attackers can find them the same way you found the flag.  
