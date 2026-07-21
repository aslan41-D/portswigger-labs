# PortSwigger Lab: Multi-step process with no access control on one step

- **Link:** https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step
- **Vulnerability Type:** Access Control Bypass (Incomplete Authorization Checks)
- **Difficulty Level:** Practitioner
- **Date:** 2026-07-20

## Executive Summary

In this lab, the admin role escalation process is designed as a two-step procedure. While authorization checks are performed in the first step, they are bypassed in the second step. This vulnerability allows an unauthorized user (wiener) to elevate a non-admin user (carlos) to admin status.

## Step-by-Step Solution

### 1. Reconnaissance

The role elevation process in the admin panel occurs in two steps:

**Step 1:** Select a user and choose an action (POST request)
**Step 2:** Confirm the action (GET or POST request)

In the first attempt made with an admin session, both steps were successfully completed, and `carlos` was made an admin.

### 2. Test with an Unauthorized User

When attempting to repeat the same process using the `wiener` account, an authorization error (401 or 403) was received in the first step. However, in the second step (confirmation), it was observed that validation was performed only based on the `username=wiener` parameter.

### 3. Identification of the Vulnerability

In the second step, the server only checks the `username` parameter and does not verify whether this user is the same as the one who initiated the process. This allows an attacker to complete a process initiated under an admin session using their own session.

### 4. Exploitation

Using the `wiener` session, direct access was gained to the confirmation URL of the process initiated by the admin session:
GET /admin-roles?username=carlos&action=upgrade&confirm=true

text

**Result:** A `200 OK` response was received, and `carlos` was granted admin privileges. The lab was solved.

### 5. Verification

Upon checking the admin panel, it was confirmed that `carlos` holds the admin role. The lab was successfully completed.

## Result (Remediation)

This vulnerability stems from the failure to perform the same authorization check at every step in multi-step processes. Specifically, during approval steps, it must be ensured that the user initiating the process is the same as the user approving it.

**Recommended Solution:**
- At every step, the user’s session and permission to initiate the process must be verified.
- A unique token must be generated for each transaction, and this token must be tied to the session in which the transaction was initiated.
- In multi-step processes, each step must have its own independent authorization check.

## Tools Used
- Burp Suite (Repeater)
- PortSwigger Academy Lab Environment

## Additional Notes
- Although this lab bears similarities to “Insecure Direct Object References (IDOR),” the primary issue is a lack of authorization controls.
- In real life, vulnerabilities of this type are particularly common in applications that involve workflows.
