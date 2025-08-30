# 🔐 Broken Authentication Security Lab Project

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Tool](https://img.shields.io/badge/Tool-Burp%20Suite-orange)
![Category](https://img.shields.io/badge/Category-OWASP%20Top%2010-red)
![Language](https://img.shields.io/badge/Language-MD-blue)

## 🧪 Using Burp Suite for Authentication & Session Testing

**Author:**  
**Date:**  

---

## 📖 Executive Summary
Broken Authentication is one of the **OWASP Top 10 vulnerabilities**. It occurs when applications improperly implement login, session, or password reset mechanisms, allowing attackers to compromise accounts.  

In this lab, we tested an intentionally vulnerable OWASP web application at `192.168.29.186` using **Burp Suite**. The goal was to identify authentication flaws, demonstrate them safely in a lab, and provide security recommendations.

---

## ⚙️ Lab Setup
- **Target VM:** OWASP DVWA / BWA / Juice Shop at `192.168.29.186`  
- **Attacker VM:** Kali Linux with Burp Suite Community Edition  
- **Browser:** Firefox configured with Burp proxy (`127.0.0.1:8080`)  
- **Test Accounts:** victim:test123, attacker:hacker123 (lab only)  
- **Network:** Host-only adapter (`192.168.29.0/24`)  

---

## 🛠️ Step-by-Step Testing Procedures

### 🔑 1. Weak Login Protections
- Intercepted login requests with Burp Proxy.  
- Replayed requests with Burp Repeater.  
- Observed verbose error messages (*invalid username vs. invalid password*).  
- No account lockout or rate limiting found.  

### 🍪 2. Cookie Security
- Captured `Set-Cookie` headers.  
- Checked for `Secure`, `HttpOnly`, and `SameSite` flags.  
- Tested tampered cookies for server acceptance.  
- Identified long expiry times and predictable values.  

### 🌀 3. Session Fixation
- Captured session cookie before login.  
- Verified no cookie rotation after login.  
- Reused old cookie successfully post-logout.  

### 🔐 4. Forgot Password Flow
- Intercepted reset token during password reset.  
- Token appeared predictable (Base64, sequential IDs).  
- Token reuse possible.  
- Tokens did not expire after single use.  

### 🚪 5. Authorization Bypass
- Accessed authenticated endpoints without cookies.  
- Modified user IDs to check for **IDOR**.  
- Server allowed unauthorized access.  

---

## 📂 Evidence Collection
- Burp project file (`.burp`) with all captured requests/responses.  
- Request/response pairs stored in Burp Repeater.  
- Recorded cookie attributes, tokens, and errors.  
- Documentation placeholders available.  

---

## 📊 Findings Table

| ID    | Category        | Severity | Description                  | Evidence                | Remediation |
|-------|----------------|----------|------------------------------|-------------------------|-------------|
| BA-01 | Weak Login     | 🔴 High  | Unlimited login attempts     | Burp Repeater logs      | Implement rate limiting & lockout |
| BA-02 | Cookie         | 🟠 Medium| Missing HttpOnly flag        | Set-Cookie response     | Add HttpOnly, Secure, SameSite flags |
| BA-03 | Session        | 🟠 Medium| No session rotation on login | Cookie comparison       | Rotate session IDs at login/logout |
| BA-04 | Reset Flow     | 🔴 High  | Token reuse possible         | Reset link evidence     | Enforce single-use tokens with expiry |
| BA-05 | Authorization  | 🔴 High  | Direct access without login  | Burp Repeater requests  | Apply server-side access control |

---

## 🛡️ Mitigation & Recommendations
- Enforce **account lockout**, CAPTCHA, and MFA for login.  
- Secure cookies with `Secure`, `HttpOnly`, and `SameSite` flags.  
- Rotate sessions after login and invalidate them after logout.  
- Use **cryptographically strong, single-use reset tokens** with expiry.  
- Apply strict **server-side authorization checks**.  

---

## ✅ Conclusion
This project demonstrated **Broken Authentication vulnerabilities** using Burp Suite against a vulnerable OWASP target.  

By applying the recommended mitigations, organizations can significantly reduce risks of **account compromise** and **unauthorized access**.  

---

### 📌 References
- [OWASP Top 10 (Authentication)](https://owasp.org/www-project-top-ten/)  
- [Burp Suite Documentation](https://portswigger.net/burp)  

