
# 🔒 Verifying Website Legitimacy Using SSL Certificate Information

As a **Security Operations Center (SOC) Analyst**, ensuring the legitimacy of a website is critical to defending your organization's network. This guide outlines a **step-by-step verification process** using SSL certificate information.

---

## ✅ Step 1: Verify the Domain Information

🔍 **What to Check:**
- **Common Name (CN):** `tryhackme.com`

💡 **How to Verify:**
- Visit `https://tryhackme.com` in a browser.
- Confirm the presence of a **lock icon** 🔒 in the URL bar, which indicates a secure connection.

---

## ✅ Step 2: Validate the Certificate Authority (CA)

🔍 **What to Check:**
- **Issued By → Common Name (CN):** `WE1`
- **Issued By → Organisation (O):** `Google Trust Services`

💡 **How to Verify:**
- Ensure the CA (Certificate Authority) is recognized and trusted (Google Trust Services is legitimate).
- If the issuer is unfamiliar, research it online.

---

## ✅ Step 3: Verify Certificate Validity Period

🔍 **What to Check:**
- **Issued On:** `Sunday, 2 February 2025`
- **Expires On:** `Saturday, 3 May 2025`

💡 **How to Verify:**
- Confirm that the current date falls within the certificate's validity range.

---

## ✅ Step 4: Check SHA-256 Fingerprints

🔍 **What to Check:**
- **Certificate Fingerprint:**
  - `7c64cd0059e68c770f122072db29964c0a5c02ceb54f7281a9f1613b9dd8a49c`
- **Public Key Fingerprint:**
  - `2393c970088d59dffde2f2b64cd695a050670803797132251fa42f85c8102aed`

💻 **Command to Run:**
```bash
openssl s_client -connect tryhackme.com:443 | openssl x509 -noout -fingerprint -sha256
```

💡 **How to Verify:**
- Match the output fingerprint with the official certificate details.

---

## ✅ Step 5: Use Online SSL Tools

🔧 **Recommended Tools:**
- [SSL Labs SSL Test](https://www.ssllabs.com/ssltest/)
- [Certificate Transparency Logs (crt.sh)](https://crt.sh/)

💡 **How to Verify:**
- Enter `tryhackme.com` and check for valid SSL configurations and warnings.

---

## ✅ Step 6: Check DNS Records

💻 **Commands to Run:**
```bash
nslookup tryhackme.com
```
```bash
dig tryhackme.com
```

💡 **How to Verify:**
- Ensure the IP address corresponds to legitimate servers.

---

## ✅ Step 7: Analyze WHOIS Information

🔧 **Tool:**
- [Whois Lookup](https://who.is/)

💡 **What to Verify:**
- Registrar details 📜
- Domain creation/expiration dates 🗓️
- Registered organization 🏢

---

## ✅ Step 8: Analyze Network Traffic (Advanced)

🔍 **Tools for Deep Analysis:**
- **[Wireshark](https://www.wireshark.org/)** → Inspect packet data 📊
- **[Nmap](https://nmap.org/)** → Scan open ports and running services 🔍

---

## ✅ Step 9: Check for Phishing or Malware

🔧 **Recommended Tools:**
- [Google Safe Browsing](https://transparencyreport.google.com/safe-browsing/search)
- [VirusTotal](https://www.virustotal.com/)

💡 **How to Verify:**
- Confirm the website isn’t flagged for malicious activity.

---

## ✅ Step 10: Log Your Findings

📝 **As a SOC Analyst:**
- Document each step thoroughly 🖊️
- Report any discrepancies 🚨
- Raise alerts for potential threats 🔔
- Follow appropriate escalation protocols 📋

---

## 👨‍💻 **Stay Secure!**

Following this process ensures your organization remains protected from fraudulent websites and potential security threats. 🔐

> *"Security is not a product, but a process." – Bruce Schneier*
