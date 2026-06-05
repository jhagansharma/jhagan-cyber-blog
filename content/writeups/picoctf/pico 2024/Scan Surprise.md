---

title: "# Scan Surprise"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

## I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?

# solution

```
1. ssh -p 64695 ctf-player@atlas.picoctf.net
2. ls
```
- you find the QR code in this

`3. zbarimg flag.png`

### zbar-tools is a Linux package that lets you scan and read barcodes or QR codes directly from the command line.
### It includes tools like:
### ✅ zbarimg → scans QR codes/barcodes from images (PNG, JPG, etc.)
### ✅ zbarcam → scans QR codes/barcodes live from a webcam