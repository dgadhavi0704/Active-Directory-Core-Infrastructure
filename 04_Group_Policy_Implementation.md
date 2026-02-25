# Group Policy Implementation

---

## 1. GPO Architecture Overview

Group Policy was implemented to enforce user and system configuration within defined Organizational Units.

Key concepts validated:

- GPOs apply to OUs, not groups
- Security filtering controls who can apply a GPO
- Read permission is required before Apply is evaluated
- User and Computer configurations process independently

---

## 2. User vs Computer Configuration

Group Policy contains two independent sections:

- Computer Configuration
- User Configuration

### Processing Behavior

- Computer Configuration applies during system startup
- User Configuration applies during user logon
- Both refresh periodically in the background
- Manual refresh can be triggered using `gpupdate /force`

A mismatch between configuration type and target object results in the GPO not applying.

---

## 3. GPO Linking Strategy

GPOs were linked directly to:

- UsersDG (for user-based restrictions)

No GPOs were linked to:

- Groups container
- Default containers (CN=Users, CN=Computers)

This ensured predictable scope control.

---

## 4. Security Filtering Model

Security filtering was implemented using security groups.

### Required Configuration

- Authenticated Users → Read
- IT-Users → Read and Apply Group Policy

### Critical Observation

Removing Authenticated Users resulted in:

- GPO appearing as N/A in `gpresult`
- Policy evaluation stopping before Apply permission

### Why This Happens

- GPO evaluation requires Read permission first
- Domain Users does not include computer accounts
- Authenticated Users includes both users and computers

Without Read permission, policy processing does not proceed.

---

## 5. GPO Validation Methods

Policy enforcement was validated using:

- `gpupdate /force`
- `gpresult /r`
- Logoff and logon cycle

### gpresult Analysis

If a GPO does not appear:

- It is not in scope
- Security filtering may be incorrect
- User token may require refresh

If a GPO appears as N/A:

- Scope or Read permission is misconfigured

---

## 6. Token Refresh Behavior

Group membership changes require:

- User logoff and logon

Group membership is evaluated during logon when the security token is created.

Failure to re-authenticate results in outdated group membership and policy misapplication.

---

## 7. Common Failure Scenarios Identified

- GPO linked to incorrect OU
- Authenticated Users removed from Read permission
- User added to group without re-login
- Configuration mismatch (User setting evaluated under Computer scope)
- Network profile not set to Domain

Each issue was identified and resolved through structured troubleshooting.

---

## 8. Operational Lessons

- Scope determines eligibility
- Security filtering determines authorization
- Read permission is mandatory for evaluation
- Token refresh is required after group changes
- GPO troubleshooting must follow evaluation order
