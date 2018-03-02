---
layout: post
category: geek
tags:
excerpt:
title: One File Linux
---

![One File Linux logo](/img/one-file-linux.png) Live linux distro combined in one ~20MB file. Runs on any UEFI computers (PC or Mac) without installation. Just copy one file to EFI system partition and boot. 

<div style="text-align:center;font-size: 200%;">
<a href="https://github.com/zhovner/OneFileLinux/releases/download/v0.3.1/OneFileLinux.efi">Download 25MB</a>
</div>
<div style="text-align:center;">
Sources: <a href="https://github.com/zhovner/OneFileLinux/">github.com/zhovner/OneFileLinux</a>
</div>

### Main advantages

* **No installation required** — no need to create additional paritions. Just copy one file to EFI system partition and add new boot entry to NVRAM.
* **No USB flash needed** — once copied to EFI partition, OneFileLinux can boot any time from system disk.
* **No Boot Manager required (GRUB, rEFInd)** — boots directly by UEFI firmware, no additional software needed.
* **Doesn't change the boot sequence** — can boot only once, next reboot will return default settings.
* **Compatible with disk encryption** — works with macOS FileVault and dm-crypt. Because EFI system parition is not encrypted.

### Why?

This can be useful when you need Linux on bare metal and can't use USB flash. In comparison with Live USB flash, one file Linux setups permanently in EFI partition and can boot any time later. 
