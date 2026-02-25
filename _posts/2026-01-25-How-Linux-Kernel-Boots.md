---
layout: post
title: "How Linux Kernel Boots ?"
date: 2026-01-25
categories: Linux
tags: bootloader grub 
image:
    path: assets/img/post/how-kernel-boot/bootloader.png
---
Ever wondered what really happens in those few seconds (or agonizing minutes on older hardware) between pressing the power button and finally seeing that glorious login screen? It often feels like staring into a black void while your machine decides whether today is the day it cooperates.

But once you peek under the hood - well, under the keyboard deck in your laptop - you'll never look at those rapid-fire boot messages the same way again. Suddenly you'll catch yourself thinking, "Ohhh, so that's GRUB flexing its menu muscles… and there goes the kernel decompression like it's unpacking its entire life story in under a second."

## A mental model of boot process
```bash
Firmware
  -> Bootloader
     -> kernel entry (arch asm)
        -> start_kernel() [C]
           -> initcalls (drivers/subsystems)
           -> mount initramfs or rootfs
           -> execve("/sbin/init")  => PID 1
              -> userspace boot continues
```

***Kernel Ring Buffer***
It is the Kernel's built-in circular log storage. To view the logs we can use the following commands.

```bash
$ dmesg | less 
$ dmesg -T #human readable timestamps
$ dmesg -w #follow new kernel message live
```
Unlike the ring buffer, the systemd journal is persistent. It is a userspace log store and keeps more history than the ring buffer.

```bash
journalctl -k -b            # kernel logs this boot
journalctl -k -b -1         # previous boot
journalctl -k -p err..alert # only errors and worse
```
***Kernel Parameters***
It is a settings string that tells the kernel how to boot and what environment it's in. The kernel exposes it at `/proc/cmdline`

```bash
$ cat /proc/cmdline

# I am using WSL
# Output
initrd=\initrd.img WSL_ROOT_INIT=1 panic=-1 nr_cpus=20 hv_utils.timesync_implicit=1 console=hvc0 debug pty.legacy_count=0 WSL_ENABLE_CRASH_DUMP=1
```

What is the keyparameter trying to say ?

This cmdline says: boot Linux in a WSL2/Hyper-V VM, use an initrd, use up to 20 CPUs, send kernel logs to the hypervisor console, enable Hyper-V time sync behavior, crank debug verbosity, don’t auto-reboot on panic, and enable WSL crash-dump related behavior.

<hr style="border:2px solid blue">

## Boot Loaders
Here is a funny "chicken-and-egg" problem. 

The function of bootloader is to load the kernel into memory and then start the kernel with a set of kernel paramters.

But where is kernel and kernel parameter? They are somewhere on the root filesystems. But the kernel hasn't started yet !!

***Solution***: Staged Bootstrapping. 

Bootloader uses BIOS/UEFI to access disks. And modern bootloader has the capability to read the partition tables and have built-in support for read-only access to filesystem.

### GRUB
GRand Unified Bootloader (GRUB 2): its core job is to select and load a kernel + initramfs + cmdline, then jump into the kernel.

![Grub Menu](assets/img/post/how-kernel-boot/grub-menu.png)

 To access the GRUB menu, press and hold `SHIFT` when BIOS or firmware startup screen first appears. Then press `ESC` to temporarily disable the automatic boot timeout after the GRUB menu appears.

You can view the GRUB config editor pressing `e` or comamnd line with `c`.

Lets not get into the weeds with the commands in GRUB CLI. 

***grub.cfg***
```bash
# Location in most distro
/boot/grub/grub.cfg
```
It is not recommended to hand-edit .cfg file for permanent change. Its usually auto generated and updates (kernal updates, update-grub, package upgrades) can overwrite it.

***Where is GRUB installed?***

You install GRUB as an EFI program onto the EFI System Partition (ESP) (a small FAT32 partition, usually mounted at /boot/efi). Then you create a UEFI boot entry in NVRAM (or place a fallback file).

```bash
$ findmnt /boot/efi #Ensure ESP is mounted
$ sudo mount /dev/nvme0n1p1 /boot/efi # mount if not
$ sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB # Install GRUB EFI
$ sudo grub-mkconfig -o /boot/grub/grub.cfg # Generate config
```
On Ubuntu/Debian you often use `sudo update-grub`, which runs the above generation step.

***What files get created***

You’ll typically see something like:

> /boot/efi/EFI/GRUB/grubx64.efi (the EFI executable)

> /boot/grub/grub.cfg (menu/config)

> /boot/grub/x86_64-efi/* (GRUB modules)

***Where this sits in the boot chain***

> UEFI: Firmware → grubx64.efi on ESP → GRUB menu → kernel


## TLDR
The boot process in Linux is a step-by-step handoff from hardware to software, ending with user-level processes (like your shell or desktop environment).

Here's a simplified overview:
1. BIOS/UEFI Boots: Your computer's firmware loads the bootloader (e.g., GRUB) from the disk.
2. Bootloader Loads Kernel: GRUB finds and loads the Linux kernel into memory.
3. Kernel Initializes: The kernel sets up hardware (CPU, memory, devices), mounts the root filesystem (usually /), and then looks for init.
4. Kernel Starts Init: The kernel executes /sbin/init (or whatever init program is configured). This is where init takes over.