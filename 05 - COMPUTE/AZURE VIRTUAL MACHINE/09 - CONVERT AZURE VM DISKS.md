# 09 - CONVERT AZURE VM DISKS

[![AZURE IN TAMIL - TIPS & TRICKS - AZURE VM DISK TYPE CONVERSION #azure #virtualmachine #vm](https://img.youtube.com/vi/CyReLAGezAg/0.jpg)](https://youtu.be/CyReLAGezAg)
*Watch the full quick-tip video on the [Skill Up with Stan YouTube channel](https://youtu.be/CyReLAGezAg).*

Azure provides two overarching categories of disks: **Managed Disks** and **Unmanaged Disks**. Modern Azure deployments should exclusively use Managed Disks, as Microsoft handles the underlying storage infrastructure, scalability, and availability. 

However, if you have legacy workloads using Unmanaged Disks, you can convert them to Managed Disks.

---

## 1. Unmanaged to Managed Disk Conversion (The Azure Way)

Microsoft strongly recommends converting Unmanaged Disks to Managed Disks to benefit from better reliability (99.99
