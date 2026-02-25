# Active Directory Core Infrastructure

---

## Executive Summary

This repository documents the architecture, deployment, and operational validation of a structured on-premises Active Directory environment designed to simulate enterprise-grade identity infrastructure.

The project establishes the foundational domain layer required for secure identity management, policy enforcement, and controlled resource access. Core components include:

- Active Directory Domain Services (AD DS)
- Integrated DNS
- Organizational Unit (OU) design
- Security group modeling
- Group Policy implementation
- Layered Share and NTFS access control
- Structured troubleshooting and validation

This repository represents the on-prem identity foundation that later integrates into a hybrid identity model with Microsoft Entra ID.

---

## Architecture Overview

The environment was deployed in an isolated private network segment to simulate controlled enterprise identity operations.

### Infrastructure Components

- Domain Controller (AD DS + DNS)
- Windows 10 domain-joined client
- Static IP configuration
- Dedicated OU structure
- Group-based access model
- Policy-controlled environment

For detailed topology and design rationale, see:

- [01_Architecture_Overview.md](01_Architecture_Overview.md)

---

## Core Implementation Areas

### Domain Controller Deployment

- Static IP planning
- DNS integration and SRV record validation
- Forest and domain configuration
- FSMO role awareness
- Default container redirection using `redircmp`

See:

- [02_Domain_Controller_Deployment.md](02_Domain_Controller_Deployment.md)

---

### Organizational Design and Security Model

- OU-based policy boundaries
- Security vs distribution group separation
- Role-Based Access Control (RBAC)
- Security filtering design
- Authenticated Users vs Domain Users analysis

See:

- [03_Organizational_Design_and_Security_Model.md](03_Organizational_Design_and_Security_Model.md)

---

### Group Policy Implementation

- User vs Computer configuration separation
- GPO linking strategy
- Security filtering mechanics
- Token refresh behavior
- Structured troubleshooting approach

See:

- [04_Group_Policy_Implementation.md](04_Group_Policy_Implementation.md)

---

### Access Control Model

- Share vs NTFS layered enforcement
- Group-based authorization
- Effective permission validation
- Non-admin testing approach

See:

- [05_Access_Control_and_Permission_Model.md](05_Access_Control_and_Permission_Model.md)

---

### Troubleshooting Case Studies

Real-world configuration failures were intentionally and unintentionally encountered and resolved, including:

- DNS resolution failures
- GPO "N/A" condition
- Security filtering misconfiguration
- Logon token refresh issues
- APIPA addressing and network isolation
- Network profile inconsistencies
- Share permission misalignment

See:

- [06_Troubleshooting_Case_Studies.md](06_Troubleshooting_Case_Studies.md)

---

### Operational Validation

Post-deployment validation included:

- DNS health verification
- Domain join confirmation
- GPO enforcement validation
- Share access testing
- Identity processing flow confirmation

See:

- [07_Operational_Validation_and_Health_Checks.md](07_Operational_Validation_and_Health_Checks.md)

---

## Skills Demonstrated

- Active Directory Domain Services deployment
- DNS integration and troubleshooting
- OU and security boundary design
- Group Policy architecture and validation
- RBAC implementation
- Layered access control enforcement
- Structured infrastructure troubleshooting
- Identity dependency analysis
- Operational validation methodology

---

## Relationship to Hybrid Identity Project

This repository represents the on-premises identity foundation that integrates with:

- Microsoft Entra ID
- Entra Connect (Password Hash Synchronization)
- Seamless Single Sign-On
- Scoped synchronization strategies

Hybrid implementation details are documented separately.
