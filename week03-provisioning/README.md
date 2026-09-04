# CVNP1601 Week 03 - Linux User Provisioning and Least-Privilege Sudo Configuration

## Ticket Information

- **Ticket ID:** CVNP1601-W3-003
- **Submitted By:** Engineering Manager
- **Affected Systems:** Linux User Accounts, Group Management, Sudoers Policy
- **Priority:** Medium
- **Request:** Provision a developer account with group membership and restricted sudo access for a specific administrative task.

---

## Scenario Summary

The Engineering Manager requested the creation of a new Linux user account named `devuser` with membership in the `developers` group. The account required limited administrative privileges that would permit execution of a specific service management task without granting full root access. The objective was to follow the principle of least privilege by allowing only a single approved command through sudo. Verification was required to ensure the authorized command succeeded while unrelated privileged commands were denied. Documentation of account creation, group membership, sudo policy, verification testing, and cleanup activities was also required. The lab simulated a common system administration task involving secure delegation of operational responsibilities. Throughout implementation, account and group configuration settings were validated to ensure accuracy. Several minor command and spelling errors were identified and corrected during the provisioning process. All required administrative actions were documented and verified prior to cleanup. The exercise reinforced user management, group administration, sudo delegation, and verification best practices.

---

## Objectives

- Create a Linux user account named `devuser`
- Create and validate the `developers` group
- Add `devuser` to the `developers` group
- Configure a least-privilege sudoers rule
- Verify authorized administrative access
- Verify unauthorized administrative access is denied
- Document implementation and evidence
- Perform cleanup after verification

---

## Tools Used

- Bash Shell
- useradd
- usermod
- groupadd
- groupdel
- userdel
- id
- grep
- cat
- visudo
- sudo
- Git

---

## Implementation Steps

### 1. Create User Account

```bash
sudo useradd -m -s /bin/bash devuser
