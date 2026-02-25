# Domain Controller Deployment

## 1. Server Role Planning

-- **The Domain Controller was deployed as a dedicated identity server responsible for:**

- Active Directory Domain Services (AD DS)

- Integrated DNS

- Authentication (Kerberos)

- Group Policy distribution

- SYSVOL replication and policy storage

-- The server was not configured as a general-purpose machine. It was dedicated to identity infrastructure.

## 2. Network Prerequisites
-- **Static IP Configuration**

A static IP address was configured before domain promotion.

IP Address: 10.0.0.15

Subnet Mask: 255.255.255.0

Preferred DNS: 10.0.0.15 (self-referenced)

-- Why Static IP is Mandatory?

-- **Because Active Directory relies on:**

- DNS record consistency
- Service Principal Name (SPN) registration
- Kerberos authentication mapping
- Domain locator records (_ldap._tcp)

-- **Dynamic addressing introduces instability in:**
- DNS resolution
- Domain controller discovery
- SYSVOL path resolution

-- **Initial misconfiguration of static IP resulted in:
**
- “Media disconnected” state
- DNS resolution failures
- Inability to complete the domain controller promotion

## 3. DNS Role Installation and Integration

-- **DNS was installed alongside AD DS, and Active Directory was configured with:**

- AD-integrated DNS zones
- Secure dynamic updates
- Automatic SRV record registration

-- **Troubleshooting Encountered during validation:**

- nslookup returned "Unknown"
- Forward lookup zone resolution failed
- DNS suffix inconsistencies were identified

-- **Root cause analysis revealed:**

- Incorrect DNS server assignment (127.0.0.1 instead of static IP)
- Network profile incorrectly set to Public
- Improper DNS registration sequence

-- **Corrective actions included:**

- Explicit DNS server reassignment to 10.0.0.15
- Adapter reset to force domain profile detection
- Validation using ipconfig /all and nslookup

## 4. Domain Promotion

The server was promoted to Domain Controller for:

YKT.dhruv

Forest and domain functional levels were set appropriately for a single-domain environment.

Post-Promotion Changes Observed

Network profile shifted from Public to Domain

SYSVOL and NETLOGON shares created

FSMO roles assigned to initial DC

DNS zone automatically created and populated

## 5. FSMO Role Awareness

-- **Although a single DC environment, awareness of FSMO roles was validated:**

- Schema Master
- Domain Naming Master
- RID Master
- PDC Emulator
- Infrastructure Master

-- **Particular attention was given to:**

- PDC Emulator Role

Why? 
-- **It is critical because it:**

Acts as authoritative time source, handles password updates, coordinates account lockouts and manages time consistency, which  is required for Kerberos authentication (5-minute skew tolerance).

## 6. Default Container Redirection

By default, new computer accounts are placed in:

CN=Computers

This container does not allow GPO linkage to enforce structured OU-based policy management. Using the command below:

redircmp "OU=ComputerDG,DC=YKT,DC=dhruv" -- (ComputerDG is a new OU that I created in the AD users and Computers App)

I ensured newly joined machines were automatically placed in the designated OU for policy scoping.

## 7. Initial Validation

-- **Post-deployment validation included:**

- Successful DNS resolution of DC FQDN
- Successful self-ping using FQDN
- Verification of SYSVOL and NETLOGON shares
- Domain profile confirmation

-- Domain join testing was done through Windows 10 VM on my laptop.

## 8. Infrastructure Lessons Learned

- Active Directory is DNS-dependent.
- Network profile state affects domain trust.
- Static IP must be configured before promotion.
- Default containers should be redirected to OUs for structured policy control.
- Improper security filtering can cause GPO "N/A" state even when linked correctly.
