# Domain Controller Deployment

## 1. Server Role Planning

-- The Domain Controller was deployed as a dedicated identity server responsible for:

- Active Directory Domain Services (AD DS)

- Integrated DNS

- Authentication (Kerberos)

- Group Policy distribution

- SYSVOL replication and policy storage

-- The server was not configured as a general-purpose machine. It was dedicated to identity infrastructure.

## 2. Network Prerequisites
-- Static IP Configuration

A static IP address was configured before domain promotion.

IP Address: 10.0.0.15

Subnet Mask: 255.255.255.0

Preferred DNS: 10.0.0.15 (self-referenced)

-- Why Static IP is Mandatory?

-- Because Active Directory relies on:

- DNS record consistency
- Service Principal Name (SPN) registration
- Kerberos authentication mapping
- Domain locator records (_ldap._tcp)

-- Dynamic addressing introduces instability in:
- DNS resolution
- Domain controller discovery
- SYSVOL path resolution

-- Initial misconfiguration of static IP resulted in:

- “Media disconnected” state
- DNS resolution failures
- Inability to complete the domain controller promotion

## 3. DNS Role Installation and Integration

-- DNS was installed alongside AD DS.

-- Active Directory was configured with:

- AD-integrated DNS zones
- Secure dynamic updates
- Automatic SRV record registration

-- Troubleshooting Encountered

-- During validation:
- nslookup returned "Unknown"
- Forward lookup zone resolution failed
- DNS suffix inconsistencies were identified

****Root cause analysis revealed:****

Incorrect DNS server assignment (127.0.0.1 instead of static IP)

Network profile incorrectly set to Public

Improper DNS registration sequence

Corrective actions included:

Explicit DNS server reassignment to 10.0.0.15

Adapter reset to force domain profile detection

Validation using ipconfig /all and nslookup

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

Although a single DC environment, awareness of FSMO roles was validated:

Schema Master

Domain Naming Master

RID Master

PDC Emulator

Infrastructure Master

Particular attention was given to:

PDC Emulator Role

Critical because it:

Acts as authoritative time source

Handles password updates

Coordinates account lockouts

Time consistency is required for Kerberos authentication (5-minute skew tolerance).

## 6. Default Container Redirection

By default, new computer accounts are placed in:

CN=Computers

This container does not allow GPO linkage.

To enforce structured OU-based policy management:

redircmp "OU=ComputerDG,DC=YKT,DC=dhruv"

This ensured newly joined machines were automatically placed in the designated OU for policy scoping.

## 7. Initial Validation

Post-deployment validation included:

Successful DNS resolution of DC FQDN

Successful self-ping using FQDN

Verification of SYSVOL and NETLOGON shares

Domain profile confirmation

Domain join testing from Windows 10 VM

## 8. Infrastructure Lessons Learned

Active Directory is DNS-dependent.

Network profile state affects domain trust.

Static IP must be configured before promotion.

Default containers should be redirected to OUs for structured policy control.

Improper security filtering can cause GPO "N/A" state even when linked correctly.
