# Microsoft Azure: Azure VM Disk Encryption

Data security and compliance are paramount when running enterprise workloads in the cloud. Whether dealing with financial transactions or sensitive healthcare records, organizations must protect data stored on Virtual Machine disks from unauthorized access.

**Azure Disk Encryption (ADE)** helps protect and safeguard your data to meet security and compliance commitments. It uses the **BitLocker** feature of Windows and the **DM-Crypt** feature of Linux to provide volume encryption for the OS and data disks of your Azure VMs.

---
[![AZURE in ENGLISH - LAB - DISK ENCRYPTION - AZURE KEY VAULT](https://img.youtube.com/vi/tdCpRfn5-6s/hqdefault.jpg)](https://youtu.be/tdCpRfn5-6s)
===
[![YouTube Video](https://img.youtube.com/vi/MZl0rbndjdU/hqdefault.jpg)](https://youtu.be/MZl0rbndjdU)
--


## 1. How Azure Disk Encryption Works

Azure Disk Encryption is integrated with **Azure Key Vault** to control and manage the disk encryption keys and secrets. 

*   **Key Vault Integration:** The encryption keys are secured in your Azure Key Vault, ensuring that you maintain full control over your cryptographic keys.
*   **Operating System Support:** ADE supports both Windows and Linux VMs (though specific Linux distributions require compatible kernel versions and configuration setups).
*   **Storage Levels:** Azure also offers **Storage Service Encryption (SSE)** by default for managed disks at rest, but ADE provides volume-level encryption utilizing customer-managed or platform-managed keys directly inside the guest OS stack.

---

## 2. Key Prerequisites & Limitations

Before deploying disk encryption on Azure VMs, keep the following considerations in mind:

1.  **VM Size/Tier:** 
    *   Azure Disk Encryption is **not supported** on Basic tier virtual machines (e.g., A-series basic instances). 
    *   You must use Standard tier or general-purpose/compute/memory-optimized instances (e.g., D-series, F-series, E-series) with at least **2 GB of RAM**.
2.  **Key Vault Configuration:** 
    *   The Azure Key Vault used to store the keys **must** be created in the same Azure region and subscription as the VM.
    *   When creating the Key Vault via PowerShell or CLI, you must explicitly enable the vault for disk encryption: 
        *   `-EnabledForDiskEncryption` (PowerShell) 
        *   `--enabled-for-disk-encryption` (CLI)
3.  **Backups and Snapshots:** 
    *   Always take a snapshot or backup of your virtual machine disks *before* initiating encryption on production systems. If an interruption occurs during the encryption process, recovery is much safer with a pre-existing snapshot.

---

## 3. Step-by-Step Implementation Workflow (PowerShell Lab)

As demonstrated in the tutorial, enabling disk encryption follows a structured lifecycle:

### Step 1: Deploy the Virtual Machine
Ensure you provision a supported VM size (e.g., standard general-purpose tier running Windows Server or Linux).

### Step 2: Create a Dedicated Azure Key Vault
Create a Key Vault with a globally unique name, ensuring the disk encryption flag is enabled:
```powershell
New-AzKeyVault -VaultName "StanKeyVault123" -ResourceGroupName "MyResourceGroup" -Location "East US" -EnabledForDiskEncryption

```

### Step 3: Fetch Key Vault Reference

Retrieve the Key Vault resource ID and store it in a variable to pass into the encryption command:

```powershell
$KeyVault = Get-AzKeyVault -VaultName "StanKeyVault123" -ResourceGroupName "MyResourceGroup"
$KeyVaultResourceId = $KeyVault.ResourceId

```

### Step 4: Enable Encryption on the VM

Run the encryption command specifying the volume type (`OS`, `Data`, or `All`):

```powershell
Set-AzVMDiskEncryptionExtension -ResourceGroupName "MyResourceGroup" `
    -VMName "StanAzureVM" `
    -DiskEncryptionKeyVaultId $KeyVaultResourceId `
    -VolumeType All

```

*(Note: This process may trigger a reboot of the virtual machine and will take a few minutes to complete).*

### Step 5: Verify Encryption Status

To confirm that the operating system and data volumes have been successfully encrypted, run the status cmdlet:

```powershell
Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyResourceGroup" -VMName "StanAzureVM"

```

The output will display the encryption progress and confirm whether the OS and data volumes are actively encrypted.

---

## 4. Troubleshooting Common Disk Encryption Issues

* **Extension Deployment Failures:** If the encryption extension fails to install, ensure that the Azure VM Agent is running properly and that outbound internet access (port 443) to Azure Key Vault is not blocked by a firewall or NSG.
* **Key Vault Access Policies:** Ensure the Azure Disk Encryption service principal has been granted proper permissions (Get, WrapKey, UnwrapKey, Set) to the Key Vault.

```

***

[![AZURE in ENGLISH - LAB - DISK ENCRYPTION - AZURE KEY VAULT](https://img.youtube.com/vi/tdCpRfn5-6s/hqdefault.jpg)](https://youtu.be/tdCpRfn5-6s)
http://googleusercontent.com/youtube_content/1

```
