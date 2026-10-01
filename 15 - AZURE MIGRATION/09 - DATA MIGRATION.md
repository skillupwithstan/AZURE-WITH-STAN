[![MIGRATE OS and DATA Disks to AZURE and Create Windows VM from Custom Image](https://img.youtube.com/vi/pH3x-kqLxoA/maxresdefault.jpg)](https://youtu.be/pH3x-kqLxoA)

MIGRATE OS and DATA Disks to AZURE and Create Windows VM from Custom Image
This session provides a detailed, practical guide on migrating individual on-premises Hyper-V workloads to Azure by manually uploading and converting virtual hard disks (VHDs).

Key Learning Outcomes:

VHD Conversion: Learn why dynamic disks must be converted to fixed-size VHDs before migration and how to execute this conversion via Hyper-V Manager.

Disk Upload Methods: Explores five distinct methods to transfer your VHDs to Azure Blob Storage over the internet:

Using the built-in Azure Storage Browser.

Leveraging the Azure Storage Explorer desktop application.

Running the AzCopy command-line utility for highly efficient, scriptable uploads.

Drag-and-drop within the Azure Portal.

Using third-party tools like CloudBerry Explorer for multi-cloud transfers.

Creating VMs from Uploaded Disks: Watch a step-by-step demonstration of converting an uploaded Page Blob (VHD) into an Azure Managed Disk, converting that disk into a custom Image, and ultimately deploying a fully functioning Azure Virtual Machine from that image.

OS Disk Swapping: A powerful troubleshooting and migration technique—learn how to hot-swap an OS disk on an existing Azure VM with an uploaded on-premises disk, preserving the VM's configuration and IP address while entirely replacing its underlying operating system and data.
