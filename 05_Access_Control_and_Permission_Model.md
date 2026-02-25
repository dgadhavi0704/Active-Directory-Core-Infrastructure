# Access Control and Permission Model

---

## 1. Access Control Strategy

Access to shared resources was implemented using a layered permission model based on security groups.

No permissions were assigned directly to individual user accounts.

Design principles:

- Access is granted through security groups
- Users inherit access via group membership
- Role-Based Access Control (RBAC) model enforced
- Separation between Share permissions and NTFS permissions maintained

---

## 2. Share-Level Permissions

A shared folder named `CompanyData` was created on the Domain Controller for access validation.

### Share Permissions Configured

- IT-Users → Modify
- HR-Users → Read

Share permissions provide the first layer of access control when accessing resources over the network.

However, they are not the final authority.

---

## 3. NTFS Permissions

NTFS permissions were configured directly on the folder to enforce granular access control.

### NTFS Permissions Configured

- IT-Users → Modify
- HR-Users → Read

### Why NTFS Matters

- NTFS permissions apply both locally and over the network
- NTFS is the authoritative layer in access evaluation
- If NTFS denies access, share permissions cannot override it

---

## 4. Effective Permission Logic

Access to a shared resource is evaluated as follows:

- Share permission is evaluated
- NTFS permission is evaluated
- The most restrictive combination applies

Example:

- If Share allows Modify
- But NTFS allows only Read
- Final result = Read

This layered evaluation prevents unintended privilege escalation.

---

## 5. Validation Testing

Access validation was performed using domain user accounts.

### IT User

- Successfully created, modified, and deleted files
- Confirmed Modify permission

### HR User

- Able to open and read files
- Unable to modify or delete content
- Access denied correctly enforced

This confirmed correct group-based enforcement.

---

## 6. Common Misconfiguration Risks

- Assigning permissions directly to user accounts
- Granting Full Control at share level unnecessarily
- Ignoring NTFS when troubleshooting access issues
- Over-permissioning service accounts

Improper permission design increases security risk and operational complexity.

---

## 7. Security Lessons Learned

- Always assign access through security groups
- Separate identity (user) from authorization (group)
- NTFS is the authoritative control layer
- Test with real user accounts, not administrative accounts
- Validate both Share and NTFS when troubleshooting

A structured permission model reduces administrative overhead and enforces consistent access boundaries.
