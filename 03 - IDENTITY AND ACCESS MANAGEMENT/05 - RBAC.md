
🔐 𝗔𝘇𝘂𝗿𝗲 𝗥𝗕𝗔𝗖 — 𝗪𝗵𝗼 𝗰𝗮𝗻 𝗱𝗼 𝗪𝗵𝗮𝘁, 𝗪𝗵𝗲𝗿𝗲?

Azure RBAC (Role-Based Access Control) is used to manage who has access to Azure resources, what they can do, and at which scope.

Think of it in 3 simple questions:

👤 𝗪𝗛𝗢 → 𝗪𝗵𝗼 𝗴𝗲𝘁𝘀 𝗮𝗰𝗰𝗲𝘀𝘀?User / Group / Service Principal / Managed Identity
🎯 𝗪𝗛𝗔𝗧 → 𝗪𝗵𝗮𝘁 𝗰𝗮𝗻 𝘁𝗵𝗲𝘆 𝗱𝗼?Reader / Contributor / Owner / Custom Role
📍 𝗪𝗛𝗘𝗥𝗘 → 𝗪𝗵𝗲𝗿𝗲 𝗱𝗼𝗲𝘀 𝘁𝗵𝗲 𝗽𝗲𝗿𝗺𝗶𝘀𝘀𝗶𝗼𝗻 𝗮𝗽𝗽𝗹𝘆?Management Group → Subscription → Resource Group → Resource

For example:Developer → Contributor → Resource Group

This means the developer can manage resources within that Resource Group based on the permissions defined by the Contributor role.

👉 Azure RBAC = Who + What + Where

RBAC helps implement least-privilege access, improve security, and control permissions across Azure 
