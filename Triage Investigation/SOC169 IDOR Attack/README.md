# SOC169 — Possible IDOR Attack Detected

## Summary
**Classification:** True Positive
**Severity:** Medium
**Verdict:** Malicious — IDOR enumeration attempt, escalated to L2

An external source made repeated, parameter-incrementing requests against an internal web server, consistent with an Insecure Direct Object Reference (IDOR) enumeration attack attempting to access other users' data by manipulating a predictable ID.

## Alert Details

| Field | Value |
|---|---|
| Event ID | 119 |
| Rule | SOC169 - Possible IDOR Attack Detected |
| MITRE ATT&CK | [T1190 - Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/) |
| Source IP | 134[.]209[.]118[.]137 (United States) |
| Destination | WebServer1005 (172[.]16[.]17[.]15) |
| Requested URL | `/get_user_info/` |
| HTTP Method | POST |
| Event Time | 22:48, 28.02.2022 |
| Trigger Reason | Consecutive requests to the same page |

![Alert Detail](screenshots/alert-soc169.png)

## Investigation

**Threat Intelligence:**
- AbuseIPDB: source IP reported **1,537 times**, indicating an established history of abuse
- VirusTotal: community score of **-1**, a mild but present negative signal

**Log Analysis:**
The alert triggered on repeated requests to the same endpoint, which on its own is a weak signal. Deeper inspection of the request bodies showed the `user_id` parameter incrementing sequentially (1, 2, 3...) across the request sequence — this is the actual indicator of compromise, not the request repetition itself. Sequential ID enumeration is a textbook IDOR technique: the attacker isn't guessing randomly, they're systematically walking through a predictable identifier space to see what data the application returns for IDs that don't belong to them.

HTTP response status was 200 for the requests, and response size varied per request — meaning the server returned different data for different `user_id` values rather than uniformly rejecting the requests. This is the critical detail: a properly authorized endpoint should reject or return identical "not authorized" responses for IDs outside the requester's scope. Varying response sizes with 200 status codes suggest the endpoint returned real user data for each incremented ID, which is the actual compromise, not just the attempt.

## Verdict
Classified as **True Positive** — confirmed IDOR enumeration (T1190) against WebServer1005. The combination of sequential ID manipulation, consistent 200 responses, and variable response sizes indicates the endpoint likely leaked user data for multiple accounts, not just the attacker's own.

## Recommendations
1. Contain WebServer1005 pending confirmation of the scope of data exposed
2. Add source IP 134[.]209[.]118[.]137 to blacklist
3. **Escalate to SOC Analyst L2** — this requires a code-level fix (implement proper authorization checks on `/get_user_info/` so it validates the requester owns the requested `user_id`), which is outside standard SOC containment scope
4. Audit logs for the same source IP or similar enumeration patterns against other endpoints, in case this was reconnaissance for a broader IDOR sweep

## Lessons / Notes
Request-repetition rules like SOC169 catch this pattern, but the differentiator between "user is refreshing a page" and "user is enumerating an ID space" is the parameter values across requests, not just the request count. Future triage on similar alerts should check parameter progression before ruling on severity.
