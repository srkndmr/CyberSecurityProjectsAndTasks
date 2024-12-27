# 🕵️‍♂️ Phishing Analysis Challenge: SOC-it to 'Em!

## 🕒 The Setup  
You’re a SOC analyst, tasked with analyzing suspicious `.eml` files sent by your colleagues. Your mission is to identify phishing emails, answer key questions, and write a report with screenshots of your analysis tools.

---

## 🎯 Challenge Details  
**Type:** Learning  
**Duration:** 2 Days  
**Team Challenge:** Solo  

### Mission:  
Analyze 5 emails, determine if they are phishing attempts, and document your findings.

---

## 🔍 Key Questions to Answer:  
1. What is the email's timestamp?  
2. Who is the email from?  
3. What is the sender's email address?  
4. What email address will receive a reply?  
5. What brand is being impersonated?  
6. What is the originating IP? (Defanged)  
7. What domain appears suspicious? (Defanged)  
8. What is the shortened URL? (Defanged)  
9. Is this a phishing email?

---

## 🛠️ Tools for Analysis  
- **VirusTotal**: Scan URLs and attachments for malicious content.  
- **PhishTools**: Perform metadata and link analysis.  
- **MX Lookup**: Verify sender domains and mail servers.  
- **PhishTank**: Check for known phishing URLs.  
- **Spamhaus**: Investigate blacklisted IPs and domains.  

---

## 🚨 Action Plan  
1. Open `.eml` files using an email client or text editor.  
2. Analyze email headers to identify sender, recipient, and other metadata.  
3. Inspect email content for suspicious links, attachments, or requests.  
4. Use VirusTotal, PhishTank, and Spamhaus to investigate links, domains, and IPs.  
5. Capture screenshots of tools and findings.  
6. Write a detailed report with your observations.

---

## 📚 Resources  
Complete these TryHackMe rooms for training:  
- [Phishing Emails 1](https://tryhackme.com/room/phishingemails1tryoe)  
- [Phishing Emails 2](https://tryhackme.com/room/phishingemails2rytmuv)  

---

## 💡 Reminder  
Use the NIST Incident Response process:  
**Prepare > Detect > Analyze > Contain > Eradicate > Recover > Post-Incident Handling**
