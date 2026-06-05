---

title: "canyouseeme"
date: 2026-06-05
draft: false
------------

# Description
### How about some hide and seek?

## Solution
```
1. wget https://artifacts.picoctf.net/c_titan/6/unknown.zip
3. unzip unknown.zip 
2. exiftool ukn_reality.jpg
```
## exiftool is one of the most powerful command-line tools for reading, writing, and editing metadata in files. It works on images (JPG, PNG), PDFs, videos, and even audio files


Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fYTZkZjhkYjh9Cg==

the Attribution URL section looks like it has Base64 encoded text (cGljb0NURntNRTc0RDQ3QV9ISUREM05fYTZkZjhkYjh9Cg==).
By putting that text into  with Base64 decoding the flag could be found.

USE : https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)

Flag: picoCTF{ME74D47A_HIDD3N_a6d...}