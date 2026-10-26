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
