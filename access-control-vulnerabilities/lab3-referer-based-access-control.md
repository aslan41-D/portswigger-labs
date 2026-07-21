# PortSwigger Lab: Referer-based access control

- **Link:** https://portswigger.net/web-security/access-control/lab-referer-based-access-control
- **Vulnerability Type:** Access Control Bypass (Referer Header Manipulation)
- **Difficulty Level:** Practitioner
- **Date:** 2026-07-20

## Executive Summary

In this lab, an operation related to user roles in the admin panel (`/admin-roles`) is configured to work only when the `Referer` header points to the admin page (`/admin`). This vulnerability allows an unauthorized user (wiener) to set the `Referer` header to the admin page, thereby elevating a non-admin user (carlos) to administrator status.

## Step-by-Step Solution

### 1. Reconnaissance

An endpoint used to elevate user roles was discovered in the admin panel:
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0a7200790329f3c980c55d4a00ca0057.web-security-academy.net
Cookie: session=<admin_session>
Referer: https://0a7200790329f3c980c55d4a00ca0057.web-security-academy.net/admin

text

When this request was sent using the admin session, it was successful, and `carlos` was granted admin privileges.

### 2. Testing with an Unauthorized User

We logged out of the admin session and logged in as the low-privileged user `wiener`. The same request was repeated using `wiener`’s session cookie and the same Referer header.

**Result:** The request was successful! Because the Referer header still pointed to the admin page.

### 3. Understanding the Vulnerability
The server checks whether the operation was initiated by an admin solely by examining the Referer header. Session information, on the other hand, is used only to verify the user’s identity; it does not check for admin privileges.

### 4. Exploitation

Using the `wiener` session, the same request was repeated by setting the Referer header to the admin page:
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0a7200790329f3c980c55d4a00ca0057.web-security-academy.net
Cookie: session=5lq67ZRW9tJtUKCPjTbKaP7NEYL6ho
Referer: https://0a7200790329f3c980c55d4a00ca0057.web-security-academy.net/admin

text

**Result:** A `302 Found` response was received, and the user was redirected to the `/admin` page. Subsequently, when accessing the `/admin` page, the admin panel was displayed with a `200 OK` response.

<img width="1251" height="620" alt="Ekran Görüntüsü 2026-07-20 14-27-29" src="https://github.com/user-attachments/assets/4ea51967-7f93-4f22-b64f-823f98554690" />

<img width="1251" height="620" alt="Ekran Görüntüsü 2026-07-20 14-27-33" src="https://github.com/user-attachments/assets/05ec2e54-89ec-4744-bf19-05f5972da4c9" />

### 5. Verification

Upon checking the admin panel, it was observed that the `carlos` user had been elevated to the admin role. The lab was successfully completed.

## Result (Remediation)

This vulnerability stems from the server basing its authorization decision solely on the HTTP Referer header. This header can be easily manipulated by the client and is not trustworthy.


