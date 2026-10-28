# 08 - MIGRATE OS AND DATA DISKS TO AZURE VM

[![MIGRATE OS and DATA Disks to AZURE and Create Windows VM from Custom Image](https://img.youtube.com/vi/pH3x-kqLxoA/0.jpg)](https://youtu.be/pH3x-kqLxoA)
*Watch the full step-by-step demonstration on the [Skill Up with Stan YouTube channel](https://youtu.be/pH3x-kqLxoA).*

Migrating on-premises Virtual Machine disks (OS and Data) to Microsoft Azure is a fundamental step in cloud migration. Instead of migrating the entire VM via an appliance, you can manually export, upload, and convert your existing virtual hard disks into Azure Managed Disks.

---

## 1. Prerequisites and Disk Preparation

Before uploading your on-premises virtual disks (e.g., from Hyper-V or VMware) to Azure, they must meet specific formatting requirements. Azure strictly supports **fixed-size VHDs**. 

**Preparation Steps in Hyper-V:**
1. **Turn off the VM:** Ensure the on-premises virtual machine is shut down to prevent data corruption and to capture a consistent state.
2. **Convert to VHD:** Azure does **not** support the `.vhdx` format. If your disk is a `.vhdx`, use the Hyper-V Manager (Edit Disk wizard) or PowerShell to convert it to `.vhd`.
3. **Set to Fixed Size:** Azure does **not** support dynamically expanding disks. During the Hyper-V Edit Disk wizard, ensure you convert the disk type to **Fixed size**.
4. **MBR vs. GPT:** For disks under 2 TB, use MBR (Master Boot Record) initialization. For larger disks, use GPT.

---

## 2. 5 Methods to Upload a VHD to Azure Blob Storage

Once your disk is exported as a fixed-size `.vhd`, it needs to be uploaded to an Azure Storage Account as a **Page Blob**. Here are five methods to securely upload your disk:

1. **Azure Portal (Direct Container Upload):** Navigate to your Storage Account > Containers > Upload. Suitable for very small test disks.
2. **Azure Storage Browser (in Portal):** Use the built-in Storage Browser within the Azure Portal to manage and upload blobs directly.
3. **Azure Storage Explorer (GUI Tool):** A standalone desktop application by Microsoft. Connect using an Access Key, SAS URI, or Azure AD. Drag and drop your `.vhd` file into a container. It automatically detects and uploads `.vhd` files as Page Blobs.
4. **AzCopy (Command-Line Tool):** The most efficient and recommended way for large disks.
   ```bash
   azcopy copy "D:\Temp\my-os-disk.vhd" "https://<storage-account>.blob.core.windows.net/<container>/my-os-disk.vhd?<SAS-token>" --blob-type PageBlob

   Third-Party Tools (e.g., CloudBerry Explorer): Tools that allow you to connect multiple cloud endpoints (e.g., AWS S3 and Azure Blob) and drag-and-drop data between on-premises and Azure.

3. Converting the Uploaded VHD to an Azure Managed Disk
Once the .vhd file is successfully uploaded as a Page Blob, you must convert it into an Azure Managed Disk before it can be attached to a VM.

In the Azure Portal, search for and select Disks.

Click Create.

Choose the appropriate Resource Group and Region.

For Source type, select Storage blob.

Browse your storage account and select the uploaded .vhd file.

Choose the OS type (Windows/Linux) if it is an OS disk. If it is a data disk, leave it blank/data.

Select the appropriate Size and Performance tier (e.g., Standard SSD, Premium SSD).

Click Review + Create.

4. Creating an Azure VM from the Migrated OS Disk
If the disk you migrated is an Operating System disk, you can use it to spin up an exact replica of your on-premises VM in Azure.

Method A: Create a Custom Image (Recommended for scaling)

Go to Images in the Azure Portal and click Create.

Select the Region and OS state (Generalized or Specialized).

Under OS Disk, select the Managed Disk you just created.

Once the Image is created, click Create VM directly from the Image overview page. This allows you to deploy multiple identical VMs.

Method B: Direct VM Creation
If you don't want to create an image, you can click on the Managed OS Disk you created, and select Create VM directly from the disk's overview blade.

5. Attaching a Migrated Data Disk
If the migrated disk was an additional data drive (e.g., a database volume):

Navigate to the target Virtual Machine in Azure.

Under Settings, select Disks.

Click Attach existing disks.

Select the newly created Data Managed Disk from the dropdown.

Apply the changes.

RDP/SSH into the VM. The drive, complete with its partitions, folders, and files, will be immediately available.

6. The "Swap OS Disk" Feature
Azure offers a powerful feature called Swap OS Disk. This is highly useful during migrations, upgrades, or disaster recovery troubleshooting.

If an Azure VM's OS gets corrupted, or if you simply want to replace the OS with a freshly migrated on-premises disk, you don't need to rebuild the entire VM structure (NIC, IP, NSG).

How it works:

Navigate to the VM > Disks.

Click Swap OS disk.

Select your newly migrated Managed OS Disk.

Azure will safely shut down the VM, swap the underlying OS virtual hard drive, and restart it. The VM retains its Public/Private IPs and hardware configuration, but boots into the new Operating System.
