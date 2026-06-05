---

title: "# Commitment Issues"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

`ls -la`
This shows .git exists → proof it’s a Git repo.

Check commit history
```
┌──(root㉿Harsh)-[/home/jhagan/drop-in]
└─# git log
commit 8dc51806c760dfdbb34b33a2008926d3d8e8ad49 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:06:17 2024 +0000

    remove sensitive info

commit 87b85d7dfb839b077678611280fa023d76e017b8
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:06:17 2024 +0000

    create flag
```
This shows a list of commits.
You notice one commit message says "create flag" — that’s when the flag was added.

Go back to that commit
Copy its commit hash (like e720dc26a1a55405fbdf4d338d465335c439fb3e) and run:

```
git checkout e720dc26a1a55405fbdf4d338d465335c439fb3e
```
This switches the files to how they looked at that time.

Read the file

`cat message.txt`
Boom — the flag is there:


picoCTF{s@n1t1z3_ea83ff2a}


# What you learned
- Git remembers everything, even if files are changed or deleted.
- You can travel back in time to old commits using:
- git log → see commit history
- git checkout <commit_hash> → go to that version
- In security/CTFs, you can use this to recover accidentally exposed secrets like flags, passwords, or API keys.