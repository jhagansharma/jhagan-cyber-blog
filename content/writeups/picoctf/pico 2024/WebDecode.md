---

title: "# webdecode"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------
# WebDecode


### Do you know how to use the web inspector? Start searching here to find the flag

# Solution

- inspect the about page and there is a source code right above that header is a section with this attribute notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDJjZGNiNTl9".
- from64 decord this and find the flag .

USE : https://gchq.github.io/CyberChef/#recipe=Magic(3,false,false,'')


Flag: picoCTF{web_succ3ssfully_d3c0ded_02c...}
