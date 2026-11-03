# Incident Report — Auth Log Triage

**Generated:** 2026-10-18  
**Log file:** `data/sample_auth.log`  
**Reputation feed:** `data/mock_reputation.json`  

---

## Alerts

### 198.51.100.77 — High (8 pts)

| Field | Value |
|---|---|
| **Rules triggered** | rule_burst, rule_success_after_failures |
| **Evidence** | burst: 14 failures in 5-min window (14:30:00–14:31:49) | success_after_failures: 14 failures then login as 'root' at 14:32:20 |
| **abuse_confidence** | 78 |
| **reports** | 140 |
| **MITRE ATT&CK** | T1110 Brute Force, T1078 Valid Accounts |
| **Verdict** | SUCCESS AFTER FAILURES — possible credential compromise. Block IP immediately, revoke session, escalate to L2 for forensic review. |

### 192.0.2.10 — Medium (3 pts)

| Field | Value |
|---|---|
| **Rules triggered** | rule_many_usernames |
| **Evidence** | many_usernames: 9 distinct usernames probed (ftpuser, git, guest, oracle, pi, postgres, test, ubuntu, user) |
| **abuse_confidence** | 35 |
| **reports** | 22 |
| **MITRE ATT&CK** | T1110 Brute Force |
| **Verdict** | USERNAME ENUMERATION — credential-stuffing/scan pattern. Monitor closely; consider geo-block or rate-limit on this IP. |

### 203.0.113.45 — Medium (4 pts)

| Field | Value |
|---|---|
| **Rules triggered** | rule_burst |
| **Evidence** | burst: 40 failures in 5-min window (02:14:00–02:16:57) |
| **abuse_confidence** | 92 |
| **reports** | 310 |
| **MITRE ATT&CK** | T1110 Brute Force |
| **Verdict** | BURST WITHOUT SUCCESS — brute-force attempt, no confirmed compromise. Block IP, increase fail2ban sensitivity, continue monitoring. |

---

## IPs with failures — no rule triggered

- **10.0.0.15** — 3 failure(s), username(s): priya. Pattern consistent with mistyped password (internal address, same username, < 5 failures, followed by successful login). No escalation required.
- **10.0.0.22** — 1 failure(s), username(s): rahul. Pattern consistent with mistyped password (internal address, same username, < 5 failures, followed by successful login). No escalation required.
- **10.0.0.31** — 1 failure(s), username(s): meera. Pattern consistent with mistyped password (internal address, same username, < 5 failures, followed by successful login). No escalation required.
- **10.0.0.5** — 1 failure(s), username(s): dev_admin. Pattern consistent with mistyped password (internal address, same username, < 5 failures, followed by successful login). No escalation required.

---

*End of report.*
