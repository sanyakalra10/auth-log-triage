# Auth Log Triage Tool

A Jupyter notebook that reads SSH authentication logs, detects suspicious login patterns with three simple rules,
adds IP reputation data, assigns a severity score, and writes an incident report with evidence, a MITRE ATT&CK
mapping and a recommended action.

Built for the **IBM SkillsBuild Cybersecurity Bootcamp 2026 (BharatCares)**.
Author: Sanya Kalra, B.Tech Computer Engineering (Minor: Cybersecurity), Shri Vishwakarma Skill University.

## Project description
SOC analysts spend much of their time reading authentication logs and deciding which events matter. This project
automates the repetitive first steps of L1 triage: **parse, detect, enrich, score, report**. Every rule and score is
simple and explainable (no black-box model), and an alert is treated as a lead for an analyst, not as proof of an attack.

## Features
- Parses OpenSSH-style log lines into a pandas table (timestamp, event type, username, source IP)
- Three detection rules (thresholds are variables in one cell and easy to tune):
  1. `rule_burst`: 10 or more failed logins from one IP within a 5-minute window
  2. `rule_success_after_failures`: a successful login within 10 minutes after 5 or more failures from the same IP
  3. `rule_many_usernames`: 5 or more different usernames tried from one IP
- IP reputation enrichment from a local mock file (runs offline)
- Point-based severity (Low / Medium / High) with a verdict and recommended action
- MITRE ATT&CK mapping (T1110 Brute Force, and T1078 Valid Accounts when a login succeeded)
- Lists IPs that had failed logins but triggered no rule, and explains why (false-positive handling)
- Incident report saved to `output/incident_report.md` and `output/incident_report.csv`
- Self-check cell with assertions that tests the tool against the known scenarios in the dataset

## Dataset
- Log file: [`data/sample_auth.log`](data/sample_auth.log)
- Reputation file: [`data/mock_reputation.json`](data/mock_reputation.json)

Both files are **synthetic**, created for this project. The log has 178 lines (128 tracked events and 50 session lines
that are skipped) and four scenarios: brute force with no success, brute force followed by a successful root login,
username guessing, and a harmless mistyped password. IP addresses come from reserved documentation ranges (RFC 5737),
and the reputation scores are made up for practice. No real system, person, or threat intelligence is involved.

## Technologies used
Python 3.10 or newer, pandas, regular expressions (`re`), Jupyter Notebook / Google Colab.

The notebook code was developed with **IBM Bob** and tested by running it in Google Colab (all self-check assertions pass).
The synthetic dataset and the prompts used were prepared with AI assistance (Claude by Anthropic).

## Setup and run
1. Clone the repository: `git clone https://github.com/<your-username>/auth-log-triage.git`
2. Install dependencies: `pip install -r requirements.txt`
3. Start Jupyter: `jupyter notebook`
4. Open `SanyaKalra_AuthLogTriage.ipynb` (keep the `data/` folder next to it)
5. Choose **Run All**. The report is saved in the `output/` folder.

To run in Google Colab: upload the notebook, create a `data` folder in the Files panel, upload the two files from
`data/`, then choose **Runtime > Run all**.

## Notebook structure
| Section | What it does |
|---|---|
| 1. Load and parse | Regex parsing into a DataFrame, counts skipped lines |
| 2. Detection rules | The three rules and their thresholds |
| 3. Enrichment and scoring | Mock reputation, severity points, verdict, MITRE mapping, IPs with no alert |
| 4. Report and self-check | Writes the Markdown and CSV report, runs assertions |

## Sample result
| IP | Severity | Why |
|---|---|---|
| 198.51.100.77 | High (8 pts) | 14 failed logins on `root`, then a successful login: possible compromise, escalate to L2 |
| 203.0.113.45 | Medium (4 pts) | 40 failed logins on `admin` in about 3 minutes, no success: suspicious, not confirmed |
| 192.0.2.10 | Medium (3 pts) | 9 different usernames tried: guessing or scanning |

`10.0.0.15` (3 failures, then a normal login) raises no alert because it looks like a mistyped password.
See [`output/incident_report.md`](output/incident_report.md) for the full report.

## Limitations
- Works on a small synthetic log, not on production data
- Reputation scores are mocked; there is no live threat-intelligence lookup
- Only OpenSSH password events are parsed
- Thresholds are fixed numbers and would need tuning on real traffic

## Future work
- Support Windows Event IDs 4624 / 4625
- Add a live reputation lookup (API key kept outside the code)
- Test on a public log dataset
- Store incidents in a database and add a simple chart or dashboard view

## Contact
Sanya Kalra | LinkedIn: https://www.linkedin.com/in/sanya-kalra
