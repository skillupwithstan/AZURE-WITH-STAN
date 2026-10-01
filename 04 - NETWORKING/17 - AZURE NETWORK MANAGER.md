<img width="800" height="527" alt="image" src="https://github.com/user-attachments/assets/b4de6c45-595e-41d3-bb55-4a6b896ff812" />

Start with a simple setup — multiple VNets and VMs — and explore how Network Manager can centrally manage connectivity between them.

A few things I focused on:

➡️ Creating a Network Group and adding VNets to it
➡️ Creating a Connectivity Configuration
➡️ Testing different connectivity approaches such as Mesh and Hub-and-Spoke
➡️ Understanding how Network Manager manages VNet connectivity
➡️ Validating the connectivity from the VMs
➡️ Checking what actually happens at the VNet/peering level

One thing I found interesting was that Network Manager doesn't simply mean “put two VNets in the same group and they automatically communicate.”

The actual connectivity depends on the Connectivity Configuration and the topology you define.

That distinction became much clearer once I tested it rather than just reading about it.

💡 My takeaway:
Azure Network Manager is less about manually creating individual network connections and more about centrally defining and managing network connectivity at scale.

For a small lab, creating VNet peering manually is straightforward.

But when the number of VNets grows, centrally managing connectivity, topology and network groups becomes much more valuable.
