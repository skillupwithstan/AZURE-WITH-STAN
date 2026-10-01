[![AZURE MIGRATE IN TAMIL - ON PREMISES HYPERV HOST to AZURE](https://img.youtube.com/vi/N2qwNR7Ypho/maxresdefault.jpg)](https://youtu.be/N2qwNR7Ypho)

AZURE MIGRATE IN TAMIL - ON PREMISES HYPERV HOST to AZURE
This video demonstrates a hands-on, end-to-end server migration using Azure Migrate, transferring a workload from an On-Premises Hyper-V host to Azure.

Key Learning Outcomes:

Discovery Options: Explores two distinct ways to discover on-premises inventory—using an Excel/CSV import for environments with restricted access, or deploying the Azure Migrate Appliance via a .vhd file on a Hyper-V host for agentless discovery.

Appliance Configuration: Walkthrough of deploying the Azure Migrate Appliance, generating the project key in Azure, registering the appliance, and validating the connection to pull complete VM inventories (including hardware, OS, and dependent apps) into the Azure portal.

Azure Site Recovery Integration: Step-by-step guidance on setting up replication policies, configuring target landing zones (VNet and Storage Accounts in the Azure target region), and pairing the on-prem Hyper-V site.

Testing & Cutover: Differentiates between a Test Failover and a Planned Failover. Shows how to gracefully shut down the source VM, sync final changes, bring up the replicated VM in Azure, adjust Security Groups (NSGs), and assign a Public IP.

Validation & Commit: Concludes with verifying migrated services (RDP, Web Server, File Shares) in the new Azure VM and finalizing the migration by committing and disabling ongoing replication.
