# SOC114 — Malicious Attachment Detected (Phishing)

## Summary
**Classification:** True Positive
**Severity:** High
**Verdict:** Malicious — RemcosRAT delivered via spear phishing attachment, endpoint contained

A phishing email carrying a malicious attachment was delivered to an internal user. The attachment was confirmed as RemcosRAT malware through multi-source threat intelligence correlation.

## Alert Details

| Field | Value |
|---|---|
| Event ID | 45 |
| Rule | SOC114 - Malicious Attachment Detected - Phishing Alert |
| MITRE ATT&CK | [T1566.001 - Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001/) |
| Source | accounting[@]cmail[.]carleton[.]ca (49[.]234[.]43[.]39) |
| Destination | richard[@]letsdefend[.]io (172[.]16[.]17[.]45) |
| Subject | Invoice |
| Event Time | 15:48, 31.01.2021 |

![Alert Detail](screenshots/alert-soc114.png)
![Email Content](screenshots/email-soc114.png)

## Investigation

**Email Analysis:**
The email masqueraded as an invoice notification from an "accounting" sender at a spoofed-looking domain (`cmail.carleton.ca`), a common social-engineering pretext used to get a user to open a password-protected archive without suspicion. The attachment was password-protected (`infected`) — itself a red flag, since password-protecting an attachment is a known technique to evade automated email scanning/sandboxing while still relying on the human recipient to unlock it manually.

**File Analysis:**
- Attachment MD5: `c9ad9506bcccfaa987ff9fc11b91698d`
- VirusTotal: detection rate **35/61**, community score **-9**
- Malware Bazaar: identified as **RemcosRAT**, a remote access trojan primarily targeting Windows systems, commonly used for credential theft, keylogging, and remote system control

**Sender Reputation:**
- AbuseIPDB: source IP reported **2,367 times**
- Talos Intelligence: poor reputation rating, geolocated to China

The convergence of three independent sources (VirusTotal detection rate, Malware Bazaar family identification, and sender IP reputation) on the same malicious verdict removes reasonable doubt about classification.

## Verdict
Classified as **True Positive** — confirmed Spearphishing Attachment (T1566.001) delivering RemcosRAT.

## Recommendations
1. Contain the internal endpoint (richard[@]letsdefend[.]io's workstation) immediately — RemcosRAT provides remote access, so assume compromise until endpoint forensics confirm otherwise
2. Add sender IP 49[.]234[.]43[.]39 to blacklist
3. Search mail logs for other recipients of the same sender/subject pattern, in case this was a wider campaign rather than a single targeted email
4. Alert closed as malicious

## Lessons / Notes
Password-protected attachments bypassing automated scanning is a technique worth flagging in future triage — it should raise suspicion on its own, independent of any other indicator, since legitimate business invoices are rarely encrypted this way.
