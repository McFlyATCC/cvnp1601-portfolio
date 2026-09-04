# CVNP1601 Week 03
# Linux User Provisioning and Least-Privilege Sudo Configuration

## Ticket Information

| Field | Value |
|---------|---------|
| Ticket ID | CVNP1601-W3-003 |
| Submitted By | Engineering Manager |
| Affected Systems | Linux User Accounts, Group Management, Sudoers Policy |
| Business Impact | Medium |
| Security Consideration | Full sudo access would over-grant privileges. Access must be restricted to a single approved administrative action. |

---

## Report Objective

Provision a Linux user named `devuser`, assign membership to the `developers` group, configure a restricted sudoers policy that permits only a targeted service restart operation, verify both authorized and unauthorized access attempts, document all actions performed, and return the system to its original state after testing.

---

# Provisioning Summary

This ticket required the provisioning of a Linux user account named `devuser` with limited administrative capabilities. The implementation began with collecting baseline system information and reviewing existing user account data. A new user account was created using the `useradd` command and validated with the `id` utility. A developers security group was then created and assigned to the user through group membership modification. During implementation, minor spelling errors occurred while creating the group, but these were quickly identified and corrected through verification commands. The sudoers configuration was reviewed and modified using `visudo` to ensure safe editing and proper syntax validation. A least-privilege access model was implemented by granting permission for only a single approved administrative action rather than full sudo access. Verification procedures included reviewing group membership, displaying authorized sudo permissions with `sudo -l`, and testing delegated access. The provisioning process demonstrated secure user administration practices while maintaining compliance with the ticket's security requirements. After documentation and evidence collection were completed, cleanup activities removed the temporary user account, associated group, and sudoers configuration to return the system to its original state.

---

# Scenario Summary

An Engineering Manager submitted a service request to provision a development account with restricted administrative privileges. The request required creation of a local Linux user named `devuser`, assignment to a newly created `developers` group, and implementation of a least-privilege sudo policy. The administrative delegation needed to allow a specific service restart command while preventing unrelated privileged operations. User and group management tasks were completed through standard Linux administration utilities. The sudoers configuration was modified using `visudo` to ensure syntax validation and reduce the risk of configuration errors. Verification was performed by reviewing group membership information and examining assigned sudo permissions with `sudo -l`. Testing confirmed that only approved administrative operations were permitted. Additional testing demonstrated that unauthorized privileged commands were denied as expected. All actions were documented and evidence was collected throughout the implementation. After verification, cleanup procedures removed the temporary account, groups, and delegated permissions.

---

# Commands Executed

## Environment Preparation

```bash
mkdir week03-provisioning
mkdir week03-provisioning/screenshots
```

## Evidence Collection

```bash
nano collect-evidence.sh
chmod +x collect-evidence.sh
./collect-evidence.sh
```

## User Account Review

```bash
cat /etc/passwd | head -5
sudo cat /etc/shadow | grep root
sudo cat /etc/shadow | grep mcfly
```

## User Creation

```bash
sudo useradd -m -s /bin/bash devuser
```

Verification:

```bash
id devuser
```

## Group Creation

Initial attempts:

```bash
sudo groupadd devolpers
sudo groupadd develpers
```

Successful creation:

```bash
sudo groupadd developers
```

Verification:

```bash
grep developers /etc/group
```

## Group Assignment

```bash
sudo usermod -aG developers devuser
```

Verification:

```bash
id devuser
```

## Group File Review

```bash
cat /etc/group
```

## Sudo Configuration

View sudoers:

```bash
sudo cat /etc/sudoers
```

Edit sudoers safely:

```bash
sudo visudo
```

Example entry:

```text
devuser ALL=(root) NOPASSWD: /usr/bin/systemctl restart apache2
```

## Permission Verification

```bash
sudo -u devuser sudo -l
sudo -l
```

---

# Verification Results

## User Created

Verification command:

```bash
id devuser
```

Expected result:

```text
uid=xxxx(devuser)
```

Status:

✅ Successful

---

## Developers Group Created

Verification command:

```bash
grep developers /etc/group
```

Status:

✅ Successful

---

## Group Membership Assigned

Verification command:

```bash
id devuser
```

Expected output includes:

```text
developers
```

Status:

✅ Successful

---

## Sudo Configuration Applied

Verification command:

```bash
sudo -u devuser sudo -l
```

Status:

✅ Successful

---

## Authorized Command Test

Example command:

```bash
sudo -u devuser sudo systemctl restart apache2
```

Expected Result:

```text
Command executes successfully.
```

Status:

✅ Verified

---

## Unauthorized Command Test

Example command:

```bash
sudo -u devuser sudo systemctl restart ssh
```

Expected Result:

```text
Permission denied.
```

Status:

✅ Verified

---

# Troubleshooting Notes

## Issue 1: Group Name Typographical Errors

Incorrect commands:

```bash
sudo groupadd devolpers
sudo groupadd develpers
```

Root Cause:

Misspelling of the intended group name.

Resolution:

```bash
sudo groupadd developers
```

Result:

Group created successfully.

---

## Issue 2: Incorrect Command Name

Incorrect command:

```bash
sudo visduo
```

Root Cause:

Command typo.

Resolution:

```bash
sudo visudo
```

Result:

Sudoers file edited successfully.

---

# Evidence Checklist

- [x] Baseline evidence collected
- [x] User account created
- [x] User account verified
- [x] Developers group created
- [x] Group membership assigned
- [x] Group membership verified
- [x] Sudoers configuration reviewed
- [x] Sudoers policy configured
- [x] Sudo permissions reviewed using `sudo -l`
- [x] Authorized command tested
- [x] Unauthorized command tested
- [x] Documentation completed
- [x] Cleanup performed

---

# Cleanup Procedure

After documentation was completed, the temporary account and associated groups were removed.

Remove the sudoers entry:

```bash
sudo visudo
```

Delete the user account:

```bash
sudo userdel -r devuser
```

Delete the developers group:

```bash
sudo groupdel developers
```

Remove incorrectly created groups:

```bash
sudo groupdel devolpers
sudo groupdel develpers
```

---

# Skills Demonstrated

- Linux User Administration
- Linux Group Management
- Privilege Delegation
- Principle of Least Privilege
- Sudoers Configuration
- Access Verification
- Troubleshooting and Validation
- Evidence Collection
- Security Administration
- Technical Documentation

---

# Lessons Learned

This lab reinforced the importance of validating administrative actions after every configuration change. User and group creation tasks should always be verified using commands such as `id` and `grep` before proceeding to additional configuration steps. Editing the sudoers file should always be performed through `visudo` to reduce the risk of syntax errors that could impact system administration. The exercise also demonstrated how least-privilege access can provide necessary operational functionality while reducing security risk. Finally, documenting mistakes, troubleshooting steps, verification activities, and cleanup actions provides a complete audit trail and creates professional-quality technical documentation.

---

# AI Usage Disclosure

Microsoft Copilot was used to assist with troubleshooting analysis, command review, verification planning, documentation structure, and report generation. All commands were executed, validated, and documented by the student as part of the lab activity.

---

# Portfolio Card

**Project:** Linux User Provisioning and Least-Privilege Sudo Administration  
**Course:** CVNP1601  
**Week:** 03  
**Skills Practiced:** User Management, Group Administration, Sudoers Configuration, Security Hardening, Validation, Documentation  
**Outcome:** Successfully provisioned a Linux user account with restricted administrative privileges, validated access controls, documented verification evidence, and removed temporary resources through cleanup procedures.
