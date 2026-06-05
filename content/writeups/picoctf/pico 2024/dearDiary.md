---

title: "dearDiary"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description
If you can find the flag on this disk image, we can close the case for good!
Download the disk image here.

##  🧠 Step-by-Step Breakdown:
### 📥 1. Download and Extract the Disk Image
```
wget https://artifacts.picoctf.net/c_titan/63/disk.flag.img.gz
gunzip disk.flag.img.gz
```

###  🕵️ 2. Open with Autopsy (Forensic Tool)
- Autopsy is a digital forensics platform that lets you:

- Mount and explore disk images

- Recover deleted files

- View metadata, file content, etc.

###  📂 3. Explore the Root Directory
Once loaded into Autopsy, the disk image showed 3 files in the root directory:

force-wait.sh

innocuous-file.txt

its-all-in-the-name

At first, these look ordinary — especially innocuous-file.txt.

### 🧩 4. Key Clue: "its-all-in-the-name"
This filename is a hint:

The name of the file matters.

So even if innocuous-file.txt seems unimportant, it's probably significant because of its name.

###  🔎 5. open the disk.flag.img in vs code and also download hex editor extension
- search for innocuous
- and you find 14 file the the bits give you the flag in part

###  🧵 6. Reconstructing the Flag
By piecing together the ASCII fragments found in different instances of innocuous-file.txt, you assembled the full flag:


picoCTF{1_533_n4m35_80d24b30}
(You might have seen the full flag in your actual run — this is just the partial given in your write-up.)

###  🎓 What You Learned From This CTF
Skill/Concept	What You Gained
🧠 Disk Forensics	How to inspect and analyze raw disk images.
🧱 File Fragmentation	Some files are split across disk sectors and can appear multiple times.
🧩 Pattern Recognition	You practiced identifying and assembling partial data into a coherent result.
🔍 Clue Interpretation	The filename its-all-in-the-name gave you a useful nudge — CTFs often hide clues this way.
🛠️ Autopsy Usage	You got hands-on with a real forensic analysis tool used by professionals.

🏁 Final Takeaway:
Even the most “innocent” looking files (like innocuous-file.txt) may hide secrets — especially when forensic clues are scattered and fragmented across a disk.z