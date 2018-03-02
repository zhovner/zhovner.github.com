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
There are few ways how to run OneFileLinux on PC motherboard. Some motherboards have builtin UEFI Shell that can run any efi binary from console.  
I will describe setup process for my old ThinkPad X220 that haven't UEFI shell. 

#### 1. Copy OneFileLinux.efi to EFI partition 
  
If you use Windows 10 installed in EFI mode, you have EFI system partition 100 MB in size.  
You need to find out how to mount by itself. You can do this with OneFileLinux.efi runned from USB flash or any other linux distro.


#### 2. Add NVRAM boot option

I can't find out how to do this in Windows, so you probably need Linux for this.  
Replace `/dev/sda` to you disk path and `--part 2` to your EFI partition number.  
  
`efibootmgr --disk /dev/sda --part 2 --create --label "One File Linux" --loader /OneFileLinux.efi`

#### 3. Choose One File Linux from boot menu

On my ThinkPad X220 I press F12 while power on to open boot menu. Hotkey depends on your motherboard.  
  
<img alt="ThinkPad X220 boot menu" width="600" src="/img/thinkpad-x220-boot-menu.png" />

---

## Run from USB flash
The only benefit from running OneFileLinux from USB flash, is that no additional software required to create bootable flash drive.  
Just format flash drive as FAT32 in GPT scheme and copy OneFileLinux.efi to default path `\EFI\BOOT\BOOTx64.EFI`

Windows 



