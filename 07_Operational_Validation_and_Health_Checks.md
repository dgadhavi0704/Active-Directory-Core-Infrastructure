# Operational Validation and Health Checks

---

## 1. Domain Controller Validation

After promotion, the following validations were performed to ensure domain integrity:

- Verified Domain network profile
- Confirmed creation of SYSVOL and NETLOGON shares
- Validated Forward Lookup Zone creation
- Confirmed SRV record registration

DNS and AD services were verified to be functioning before onboarding client systems.

---

## 2. DNS Health Validation

DNS integrity was validated using:

- `ipconfig /all`
- `nslookup <FQDN>`
- Forward lookup zone inspection

### Validation Checks

- Domain Controller FQDN resolves to 10.0.0.15
- SRV records (_ldap._tcp) present
- Client DNS points exclusively to Domain Controller

Successful resolution confirmed correct identity dependency chain.

---

## 3. Domain Join Validation

Domain join was tested using a Windows 10 virtual machine.

### Validation Criteria

- Successful authentication using domain credentials
- Automatic computer object placement in ComputerDG OU
- Confirmation of proper DNS resolution
- Network profile switched to Domain

Successful join validated directory communication and trust relationship.

---

## 4. Group Policy Validation

Policy application was validated using:

- `gpupdate /force`
- `gpresult /r`
- Logoff and logon cycle

### Validation Criteria

- Target GPO appears under Applied Group Policy Objects
- GPO not displayed as N/A
- Security filtering correctly enforced
- User-based restrictions confirmed

Policy enforcement confirmed correct scope, filtering, and token evaluation.

---

## 5. Access Control Validation

Shared resource (CompanyData) access was tested using non-administrative accounts.

### IT User

- Modify capability confirmed
- File creation and deletion validated

### HR User

- Read-only access confirmed
- Write operations denied

Layered permission enforcement confirmed functional RBAC model.

---

## 6. Identity Processing Flow Confirmation

The following identity chain was validated:

- Client resolves Domain Controller via DNS
- Kerberos authentication issued by DC
- SYSVOL accessible for policy retrieval
- GPO applied during user logon
- Resource access evaluated via group membership

Successful execution confirmed operational integrity of:

- DNS
- AD DS
- Kerberos
- Group Policy
- NTFS access control

---

## 7. Operational Stability Observations

- Static IP configuration ensured consistent DNS registration
- Network profile consistency required for domain trust
- Security filtering errors immediately impacted policy scope
- Group membership changes required re-authentication

The environment demonstrated stable identity and policy operations after configuration corrections.
