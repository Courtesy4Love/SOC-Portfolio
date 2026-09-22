# SOC Portfolio

Hands-on SOC alert triage and incident investigation write-ups, documenting my analysis process, tooling, and decision-making as I train toward a SOC Analyst role.

Each investigation follows a consistent format: alert context, evidence gathering, verdict, and recommended response — the same structure I'd use triaging a real alert queue.

---

## 🔬 Featured Investigation

**[SOC170 — Possible LFI Attack](investigations/soc170-lfi-attack/)**
Path traversal attempt against a public-facing web server. Confirms the attempt failed via two independent checks (server response + endpoint forensics) rather than trusting a single signal, and flags a WAF gap that let the request through in the first place.

## Other Investigations

| Alert | Type | Verdict | Notes |
|---|---|---|---|
| [SOC169 — IDOR Attack](Triage%20Investigation/SOC169%20IDOR%20Attack/) | Web Attack | True Positive — escalated to L2 | Sequential ID enumeration exposing other users' data |
| [SOC114 — Malicious Attachment](Triage%20Investigation/SOC114%20Phsihing/) | Phishing | True Positive | RemcosRAT confirmed across three independent sources |
| [SOC120 — Internal Phishing Alert](Triage%20Investigation/SOC120%20Phishing%20(Internal%20to%20Internal)/) | Phishing | False Positive | Rule-logic false positive, closed with no action |

## Tools Used Across Investigations

![VirusTotal](https://img.shields.io/badge/-VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/-AbuseIPDB-FF6B00?style=flat-square)
![Talos Intelligence](https://img.shields.io/badge/-Talos%20Intelligence-00BCEB?style=flat-square)
![Malware Bazaar](https://img.shields.io/badge/-Malware%20Bazaar-C1121F?style=flat-square)
![MITRE ATT&CK](https://img.shields.io/badge/-MITRE%20ATT%26CK-CC0000?style=flat-square)

## About This Portfolio

Alerts are sourced from [LetsDefend](https://letsdefend.io)'s SOC Analyst training environment. Write-ups are my own analysis and are not copies of any official walkthrough or solution key.

More investigations are added as I progress through training.
