# Infrastructure Topology

The Active Directory environment was deployed on an isolated private network segment to simulate a controlled enterprise identity infrastructure without dependence on the external internet.

## Network Configuration

Domain Controller (Dell Server)

IP Address: 10.0.0.15

Subnet Mask: 255.255.255.0

DNS: Self-referenced (10.0.0.15)

No external gateway (isolated environment)

Domain-Joined Client (Windows 10 VM)

IP Address: 10.0.0.2

DNS: 10.0.0.1

Static assignment to ensure consistent domain communication

## Design Rationale

The environment was intentionally isolated to:

Eliminate external routing variables

Enforce dependency on internal DNS resolution

Validate Kerberos-based authentication

Ensure Group Policy retrieval directly from the Domain Controller

Initial NAT-based networking caused DNS resolution failures and GPO communication issues, leading to a redesign using a direct private network configuration.

## Identity Flow

Explaining:

Client → DNS → DC → Kerberos → SYSVOL → GPO → Share access
