# Troubleshooting Case Studies

---

## 1. DNS Resolution Failure During Domain Validation

### Issue

- `nslookup` returned "Unknown"
- Client unable to resolve Domain Controller FQDN
- Domain profile not switching from Public to Domain

### Root Cause

- Incorrect DNS server assignment (127.0.0.1 instead of static IP)
- DNS records not properly registered
- Network profile not correctly detected

### Diagnostic Steps

- Verified `ipconfig /all`
- Checked Preferred DNS configuration
- Tested FQDN resolution using `nslookup`
- Restarted network adapter to force profile reassessment

### Resolution

- Set Preferred DNS to 10.0.0.15
- Reset adapter to trigger Domain network detection
- Confirmed forward lookup zone integrity

### Lesson

Active Directory is DNS-dependent.  
Incorrect DNS configuration breaks authentication and policy processing.

---

## 2. GPO Appearing as "N/A" in gpresult

### Issue

- GPO linked correctly to OU
- User member of target security group
- `gpresult` displayed GPO as N/A

### Root Cause

- Authenticated Users removed from Read permission
- GPO evaluation stopped before Apply permission

### Diagnostic Steps

- Verified OU scope
- Reviewed Security Filtering
- Checked Delegation tab permissions
- Compared Domain Users vs Authenticated Users

### Resolution

- Restored Authenticated Users with Read permission
- Ensured target group retained Read and Apply
- Logged off and re-authenticated user

### Lesson

GPO processing requires Read permission before Apply is evaluated.  
Domain Users is not equivalent to Authenticated Users.

---

## 3. Group Membership Not Updating

### Issue

- User added to security group
- Policy did not apply immediately

### Root Cause

- Logon token created before group membership change
- Security token not refreshed

### Diagnostic Steps

- Verified group membership in AD
- Checked `gpresult`
- Confirmed user had not logged out

### Resolution

- User logged off and logged back in
- Policy applied successfully

### Lesson

Group membership is evaluated at logon.  
Token refresh requires re-authentication.

---

## 4. APIPA Address on Virtual Machine

### Issue

- VM received 169.254.x.x address
- Unable to communicate with Domain Controller

### Root Cause

- VM network adapter misconfigured
- NAT mode prevented proper domain communication
- No DHCP in isolated environment

### Diagnostic Steps

- Checked network adapter settings
- Verified IP configuration
- Reviewed adapter mode (NAT vs Host-only)

### Resolution

- Configured static IP (10.0.0.2)
- Set DNS to 10.0.0.15
- Validated connectivity with ping and domain join

### Lesson

Active Directory environments require consistent IP and DNS configuration.  
NAT networking can introduce DNS and discovery issues in isolated labs.

---

## 5. Network Profile Stuck on Public

### Issue

- Domain Controller network profile remained Public
- Domain authentication unreliable

### Root Cause

- DNS resolution incomplete during initial configuration
- Adapter state change required

### Diagnostic Steps

- Checked network status
- Verified DNS resolution
- Restarted adapter service

### Resolution

- Disabled and re-enabled adapter
- Confirmed switch to Domain profile

### Lesson

Network profile state affects domain trust behavior.  
DNS integrity must exist before domain profile activation.

---

## 6. Share Access Denied Despite Group Membership

### Issue

- User member of correct security group
- Access denied when opening shared folder

### Root Cause

- NTFS permissions misaligned with share permissions
- Access evaluated at both layers

### Diagnostic Steps

- Reviewed Share permissions
- Reviewed NTFS permissions
- Tested with alternate user account

### Resolution

- Aligned NTFS permissions with intended access model
- Revalidated with non-admin user

### Lesson

NTFS is the authoritative permission layer.  
Effective permissions require evaluation of both Share and NTFS.
