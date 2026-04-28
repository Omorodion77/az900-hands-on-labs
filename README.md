# AZ-900 Azure Fundamentals — Hands-On Labs

> Documenting my journey to Azure certification through practical, hands-on projects.

**Author:** Eromosele  
**Status:** In Progress  
**Subscription:** Azure for Students  
**Certification Target:** AZ-900 Azure Fundamentals

---

## Projects

| # | Project | Key Services | Status |
|---|---------|-------------|--------|
| 1 | [Deploy a VM & Web Server](#project-1-deploy-a-virtual-machine--web-server) | Compute, VNet, NSG | ✅ Complete |
| 2 | [Static Website with Azure Storage](#project-2-static-website-with-azure-storage) | Blob Storage, Static Website | ✅ Complete |
| 3 | [Budget & Monitoring Dashboard](#project-3-budget--monitoring-dashboard) | Cost Mgmt, Monitor, Policy | ⬜ Pending |
| 4 | [Identity & Access Management](#project-4-identity--access-management) | Entra ID, RBAC, MFA | ⬜ Pending |
| 5 | [SQL Database + App Service](#project-5-sql-database--app-service) | PaaS, SQL, App Service | ⬜ Pending |
| 6 | [ARM Templates (IaC)](#project-6-arm-templates-iac) | ARM, Bicep, CLI | ⬜ Pending |
| 7 | [VNet Peering & Networking](#project-7-vnet-peering--networking) | VNet, Peering, VPN | ⬜ Pending |

---

## Project 1: Deploy a Virtual Machine & Web Server

**Exam Domain:** Azure Compute, Networking, Resource Management (25-30% of exam)

### What I Did

- Created a Resource Group (`AZ900-VM-Lab`) using Azure CLI via Cloud Shell
- Deployed a Virtual Network (`az900-vnet`) with address space `10.0.0.0/16` and a subnet (`10.0.1.0/24`)
- Created a Network Security Group with inbound rules for SSH (port 22) and HTTP (port 80)
- Explored all resources in the portal to understand their relationships

> **Note:** Azure for Students subscription had VM quota restrictions across all regions, so I used Azure Cloud Shell + CLI to create networking resources directly. This was a great lesson in real-world Azure constraints.

### Key Concepts Learned

- **IaaS vs PaaS vs SaaS** — VMs are IaaS (you manage the OS and applications)
- **Resource Groups** — Logical containers that hold related Azure resources; deleting the group deletes everything inside
- **Network Security Groups (NSGs)** — Act as firewalls with priority-based rules (lower number = higher priority)
- **Virtual Networks & Subnets** — Private network isolation in Azure; resources in the same VNet can communicate, different VNets are isolated by default
- **Azure Regions** — Physical datacenter locations that affect latency, pricing, and service availability
- **Azure Cloud Shell** — Browser-based CLI environment with pre-installed Azure tools

### CLI Commands Used

```bash
# Create Resource Group
az group create --name AZ900-VM-Lab --location eastus

# Create VNet with subnet
az network vnet create \
  --resource-group AZ900-VM-Lab \
  --name az900-vnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name default \
  --subnet-prefix 10.0.1.0/24

# Create NSG
az network nsg create \
  --resource-group AZ900-VM-Lab \
  --name az900-nsg

# Add SSH rule
az network nsg rule create \
  --resource-group AZ900-VM-Lab \
  --nsg-name az900-nsg \
  --name Allow-SSH \
  --priority 1000 \
  --destination-port-ranges 22 \
  --protocol Tcp \
  --access Allow

# Add HTTP rule
az network nsg rule create \
  --resource-group AZ900-VM-Lab \
  --nsg-name az900-nsg \
  --name Allow-HTTP \
  --priority 1001 \
  --destination-port-ranges 80 \
  --protocol Tcp \
  --access Allow

# List all resources in the group
az resource list --resource-group AZ900-VM-Lab --output table

# Cleanup
az group delete --name AZ900-VM-Lab --yes --no-wait
```

### Cleanup

✅ Deleted resource group `AZ900-VM-Lab` to preserve free credits.

---

## Project 2: Static Website with Azure Storage

**Exam Domain:** Azure Storage, CDN, DNS (15-20% of exam)

### What I Did

- Created a Storage Account (`az900eromosele`) with LRS redundancy in East US
- Enabled the Static Website feature, which auto-created a `$web` blob container
- Created an `index.html` file and uploaded it to the `$web` container
- Fixed the Content-Type from `application/octet-stream` to `text/html` so the browser renders HTML properly
- Accessed the live website at `https://az900eromosele.z13.web.core.windows.net/`
- Explored Access Keys, SAS tokens, Redundancy options, and Metrics

### Screenshot

![Static Website Live] <img width="1222" height="530" alt="Image" src="https://github.com/user-attachments/assets/311dee15-7caa-4fbb-95b5-f52bb5409288" />

*My static website hosted on Azure Blob Storage*

### Key Concepts Learned

- **Azure Storage Accounts** — Globally unique namespace for storing data (blobs, files, queues, tables)
- **Blob Storage** — Optimized for unstructured data like files, images, and videos
- **Access Tiers** — Hot (frequent access, higher storage cost), Cool (infrequent, cheaper), Archive (rare access, cheapest but slow retrieval)
- **Storage Redundancy:**
  - **LRS** — 3 copies in one datacenter
  - **ZRS** — 3 copies across availability zones
  - **GRS** — 6 copies across two regions
  - **RA-GRS** — GRS + read access from secondary region
- **Authentication Methods:**
  - **Access Keys** — Full admin access, rotate regularly
  - **SAS Tokens** — Scoped, time-limited access
  - **Entra ID** — Identity-based access (recommended)
- **Static Website Hosting** — PaaS-like feature on top of Blob Storage, auto-creates `$web` container
- **Content-Type headers** — Critical for browsers to render files correctly

### Resources Created

| Resource | Type | Location |
|----------|------|----------|
| AZ900-Storage-Lab | Resource Group | East US |
| az900eromosele | Storage Account (LRS) | East US |
| $web | Blob Container | — |

### Cleanup

✅ Deleted resource group `AZ900-Storage-Lab` to preserve free credits.

---

## Project 3: Budget & Monitoring Dashboard

*Coming soon...*

---

## Project 4: Identity & Access Management

*Coming soon...*

---

## Project 5: SQL Database + App Service

*Coming soon...*

---

## Project 6: ARM Templates (IaC)

*Coming soon...*

---

## Project 7: VNet Peering & Networking

*Coming soon...*

---

## Exam Study Tracker

| Domain | Weight | Confidence |
|--------|--------|-----------|
| Cloud Concepts | 25-30% | 🟡 Learning |
| Azure Architecture & Services | 35-40% | 🟡 Learning |
| Management & Governance | 30-35% | ⬜ Not Started |

---

## How to Use This Repo

1. Each project folder contains detailed notes and screenshots
2. CLI commands are documented for reproducibility
3. Key concepts are mapped to AZ-900 exam objectives
4. Cleanup steps are included to manage Azure credits

## Resources

- [Microsoft Learn — AZ-900 Learning Path](https://learn.microsoft.com/en-us/training/paths/az-900-describe-cloud-concepts/)
- [Azure Free Account](https://azure.microsoft.com/free)
- [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/)
