# Syslog Triage CLI

A high-throughput, constant-memory command-line utility for automated server log parsing, severity filtering, and incident reporting.

## Problem Statement
In production environments, daily service logs regularly scale beyond 10–15 GB. Desktop tools (Excel, standard text editors) fail due to hard row limits (Excel's 1,048,576 row ceiling) or cause Out-Of-Memory (OOM) crashes. This utility provides an automated, zero-dependency streaming solution for rapid L1/L2 incident triage.

## Architecture & Engineering Decisions
* **Constant Memory Iteration ($O(1)$ Space):** Utilizes generator stream reads (`for line in infile`) instead of buffered loading (`.readlines()`), maintaining stable memory utilization (<15 MB RAM) regardless of file size.
* **POSIX Exit Codes for Automation:** Returns `0` on successful completion and `1` on missing paths or access violations, enabling plug-and-play scheduling in Linux `cron` and CI/CD pipelines.
* **Defensive Parameter Validation:** Enforces argument length checks before disk I/O to prevent unhandled `IndexError` exceptions.
* **Non-Destructive Audit Trail:** Appends triage results (`mode='a'`) to preserve run history across successive automated sweeps.

## Usage
bash

Execute triage against a target log:
python3 src/triage.py /var/log/syslog

View generated incident summary:
cat incident_report.txt
