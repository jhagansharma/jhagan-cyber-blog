---

title: "# Collaborative Development"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
#Description
My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?
You can download the challenge files here:
challenge.zip 

# solution

```
wget https://artifacts.picoctf.net/c_titan/177/challenge.zip
unzip challenge.zip
cd drop-in/
git branch -a
```
this will show all branch 

```
git checkout   feature/part-1
cat flag.py
```
┌──(root㉿Harsh)-[/home/jhagan/drop-in/drop-in]  
└─# cat flag.py  
print("Printing the flag...")   
print("picoCTF{t3@mw0rk_", end='')  
```
git checkout   feature/part-2
cat flag
```
┌──(root㉿Harsh)-[/home/jhagan/drop-in/drop-in]  
└─# cat flag.py  
print("Printing the flag...")  

print("m@k3s_th3_dr3@m_", end='')  

```
git checkout   feature/part-3
cat flag
```
┌──(root㉿Harsh)-[/home/jhagan/drop-in/drop-in]  
└─# cat flag.py  
print("Printing the flag...")  

print("w0rk_7ae8dd33}")  

- picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_7ae8dd33}





















