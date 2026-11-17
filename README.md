# File Integrity Monitor

A lightweight real-time file integrity monitor written in Python. It watches one or more directories for file creation, modification, and deletion, computes cryptographic hashes, and writes timestamped events to a log so unauthorized or unexpected changes are easy to spot.

Useful for security labs, SOC practice, and as a base for intrusion-detection tooling.

---

## Features

- Detects **file creation, modification, and deletion** in real time
- Generates **cryptographic hashes** (SHA-256 by default, configurable)
- Monitors one or more directories declared in `config.json`
- Logs every event with a **timestamp, path, and hash**
- Extensible for alerting (email, Slack, Discord) and reporting

## Project structure

- `monitor.py` — entry point; runs the watchdog observer and event handler
- `config.json` — folders to watch, hash algorithm, and log file path
- `logs.txt` — runtime log of detected events
- `hash_db.json` — local store of the latest hashes for monitored files

## Requirements

- Python 3.8+
- `watchdog`

## Setup

```bash
git clone https://github.com/muhammadsaud1/FileIntegrityMonitor.git
cd FileIntegrityMonitor
python3 -m venv venv
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate         # Windows
pip install watchdog
```

## Configuration

Edit `config.json`:

```json
{
    "folders_to_monitor": ["monitor_folder"],
    "hash_algorithm": "sha256",
    "log_file": "logs.txt"
}
```

## Run

Create the folder to monitor if it does not exist, then start the monitor:

```bash
mkdir -p monitor_folder
python3 monitor.py
```

## Try it out

With the monitor running, make changes in the watched folder:

```bash
echo "hello" > monitor_folder/test1.txt      # NEW
echo "update" >> monitor_folder/test1.txt    # MODIFIED
rm monitor_folder/test1.txt                  # DELETED
```

Each action appears in `logs.txt` with a timestamp and, where applicable, the SHA-256 hash.

## Roadmap

- Alerting integrations (email, Slack, Discord)
- PDF / HTML report exports
- Baseline mode (snapshot and compare)
- Multi-hash support (MD5, SHA-1, SHA-256 at once)

## Safety and ethics

This project is intended for educational and defensive use only. Running it against your own files is safe; take care when deploying monitoring tools on production systems. Do not use this code for malicious purposes.
