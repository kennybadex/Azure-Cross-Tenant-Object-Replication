# Azure-Cross-Tenant-Object-Replication
Postman collection to implement Azure Cross-Tenant Object Replication via API



# Azure Cross-Tenant Storage Object Replication (OR)

This repository demonstrates how to configure **Azure Storage Object Replication across tenants** using **Azure Resource Manager REST APIs** and **Postman**.

> ⚠️ Cross-tenant Object Replication is not fully supported via Azure Portal or Az PowerShell and **requires direct REST API calls**.

---

## 🧠 Architecture Overview

- **Tenant A** → Source storage account
- **Tenant B** → Destination storage account
- Each tenant uses **its own Service Principal**
- The Object Replication policy **must be created on the destination first**
- The same policy is then **applied on the source**



---

## 🔐 Authentication Model (Important)

- Two **separate Service Principals** are used:
  - One in **Tenant A**
  - One in **Tenant B**
- ✅ Each SP authenticates **only to its own tenant**

This works because Object Replication is validated by Azure Storage **after both policies exist**, not during token issuance.

---

## 🧾 Prerequisites

- Azure Storage accounts (Blob Storage / GPv2)
- Containers created on both sides
- Object Replication supported region
- Owner or Contributor on the storage accounts
- Postman

---

## 🔧 Generate Required GUIDs

Before running the requests, generate GUIDs for PolicyID and RuleID that will be sent in the first PUT request to Destination Tenant.

### PowerShell
```powershell
$PolicyGuid = (New-Guid).Guid
$RuleId     = (New-Guid).Guid

Import Postman json collection into Postman
