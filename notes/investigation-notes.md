# Investigation Notes – SSH Brute Force Detection

## Investigation Summary

Multiple failed SSH authentication attempts were observed against the Linux server.

The source system repeatedly attempted to authenticate using a non-existent account.

Wazuh successfully generated alerts corresponding to the authentication failures.

---

## Timeline

### SSH Service Verification

```bash
sudo systemctl status sshd
```

Result:

```text
SSH service running
```

---

### Port Verification

```bash
sudo ss -tulpn | grep :22
```

Result:

```text
SSH listening on port 22
```

---

### Attack Execution

Attacker:

```text
Windows 11
```

Tool:

```text
PuTTY
```

Username:

```text
fakeuser
```

Target:

```text
192.168.203.133
```

---

### Linux Authentication Logs

Observed:

```text
Failed password for invalid user fakeuser
```

Observed:

```text
Maximum authentication attempts exceeded
```

Observed:

```text
Disconnected invalid user fakeuser
```

---

## Wazuh Detections

### Rule 5710

```text
Attempt to login using a non-existent user
```

Level:

```text
5
```

---

### Rule 5758

```text
Maximum authentication attempts exceeded
```

Level:

```text
8
```

---

### Rule 2502

```text
User missed the password more than one time
```

Level:

```text
10
```

---

## Source Information

Source IP:

```text
192.168.203.1
```

Target:

```text
192.168.203.133
```

Protocol:

```text
SSH
```

Port:

```text
22
```

---

## Analyst Assessment

The activity represents an SSH brute-force attempt against a Linux system.

Evidence includes:

- Repeated failed authentication attempts
- Invalid user usage
- Authentication threshold exceeded
- Multiple correlated Wazuh detections

---

## Conclusion

Wazuh successfully detected and correlated brute-force activity through SSH authentication logs.

The attack was unsuccessful because no valid credentials were provided.
