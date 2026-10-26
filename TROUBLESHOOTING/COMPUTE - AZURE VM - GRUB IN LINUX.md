# GRUB in Linux: Troubleshooting Mode via Azure Serial Console

When a Linux Virtual Machine in Azure becomes inaccessible via SSH due to boot failures, file system errors, or credential issues, you cannot rely on physical console access like HP iLO or Dell iDRAC. Instead, Azure provides the **Serial Console**, which allows you to access the Grand Unified Bootloader (GRUB) to enter **Single-User Mode** (Maintenance Mode) or **Rescue Mode**.

This guide covers how to interrupt the boot process and edit GRUB parameters to perform emergency maintenance.

[![Azure Tips & Tricks - GRUB in Linux Troubleshooting Mode](https://img.youtube.com/vi/eHS7rlbsr3o/hqdefault.jpg)](https://youtu.be/eHS7rlbsr3o)
---

## 1. Prerequisites: Enabling the GRUB Menu

By default, some Linux distributions boot too quickly to catch the GRUB menu. You must first configure the GRUB timeout via the Serial Console to give yourself time to intercept the boot process.

1. In the Azure Portal, go to your VM and scroll down to **Help > Serial Console**.
2. Log in using your existing credentials.
3. Edit the GRUB configuration file:
   * **Ubuntu/Debian:** `sudo vi /etc/default/grub`
   * **RHEL/CentOS:** `sudo vi /etc/default/grub`
4. Modify the following parameters to ensure the menu appears and stays on screen long enough:
   ```bash
   GRUB_TIMEOUT=10        # Set to 10 seconds or higher
   GRUB_TIMEOUT_STYLE=menu # Change from 'hidden' to 'menu'
Update the GRUB configuration to apply changes:

Ubuntu: sudo update-grub

RHEL/CentOS: sudo grub2-mkconfig -o /boot/grub2/grub.cfg

2. Booting into Single-User / Maintenance Mode (Ubuntu)
If you are locked out or need to run file system checks without loading full services:

Restart the Virtual Machine from the Azure Portal (or type reboot in the console).

Watch the Serial Console. When the GRUB menu appears, immediately press e to edit the boot parameters.

Scroll down to the line that starts with linux (or linux16 / linuxefi).

Look for the parameter ro (read-only).

Add the word single immediately after ro (ensure there are spaces before and after single).

Example: ... ro single root=UUID=...

Press Ctrl + X or F10 to boot with the modified parameters.

You will be dropped directly into a root shell (Maintenance Mode) without needing a password.

Common Troubleshooting Commands in Single-User Mode:
View System Logs: journalctl -xb

Check File Systems: fsck -y /dev/sdX

Reset Root/User Password: passwd <username>

Exit and resume normal boot: Type exit or reboot.

3. Rescue Mode & Emergency Mode for RHEL / CentOS / SUSE
Different flavors of Linux handle maintenance modes slightly differently.

RHEL 7 / CentOS 7
Instead of adding single, you change the systemd target.

Interrupt GRUB and press e to edit.

Find the linux16 line.

Append systemd.unit=rescue.target to the end of the line.

Press Ctrl + X to boot.

RHEL 8 / RHEL 9 (rd.break)
For newer Red Hat systems, the standard method is to interrupt the initramfs phase before the actual root file system is mounted.

Interrupt GRUB and press e to edit.

Find the linux line.

Append rd.break to the end of the line.

Press Ctrl + X to boot.

Remount the sysroot in read-write mode to reset passwords:

Bash
mount -o remount,rw /sysroot
chroot /sysroot
SUSE Linux Enterprise Server (SLES)
In SUSE, you can use the yast bootloader configuration tool, or manually append the boot parameter.

In the GRUB menu, press e to edit.

Find the line starting with linux or linuxefi.

Append init=/bin/sh or systemd.unit=rescue.target to the end of the line.

Press F10 to boot into the rescue shell.

Note: Editing GRUB at boot time is temporary. It only applies to that specific boot instance. Once you reboot, the machine will revert to the standard settings defined in /etc/default/grub.

http://googleusercontent.com/youtube_content/1
