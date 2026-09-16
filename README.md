# wazuh-siem-deployment
Wazuh SIEM Deployment — Security Monitoring Lab

A full Wazuh SIEM environment built from scratch in VMware (16GB RAM host), following official Wazuh documentation. Covers clustered infrastructure, custom detection engineering, threat intelligence integration, and automated security reporting, built and tested in an isolated lab, then applied to real alert tuning in a client environment.

Environment
  Node	          Role
- wazuh-master  | Indexer node-1, Manager (master role), Dashboard
- wazuh-worker	| Indexer node-2, Manager (worker role)

2-node indexer cluster, 2-node manager cluster — both verified through repeated status checks
Dashboard served over HTTPS on the master node

Agents: host-laptop (Windows 10) and win-server (Windows Server 2022 VM) — both Active
All nodes run on an isolated internal lab network, not exposed externally
What's built:

* Custom FIM (File Integrity Monitoring)

Real-time monitoring of a dedicated C:\FIM directory on host-laptop, scoped via a fim-monitor agent group. Custom rule chain (100022–100025) is silent (level 0) by default and escalates to level 10 only during an after-hours window (5 PM–8 AM), covering created/modified/deleted events, plus a correlation rule for renames. Validated end-to-end with an EICAR test file.

* Windows account & privilege monitoring

Three custom rules (100030–100032): user account creation, addition to the Administrators group, and account disablement. Rewritten to explicitly override the built-in Wazuh rules that were catching these events first.

* Automated weekly reporting (script-based)

Two scripts, both scheduled via cron for Saturday 8:00 AM, both confirmed to fire automatically:

automation/weekly_report.sh — all alerts at level 10+ from the past 7 days

Skills & tools

Wazuh, VMware, OpenSearch, VirusTotal API, Bash, Cron, XML (Wazuh ruleset), Windows Event Log Monitoring, FIM, SIEM Tuning, Rule Correlation, Report Automation
