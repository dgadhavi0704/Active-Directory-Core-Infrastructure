3. Group Design Model

Security groups were separated from user OUs and placed inside a dedicated Groups container.

Groups Created

IT-Users (Security Group)

HR-Users (Security Group)

HR-Distribution (Distribution Group)

Design Principles

Security groups are used for permission enforcement

Distribution groups are used for communication only

No direct user-to-resource assignments

Access is granted through group membership

Aligns with Role-Based Access Control (RBAC)

Group location does not affect GPO scope. OU placement controls policy. Group membership controls access.

4. Security Filtering and GPO Scope

Group Policy was implemented using security filtering.

Required Configuration

Authenticated Users → Read

Target Security Group (e.g., IT-Users) → Read and Apply Group Policy

Observed Issue

Removing Authenticated Users caused the GPO to appear as N/A in gpresult.

Root Cause

GPO processing requires Read permission

Domain Users is not equal to Authenticated Users

Computer accounts are included in Authenticated Users

Without Read permission, policy evaluation stops

Evaluation Flow

OU scope validation

Read permission check

Apply permission check

Logon token evaluation

Improper filtering results in silent policy failure.

5. Access Control Model (Share and NTFS)

A shared resource (CompanyData) was created to validate layered access control.

Share Permissions

IT-Users → Modify

HR-Users → Read

NTFS Permissions

IT-Users → Modify

HR-Users → Read

Validation Results

IT user confirmed modify capability

HR user confirmed read-only access

Access denied enforced correctly

Key Principle

NTFS permissions are authoritative.

Access is granted only when both Share and NTFS permissions allow it.

6. Identity Flow Validation

The authentication and policy flow was validated as:

Client → DNS → Domain Controller → Kerberos → SYSVOL → GPO → Resource Access

Successful domain join, GPO enforcement, and share access confirmed correct identity processing.

7. Design Lessons

OU placement defines policy scope.

Groups control access, not GPO location.

Authenticated Users must retain Read permission.

Default containers should be redirected.

Security filtering errors can silently break policy enforcement.
