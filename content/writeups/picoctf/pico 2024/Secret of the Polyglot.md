---

title: "# Secret of the Polyglot"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

### The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?

# Solution
```
1.  wget https://artifacts.picoctf.net/c_titan/9/flag2of2-final.pdf
2. ls
3. convert flag2of2-final.pdf flag2of2-final.png
4. open flag2of2-final.png
```
picoCTF{f1u3n7_

```
5. apt install poppler-utils
6.  pdftotext flag2of2-final.pdf
7. cat flag2of2-final.txt
```
1n_pn9_&_pdf_7f9bccd1}

### picoCTF{f1u3n7_1n_pn9_&_pdf_7f9bccd1}
# Conclusion
This challenge showed us how information can be hidden in files by simply changing their extensions and how to recover it. As a forensic analyst, you will encounter many similar examples of files being saved with incorrect extensions to evade analysis. It is always a good idea to open up suspicious files in a Hex Editor to find their true extensions.
USE : https://hexed.it/