---
layout: post
category: geek
tags:
image: one-file-linux-preview.png
title: "One File Linux"
description: "Live linux distro combined in one ~20MB file. Runs on any UEFI computer (PC or Mac) without installation. Just copy one file to EFI system partition and boot. "
---

![One File Linux logo](/img/one-file-linux.png) Live linux distro combined in one ~20MB file. Runs on any UEFI computer (PC or Mac) without installation. Just copy one file to EFI system partition and boot.

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

This can be useful when you need Linux on bare metal and can't use USB flash. In comparison with Live USB flash, one file Linux setups permanently in EFI partition and can boot any time later.<br>My personal goal is to use laptop's internal PCIe WiFi card for cracking WiFi with **aircrack-ng** and **reaver** software, since PCIe devices can't be forwarded into virtual machine.

## Run on Macbook

---

#### 1\. Download OneFileLinux.efi from link above.

#### 2\. Mount EFI System Partition

`diskutil mount diskN`

where diskN is your EFI disk number.<br>To find your EFI disk number use `diskutil list` command.

![macOS diskutil list EFI partition](/img/diskutil-list-efi.png)

For me it will be: `diskutil mount disk0s1`

#### 3\. Copy OneFileLinux.efi to EFI partition

`cp ~/Downloads/OneFileLinux.efi /Volumes/EFI/`

#### 4\. Set boot option in NVRAM

On macOS since El Capitan enabled by default SIP (System Integrity Protection) prohibits to change boot options.<br>To check SIP state run `csrutil status`. In normal situation it should be enabled.

If SIP is enabled you can run `bless` only from Recovery console, otherwise it returns error.<br>To boot in Recovery mode press **CMD+R** while boot and go to ***Utilities —&gt; Terminal*** from top menu.<br>In recovery console follow steps 2 and 4 every time you need to boot OneFileLinux.

`bless --mount /Volumes/EFI --setBoot --nextonly --file /Volumes/EFI/OneFileLinux.efi`

This command sets NVRAM option to boot OneFileLinux.efi only once. Next reboot will return default boot order.

#### 5\. Reboot

Reboot to run OneFileLinux. Once you've done, type `reboot` in Linux console and go back to macOS. <br>Every time when you need it again, follow steps 2 and 4 from recovery console.

## Run on PC

---

There are few ways how to run OneFileLinux on PC motherboard. Some motherboards have builtin UEFI Shell that can run any efi binary from console.<br>I will describe setup process for my old ThinkPad X220 that doesn't have UEFI shell.

#### 1\. Copy OneFileLinux.efi to EFI partition

If you use Windows 10 installed in EFI mode, you have EFI system partition 100 MB in size.<br>You need to find out how to mount by itself. You can do this with OneFileLinux.efi run from USB flash or any other linux distro.

#### 2\. Add NVRAM boot option

I can't find out how to do this in Windows, so you probably need Linux for this.<br>Replace `/dev/sda` to you disk path and `--part 2` to your EFI partition number.

`efibootmgr --disk /dev/sda --part 2 --create --label "One File Linux" --loader /OneFileLinux.efi`

#### 3\. Choose One File Linux from boot menu

On my ThinkPad X220 I press F12 while power on to open boot menu. Hotkey depends on your motherboard.

![ThinkPad X220 boot menu](/img/thinkpad-x220-boot-menu.png)

## Run from USB flash

---

The only benefit from running OneFileLinux from USB flash, is that no additional software is required to create bootable flash drive.<br>Just format flash drive as FAT32 in GPT scheme and copy OneFileLinux.efi to default path:

`\EFI\BOOT\BOOTx64.EFI`

#### Format in GPT scheme in Windows

Windows does not allow to format flash drive in GPT scheme from GUI, so you need to use command line tool.

1. Open `cmd.exe` as administrtor
2. Type`diskpart`
3. `list disk` to see all disks
4. `select disk <disknumber>`
5. `clean` do delete parition table
6. `convert gpt` to convert disk in GPT scheme
7. `exit`

Then format drive from `diskmgmt.msc` in FAT32.

## Build your own

---

You can build your own version of One File Linux.<br>It based on Alpine Linux and vanilla kernel.

1. Clone repositry<br>`git clone https://github.com/zhovner/OneFileLinux`

2. Make changes in root filesystem and kernel<br>`chroot alpine-minirootfs /bin/ash`

3. Build<br>`./build.sh`