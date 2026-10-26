# Resetting Forgotten Passwords in Azure Virtual Machines

Losing or forgetting administrator credentials (root for Linux, local admin for Windows) is a common scenario. In a traditional on-premises data center, recovering a password often requires booting into a live CD, altering GRUB menus for Single-User Mode, or using a "jail" environment. 

[![Azure Troubleshooting - Reset Forgotten Password](https://img.youtube.com/vi/6rgLXv2sw8M/hqdefault.jpg)](https://youtu.be/6rgLXv2sw8M)

In Azure, this process is significantly streamlined through the use of **VM Extensions**, eliminating the need for complex, manual OS-level interventions.

## Method 1: The Azure Portal (Reset Password Extension)

This is the easiest and most direct way to regain access to your VM. Azure provides a built-in tool that uses the Azure VM Agent (VMAccess Extension) to inject a new password directly into the operating system.

### For Windows & Linux VMs:
1. Navigate to the specific Virtual Machine in the Azure Portal.
2. In the left-hand menu, scroll down to the **Help** section.
3. Click on **Reset password**.
4. You will be presented with three options (for Linux, fewer for Windows):
   * **Reset password:** Change the password for an existing user or create a new user with admin privileges.
   * **Reset SSH public key (Linux only):** Overwrite or append a new SSH public key for the specified user.
   * **Reset configuration only (Linux only):** Resets the SSH daemon configuration (`sshd_config`) to its default state if you've locked yourself out due to a misconfiguration.
5. Enter your **Username** (either an existing admin user or a new name to create a brand new admin account).
6. Enter and confirm the new **Password**.
7. Click **Update**.

*Behind the Scenes:* Azure deploys the `VMAccess` extension (e.g., `VMAccessForLinux` or the Windows equivalent) into the `C:\Packages\Plugins` or `/var/lib/waagent` directory. This extension executes a script locally with SYSTEM/root privileges to reset the credentials and add the user to the local Administrators or `sudoers` group.

---

## Method 2: Azure CLI

If you prefer command-line tools or need to script the password reset across multiple environments, you can use the Azure CLI. This performs the exact same action as the portal by calling the VMAccess extension.

**Reset a Linux or Windows VM Password:**
```bash
az vm user update \
  --resource-group MyResourceGroup \
  --name MyVM \
  --username adminuser \
  --password NewStrongPassword123!

  Method 3: Azure PowerShell
For Windows-heavy environments, PowerShell is often the tool of choice. You can use the Set-AzVMAccessExtension cmdlet to force a credential update.

\$Sub = "Your-Subscription-ID"
\$RgName = "MyResourceGroup"
\$VmName = "MyVM"
\$Username = "adminuser"
\$Password = "NewStrongPassword123!"

\$Cred = New-Object System.Management.Automation.PSCredential ($Username, (ConvertTo-SecureString$Password -AsPlainText -Force))

Set-AzVMAccessExtension -ResourceGroupName \$RgName -VMName $VmName -Name "VMAccessAgent" -TypeHandlerVersion "2.0" -Credential $Cred

Fallback Method: The Serial Console
If the Azure VM Agent is broken, offline, or the extension fails to execute, you cannot use the methods above. You must resort to the Azure Serial Console, which acts as a direct COM port connection to the machine.

For Linux (Single-User Mode):
Go to Help > Serial console in the Azure Portal.

Reboot the VM.

Interrupt the GRUB boot menu by pressing e.

Append single or rd.break to the linux boot line (depending on your distribution).

Boot into the root shell and use the passwd command to reset the root password.

For Windows (CMD via SAC):
Go to Help > Serial console.

Open a CMD channel (cmd -> ch -si 1).

If you have any remaining functional admin credentials, log in.

Use the net user command to reset the password or create a new user:

net user NewAdmin UserPass123! /add
net localgroup administrators NewAdmin /add

Important Considerations
VM Agent Requirement: Methods 1, 2, and 3 strictly require the Azure VM Agent to be installed and running properly on the guest OS.

Domain Controllers: The Azure Portal "Reset Password" tool does not work on Active Directory Domain Controllers. It only resets local accounts, not domain accounts. To reset a Domain Admin password, you must use Directory Services Restore Mode (DSRM).

