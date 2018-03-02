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
My personal goal is to use laptop's internal PCIe WiFi card for cracking WiFi with <b>aircrack-ng</b> and <b>reaver</b> software, since PCIe devices can't be forwarded into virtual machine. 

---
  
## Run on Macbook

#### 1. Download OneFileLinux.efi from link above.  
  
  

#### 2. Mount EFI system partition 

`diskutil mount diskN` 

Where diskN is your EFI disk number.  
To find your EFI disk number use `diskutil list` command.  
  
<img width="500" alt="macOS diskutil list EFI partition" src="/img/diskutil-list-efi.png" />

For me it will be: `diskutil mount disk0s1`

  
  
  
#### 3. Copy OneFileLinux.efi to EFI partition
  
`cp ~/Downloads/OneFileLinux.efi /Volumes/EFI/`

  
  
#### 4. Set boot option in NVRAM

On macOS since El Capitan enabled by default SIP (System Integrity Protection) prohibits to change boot options.  
To check SIP state run `csrutil status`. In normal situation it should be enabled.  
  
If SIP is enabled you can run bless <b>only</b> from Recovery console, otherwise it returns error.  
To boot in Recovery mode press <b>CMD+R</b> while boot and go to **_Utilities —> Terminal_** from top menu.  
In recovery console follow steps 2 and 4 every time you need to boot OneFileLinux.  

`bless --mount /Volumes/EFI --setBoot --nextonly --file /Volumes/EFI/OneFileLinux.efi`
  
  
This command sets NVRAM option to boot OneFileLinux.efi only once. Next reboot will return default boot order. 
  
  
  
### 5. Reboot 

Reboot to run OneFileLinux. Once you've done, type `reboot` in Linux console and go back to macOS.   
Every time when you need it again, follow steps 2 and 4 from recovery console.

---

## Run on PC


