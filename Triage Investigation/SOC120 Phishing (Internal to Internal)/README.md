# SOC120 — Phishing Mail Detected (Internal to Internal)

## Summary
**Classification:** False Positive
**Severity:** Medium
**Verdict:** Benign — flagged solely by mail flow rule, no malicious indicators

An internal-to-internal email was flagged by a rule designed to catch phishing, but investigation found no malicious content, attachment, or indicator. The alert reflects a rule design limitation, not an actual threat.

## Alert Details

| Field | Value |
|---|---|
| Event ID | 52 |
| Rule | SOC120 - Phishing Mail Detected - Internal to Internal |
| Sender | john[@]letsdefend[.]io (172[.]16[.]20[.]3) |
| Recipient | susie[@]letsdefend[.]io |
| Subject | Meeting |
| Event Time | 04:24, 07.02.2021 |

![Alert Detail](screenshots/alert-soc120.png)
![Email Content](screenshots/email-soc120.png)

## Investigation
The email content was a plain-text request asking whether the recipient was available to meet — no links, no attachments, no urgency language, no impersonation indicators. Both sender and recipient are legitimate internal addresses on the same domain. The alert fired purely because the rule logic flags internal-to-internal mail flow as inherently suspicious, without evaluating content for actual phishing indicators.

## Verdict
Classified as **False Positive**. No anomalies found in sender, content, or structure.

## Recommendations
1. Close the alert — no containment or blacklisting required
2. Flag the underlying rule logic for tuning: internal-to-internal flow alone is too broad a trigger condition and will generate recurring noise without added content-based indicators (e.g., suspicious links, attachment presence, spoofed display names)

## Lessons / Notes
Not every alert closure requires a complex investigation — the discipline here is confirming absence of risk (no attachment, benign content, legitimate sender) rather than assuming benign just because the scenario looks simple.
