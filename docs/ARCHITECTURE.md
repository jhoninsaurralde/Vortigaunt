# Vortigaunt Architecture

## Overview

Vortigaunt is a lightweight persistent monitoring system designed for small Linux home servers.

The goal is not to replace enterprise monitoring platforms, but to provide a simple, transparent and self-hosted layer that gives a machine:

- Visibility
- Historical records
- Persistent memory
- Automated maintenance tasks
- Recovery information

Vortigaunt is designed to run on low-resource hardware while maintaining reliability.

---

# Architecture Layers

## 1. Core System

The base layer consists of:

- Linux operating system
- Bash scripts
- systemd services and timers
- mdadm RAID monitoring

Vortigaunt relies on native Linux tools instead of external databases or heavy frameworks.

---

# 2. Monitoring Layer

The monitoring layer periodically collects system information.

Tracked information:

- CPU temperature
- Memory usage
- Storage availability
- RAID status
- System uptime
- Boot events

Data is stored as human-readable logs.

Example:


2026-07-21 03:43:41 | CPU:+40.0°C | RAM:5.9% | RAID:OK


---

# 3. Persistent Memory System

Vortigaunt maintains a simple memory structure.


memory/

├── events.log
├── experiences.log
├── milestones.log
└── system/
├── current/
└── history/


## Events

Short-term operational history.

Examples:

- Boot events
- Monitoring results
- Backup execution

---

## Experiences

Long-term operational records.

Examples:

- Successful RAID creation
- Backup completion
- System recovery after reboot

---

## Milestones

Important development or system events.

Examples:

- First persistent storage created
- First successful backup
- First stable deployment

---

# 4. Storage Layer

Vortigaunt was designed around redundant storage.

Current reference deployment:


RAID1 md0

/dev/sdb
/dev/sdc


Purpose:

- Protect against single disk failure
- Maintain availability
- Preserve system memory

---

# 5. Backup Layer

Backups store:

- Database files
- Memory logs
- System history

Structure:


backups/

├── archive/
│ └── vortigaunt-date/
│
└── latest/


Backups can be executed manually or automatically.

---

# 6. Automation Layer

Vortigaunt uses systemd timers.

Current timers:


vortigaunt-monitor.timer

vortigaunt-report.timer

vortigaunt-backup.timer


This allows the system to operate independently after startup.

---

# Design Philosophy

Vortigaunt follows three principles:

## Simple

Use existing Linux components.

No unnecessary dependencies.

## Transparent

All data is stored in readable files.

The administrator can inspect everything manually.

## Persistent

A machine should remember:

- What happened
- What changed
- What was repaired
- What milestones were reached

---

# Future Development

Possible improvements:

- Web dashboard
- Notification system
- Hardware inventory database
- Network monitoring
- Remote nodes
- Encrypted backups


