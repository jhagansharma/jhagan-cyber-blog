---

title: "canYouSeeMe"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

# CanYouSeeMe

## Challenge Description

> How about some hide and seek?

This challenge focuses on metadata analysis. The flag is hidden within the metadata of an image file.

---

## Download Challenge

```bash
wget https://artifacts.picoctf.net/c_titan/6/unknown.zip
```

Extract the archive:

```bash
unzip unknown.zip
```

This gives us:

```text
ukn_reality.jpg
```

---

## Initial Analysis

A good first step in forensic challenges is checking file metadata.

Run:

```bash
exiftool ukn_reality.jpg
```

### What is ExifTool?

ExifTool is a powerful command-line utility used to read, write, and edit metadata from files such as:

* Images (JPG, PNG)
* PDFs
* Audio files
* Video files

It is widely used in digital forensics and CTF challenges.

---

## Interesting Finding

The output contains the following field:

```text
Attribution URL : cGljb0NURntNRTc0RDQ3QV9ISUREM05fYTZkZjhkYjh9Cg==
```

This looks like Base64 encoded data.

---

## Decoding the Value

The string can be decoded using CyberChef.

Recipe:

```text
From Base64
```

After decoding, the hidden flag is revealed.

---

## Flag

```text
picoCTF{ME74D47A_HIDD3N_a6df8db8}
```

---

## Key Takeaways

* Always inspect metadata during forensic challenges.
* ExifTool is an essential tool for CTFs and DFIR work.
* Metadata often contains hidden information.
* Base64 encoding is commonly used to conceal flags in beginner challenges.
