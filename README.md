# Wazuh-Lab-04-SSH-Brute-Force-Detection
## Overview

This lab demonstrates how Wazuh detects SSH brute-force activity against a Linux server.

Multiple failed SSH login attempts were generated from a Windows attacker machine using PuTTY against a Rocky Linux system running OpenSSH.

The resulting authentication failures were collected by Wazuh and analyzed through the Threat Hunting module.

---

## Lab Objectives

- Verify SSH service availability
- Generate failed SSH authentication attempts
- Validate Linux authentication logs
- Detect brute-force activity using Wazuh
- Investigate generated alerts

---

## Lab Environment

### Wazuh Manager

| Component | Value |
|------------|------------|
| Platform | Rocky Linux |
| IP Address | 192.168.203.133 |
| Wazuh Version | 4.x |

### Attacker System

| Component | Value |
|------------|------------|
| Platform | Windows 11 |
| Tool | PuTTY |

---

## Attack Simulation

A non-existent user account named:

```text
fakeuser
```

was used to perform multiple SSH login attempts against the Wazuh manager.

Example:

```text
fakeuser@192.168.203.133
```

Several incorrect passwords were entered to trigger brute-force related detections.

---

## Verification Steps

### Verify SSH Service

```bash
sudo systemctl status sshd
```

Expected:

```text
active (running)
```

---

### Verify SSH Port

```bash
sudo ss -tulpn | grep :22
```

Expected:

```text
LISTEN 0.0.0.0:22
```

---

### Monitor Authentication Logs

```bash
sudo tail -f /var/log/secure
```

Observed Events:

```text
Failed password for invalid user fakeuser
Maximum authentication attempts exceeded
Disconnected invalid user fakeuser
```

---

## Wazuh Threat Hunting

Navigate to:

```text
Threat Hunting → Events
```

Investigate SSH related detections.

---

## Detected Rules

### Rule 5710

```text
sshd: Attempt to login using a non-existent user
```

Severity:

```text
Level 5
```

---

### Rule 5758

```text
Maximum authentication attempts exceeded
```

Severity:

```text
Level 8
```

---

### Rule 2502

```text
User missed the password more than one time
```

Severity:

```text
Level 10
```

---

## MITRE ATT&CK Mapping

### T1110

```text
Brute Force
```

Credential access attempts through repeated authentication failures.

---

## Skills Practiced

- SSH Log Analysis
- Linux Authentication Monitoring
- Wazuh Threat Hunting
- Brute Force Detection
- Alert Validation
- SOC Investigation Workflow

---

## Outcome

Successfully generated and detected SSH brute-force activity.

Validated:

- Linux authentication logs
- SSH security events
- Wazuh detection rules
- Threat Hunting investigations
