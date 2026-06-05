---

title: "mob Psycho"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
## Description
Can you handle APKs?
Download the android apk here.

# Solution
```
wget  https://artifacts.picoctf.net/c_titan/142/mobpsycho.apk
unzip mobpsycho.apk
grep -iR picoCTF *
strings mobpsycho.apk | grep flag
```

  
## output
res/color/flag.txtUT

## 🔍 Breakdown: grep -iR picoCTF *

grep — Command-line tool to search for text patterns in files.

-i — Makes the search case-insensitive (picoctf, PicoCTF, PICOCTF, etc.).

-R — Recursively searches all directories and subdirectories starting from the current one.

picoCTF — The string you're searching for (often the format of a flag in Capture The Flag (CTF) challenges).

* — Wildcard meaning "all files and directories" in the current directory.

## 🔍 Breakdown:  strings mobpsycho.apk | grep flag

strings — Extracts printable ASCII strings from a binary file.

mobpsycho.apk — The APK file you're analyzing (likely from a CTF or reverse engineering task).

| — Pipes the output of strings into...

grep flag — Searches for any line containing the word "flag" (case-sensitive).



`cat res/color/flag.txt`

7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f37343664666133397d

`cat res/color/flag.txt | xxd -r -p`

picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_746dfa39}

# 🧠 What You Learned From This CTF:
#### 🔧 1. APKs are just zip archives.

#### 🔍 2. Pattern Searching Techniques:
Using grep -iR to search recursively through all extracted files for common flag patterns (picoCTF, flag, etc.).

#### 3. Hex Decoding Skills:
You encountered a hex-encoded flag and used:
xxd -r -p
