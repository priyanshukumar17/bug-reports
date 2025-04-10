# 🔐 JWT Identity Spoofing – Vulnerability Report

## 📌 Summary

A client-side JWT tampering vulnerability was discovered in the LNMDoubts web application, allowing identity spoofing by modifying token payload data without proper backend validation.

---

## 📂 Brief

The JWT (JSON Web Token) used for authentication is stored in the browser's localStorage. By modifying the token's payload—specifically the `email` and `name` fields—a user can spoof another identity. The app accepts the tampered token without validating the signature, leading to inconsistent access control.

---

## 🚨 Exploitation

1. Log in through Google to obtain a valid token from localStorage.
2. Decode the JWT using tools like [jwt.io](https://jwt.io).
3. Modify the payload — change `email`, `name`, etc.
4. Re-encode the token and replace it in localStorage.
5. Reload the site.

### Behavior:
- In the original session: Full access (can post, comment, etc.)
- In incognito/private session: Read-only access, no posting or commenting.

---

## 🧪 Proof of Concept

```json
// Example payload before modification
{
  "email": "user@lnmiit.ac.in",
  "name": "Original User"
}

// Modified payload
{
  "email": "0xN@cipher.com",
  "name": "0xN"
}
```

> ⚠️ Signature was not modified. Despite this, the app trusted the new identity.

---

## 📉 CVSS Score Summary

| Metric                 | Value                                           |
|------------------------|-------------------------------------------------|
| **CVSS Score**         | 7.6                                             |
| **Severity**           | High                                            |
| **Vector String**      | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:N`  |
| **Attack Vector**      | Network                                         |
| **Attack Complexity**  | Low                                             |
| **Privileges Required**| None                                            |
| **User Interaction**   | None                                            |
| **Scope**              | Unchanged                                       |
| **Confidentiality**    | Low                                             |
| **Integrity**          | High                                            |
| **Availability**       | None                                            |

🔗 [Open in CVSS Calculator](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:N)

---

## 🧩 Root Cause

The application trusts the JWT payload from localStorage without verifying its signature. Firebase Admin SDK or equivalent backend validation is missing or inconsistently implemented. This leads to partial access using tampered tokens.

---

## 🛠️ Prevention

- **Always verify JWTs on the backend** using Firebase Admin SDK.
- Reject tokens that fail signature validation or contain inconsistent claims.
- Never rely solely on localStorage for authentication.
- Add server-side permission checks for actions like posting, commenting, or voting.
- Implement Firebase security rules to block unauthorized token usage.

---

## 🧯 Recovery

To recover from abuse, logs should be reviewed for suspicious actions taken under spoofed identities. Manual corrections (e.g., content removal or user warnings) may be required.

---

## Author

**Priyanshu Kumar**  
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/priyanshukumar17)

[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/PriyanshuOtaku)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/priyanshu-kumar-101514326/)

