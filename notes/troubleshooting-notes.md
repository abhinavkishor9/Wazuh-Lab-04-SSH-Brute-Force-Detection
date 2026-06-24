# Troubleshooting Notes – SSH Brute Force Detection Lab

## Issue 1

### SSH Not Running

Symptoms:

```text
Connection refused
```

Verification:

```bash
sudo systemctl status sshd
```

Resolution:

```bash
sudo systemctl start sshd
sudo systemctl enable sshd
```

---

## Issue 2

### SSH Port Not Listening

Verification:

```bash
sudo ss -tulpn | grep :22
```

Expected:

```text
LISTEN on port 22
```

Resolution:

Restart SSH service.

```bash
sudo systemctl restart sshd
```

---

## Issue 3

### Firewall Blocking SSH

Verification:

```bash
sudo firewall-cmd --list-all
```

Resolution:

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

---

## Issue 4

### No Authentication Logs

Verification:

```bash
sudo tail -f /var/log/secure
```

Resolution:

Confirm SSH service is receiving login attempts.

---

## Issue 5

### No Wazuh Alerts Generated

Verification:

```bash
sudo tail -f /var/ossec/logs/ossec.log
```

Check:

```text
Log collection status
```

Verify:

```bash
sudo systemctl status wazuh-manager
```

---

## Issue 6

### No Events in Threat Hunting

Actions:

1. Increase time range to Last 24 Hours
2. Remove all filters
3. Refresh Threat Hunting page

Search:

```text
rule.id:5710
```

```text
rule.id:5758
```

```text
rule.id:2502
```

---

## Issue 7

### Agent Filter Hiding Results

Symptoms:

```text
No results found
```

Resolution:

Remove:

```text
agent.id:001
```

filter from Threat Hunting.

---

## Key Commands Used

```bash
sudo systemctl status sshd
```

```bash
sudo ss -tulpn | grep :22
```

```bash
sudo tail -f /var/log/secure
```

```bash
sudo systemctl status wazuh-manager
```

---

## Final Verification

Successful detections:

```text
Rule 5710
Rule 5758
Rule 2502
```

confirmed that brute-force activity was successfully detected and investigated.
