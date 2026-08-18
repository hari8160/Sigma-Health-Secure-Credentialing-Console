# Sigma Health — Secure Credentialing Console

A front-end security prototype built for **Code for Communities** (Tech for Good 2026), under the **Good Health & Well-being** track — inspired by Team Sigma's problem statement on hospital credential verification.

> ⚠️ **Scope note:** This is a client-side (HTML/CSS/JavaScript) prototype built to demonstrate web security *concepts and patterns*. It is not a production system, does not store real data anywhere, and does not replace a licensed credentialing process. No data leaves the browser tab.

---

## 📌 Problem Statement

> Shaji J, an administrator at a clinic, manually verifies healthcare professionals' credentials before they can begin practicing or complete the onboarding process. This process is slow, error-prone, and hard to audit.

**Sigma Health** prototypes a credentialing workflow where administrators can review, approve, or reject a clinician's submitted documents — with the security of that workflow treated as a core design requirement, not an afterthought.

---

## ✨ Features

- Authenticated sign-in flow with real-time input validation
- Role-based dashboard (Admin / Verifier / Auditor)
- Credential document upload with client-side validation
- Cryptographic file fingerprinting (SHA-256)
- Approve / reject workflow gated by role permissions
- Tamper-evident audit trail of every action taken
- Transparent security posture panel (what's demo vs. production-ready)

---

## 🔒 Security Techniques Implemented

| Technique | Where it's used |
|---|---|
| **Input validation** | Email format & password strength checked client-side before any submit |
| **Password strength scoring** | Live strength meter (length, case, digits, symbols) |
| **Rate limiting** | Login locks after 5 failed attempts, with exponential backoff |
| **Cryptographic hashing (SHA-256)** | File fingerprinting via `crypto.subtle.digest` to detect tampering |
| **Signed session tokens (JWT-style)** | HMAC-SHA256 signed header.payload.signature token with expiry |
| **Role-Based Access Control (RBAC)** | Approve/reject actions enabled or disabled based on active role |
| **Secure file upload validation** | File type and size checked before acceptance (PDF/PNG/JPG, ≤5MB) |
| **Output encoding / XSS defense** | User-entered notes rendered only via `textContent`, never `innerHTML` |
| **Tamper-evident audit logging** | Each log entry hashes in the previous entry's hash — a broken chain reveals tampering |
| **Principle of least privilege** | Read-only "Auditor" role cannot approve, reject, or upload |

---

## 🛠️ Tech Stack

- HTML5, CSS3 (custom design system, no framework)
- Vanilla JavaScript (ES2020+)
- Web Crypto API (`SubtleCrypto`) for hashing and HMAC signing
- Google Fonts (Fraunces, Inter, IBM Plex Mono)

No build tools, no backend, and no external dependencies beyond web fonts — runs as a single static HTML file.

---

## 🚀 Running Locally

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/sigma-health-credentialing-console.git
   cd sigma-health-credentialing-console
   ```
2. Open `sigma-health-credentialing-console.html` directly in any modern browser (Chrome, Firefox, Edge, Safari).
   - No server or build step required.
   - `crypto.subtle` requires either `localhost` or `https://` — opening the file directly (`file://`) also works in most browsers, but if hashing doesn't run, serve it locally instead:
     ```bash
     python3 -m http.server 8000
     # then visit http://localhost:8000/sigma-health-credentialing-console.html
     ```

---

## 📂 Project Structure

```
sigma-health-credentialing-console/
├── sigma-health-credentialing-console.html   # Full app (HTML + CSS + JS, single file)
└── README.md
```

---

## ⚠️ Production Gaps (Intentionally Not Solved Here)

This prototype is explicit about what it does *not* do, since pretending otherwise would misrepresent the security model:

- No real backend — nothing is actually stored, authenticated against a database, or transmitted over a network.
- Session-signing key is visible in client code for demonstration only; in production this must live server-side and never reach the browser.
- File validation happens only in the browser; a real system must re-validate type, size, and content server-side and scan for malware.
- No actual TLS/HTTPS enforcement, CSP headers, or CORS policy — these require server/infrastructure configuration this prototype doesn't have.
- Rate limiting is per-browser-session only; a real system needs server-side throttling per account/IP.

See the in-app **Security Posture** tab for the full prototype-vs-production comparison table.

---

