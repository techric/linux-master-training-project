# Module 06: Advanced Logging, Tracing, and Event Correlation  
(The Other 5%)

## Overview

This module teaches advanced techniques for inspecting, correlating, and tracing logs across subsystems. These skills become crucial when:

- Issues span multiple services  
- Logs appear normal but the system is malfunctioning  
- Kernel, network, and application events interact  
- Timing, sequencing, and concurrency problems arise  
- You need to trace a problem backwards from its symptoms  

This is deep-dive work involving system internals, structured logging, kernel messages, journald indexing, and real-time log tracing.

---

## Learning Objectives

By the end of this module, you should be able to:

- Query journald with advanced filters  
- Extract logs with precise time windows  
- Correlate service, kernel, and application logs  
- Trace events with `journalctl -f`, `-o json`, and `--since`  
- Interpret kernel logs for I/O, networking, and memory issues  
- Use `systemd-analyze` for startup and boot diagnostics  
- Create temporary structured log filters for investigation  
- Diagnose silent failures, restarts, and systemd dependency loops  

---

## Lesson 1: Advanced journalctl Techniques

### Time-Based Filtering

Show logs since 10 minutes ago:
    journalctl --since "10 minutes ago"

Between two timestamps:
    journalctl --since "2025-01-01 10:00" --until "2025-01-01 10:30"

Show logs for a specific systemd unit:
    journalctl -u nginx.service

Show logs for a unit during last boot:
    journalctl -u nginx.service -b

Follow logs in real time:
    journalctl -fu nginx.service

### Format as JSON (for deeper analysis):
    journalctl -u nginx.service -o json-pretty

This is helpful for correlation with external SIEM tools.

---

## Lesson 2: Kernel Logging (dmesg Deep Dive)

`dmesg` provides real-time kernel messages that often reveal issues hidden from application logs.

View all kernel messages:
    dmesg

Filter for common issue categories:

I/O storage:
    dmesg | grep -i "blk"

Memory:
    dmesg | grep -i oom

Networking:
    dmesg | grep -i eth
    dmesg | grep -i net

Hardware problems:
    dmesg | grep -i error

Recently added messages only:
    dmesg --follow

Kernel logs often reveal problems such as:

- Disk read/write errors  
- NIC resets  
- OOM killer events  
- Kernel module failures  
- PCI bus issues  

---

## Lesson 3: Application Logging and Structured Logs

Many production systems (Nginx, Postgres, Docker, Kubernetes) now produce structured logs.

View Docker logs:
    docker logs container-name

Follow in real time:
    docker logs -f container-name

View Kubernetes pod logs:
    kubectl logs podname
    kubectl logs podname -f

Structured JSON logs:
    jq '.' /var/log/app.log

Use `jq` filters:
    jq 'select(.level == "error")' /var/log/app.json

This is essential for modern cloud-native debugging.

---

## Lesson 4: Service Restart and Failure Diagnostics (systemd)

Check why a service failed:
    systemctl status nginx.service

Show last 50 log lines:
    journalctl -u nginx.service -n 50

Show exit code:
    systemctl show nginx.service -p ExecMainStatus

Show restart count:
    systemctl show nginx.service -p NRestarts

Detect restart loops:
    systemctl status nginx.service | grep -i "restarted"

Check dependency failures:
    systemctl list-dependencies nginx.service --failed

Failure analysis often involves layering:

- Unit logs  
- Kernel logs  
- Dependency logs  
- Boot logs  

---

## Lesson 5: Boot Problems, Startup Timing, and systemd-analyze

General boot timeline:
    systemd-analyze

Detailed breakdown:
    systemd-analyze blame

Critical path (graph):
    systemd-analyze critical-chain

Plot SVG boot chart (if supported):
    systemd-analyze plot > boot.svg

This helps when:

- Systems boot slowly  
- Certain services delay others  
- Storage or network initialization lags  
- systemd ordering issues appear  

---

## Lesson 6: Tracing Events Across Subsystems

The real power of advanced logging is correlation.

### Example: Application slow due to storage bottleneck

1. Check application logs:
       journalctl -u app.service

2. Check I/O wait in kernel logs:
       dmesg | grep -i blk

3. Check disk utilization:
       iostat -xz 1

If timestamps match, you’ve found the root cause.

---

### Example: Network outage affecting a service

1. Application timeout logs:
       journalctl -u myapp.service | grep timeout

2. Kernel logs for NIC:
       dmesg | grep -i eth

3. Network manager or systemd-networkd logs:
       journalctl -u systemd-networkd

If NIC resets appear, the issue is hardware, not application-level.

---

### Example: OOM Killing root cause

1. OOM event:
       dmesg | grep -i oom

2. Identify killed process:
       journalctl -k | grep -i "killed process"

3. Check memory usage at that time (if logs captured):
       journalctl --since "..." --until "..."

Memory exhaustion events often appear only in dmesg.

---

## Lesson 7: Advanced Log Extraction and Exporting

Export system logs for external analysis:
    journalctl > all_logs.txt

Export only kernel logs:
    journalctl -k > kernel_logs.txt

Export unit-specific logs:
    journalctl -u nginx.service > nginx_logs.txt

Export with structured formatting:
    journalctl -u nginx.service -o json > nginx.json

These files can be uploaded into:

- Splunk  
- ELK stack  
- Datadog  
- Security tools  
- Offline forensic analysis environments  

---

## Lesson 8: Real-World Scenarios

### Scenario 1: Service working intermittently

- Check for restarts (systemctl status)  
- Check kernel logs (I/O failures)  
- Check network logs (NIC resets)  
- Check application logs (timeouts)  

### Scenario 2: Silent failure—service exits without logs

- Journalctl in JSON mode  
- systemd ExecMainStatus  
- dmesg for hardware-related termination  

### Scenario 3: Server slow with no obvious errors

- disk latency in dmesg  
- OOM events  
- dependency loops during boot  
- restart loops in systemd  

---

## Lab Exercise

1. Query logs for a service over the last 5 minutes:
       journalctl -u yourservice --since "5 minutes ago"

2. Extract only kernel networking messages:
       dmesg | grep -i net

3. Identify the slowest boot services:
       systemd-analyze blame

4. Export all JSON logs for analysis:
       journalctl -o json > system.json

5. Filter logs for errors only (using jq):
       journalctl -o json | jq 'select(.PRIORITY == "3")'

Document your output and interpretations.

---

## Quiz

1. What does `journalctl -u service -b` show?  
2. Why is dmesg critical for diagnosing storage or memory failures?  
3. What does `systemd-analyze blame` help visualize?  
4. Why use `journalctl -o json` in complex investigations?  
5. How do you detect service restart loops?  

---

## Notes and Observations

Use this space to record:

- Log correlations you found  
- Kernel events related to service failures  
- Timing issues during service startup  
- Any insights from structured logs (JSON)  
- Difficult failures that required multi-log analysis  
