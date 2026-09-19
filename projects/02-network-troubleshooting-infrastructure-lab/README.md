# 🔧 Network Troubleshooting & Infrastructure Lab

A hands-on infrastructure troubleshooting lab built using Windows 11, VirtualBox, and Ubuntu Linux.

The goal of this project was to practice troubleshooting common network and Linux infrastructure issues by identifying symptoms, collecting evidence, finding the root cause, applying a controlled fix, and verifying the result.

---

## 🎯 Objectives

- Understand Windows ↔ Linux network connectivity
- Practice troubleshooting IP connectivity and DNS
- Understand TCP port connectivity
- Troubleshoot SSH connectivity
- Investigate Linux service states and logs
- Understand the difference between service failures and firewall blocks
- Follow a structured troubleshooting methodology
- Document technical findings and evidence

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Host OS | Windows 11 |
| Guest OS | Ubuntu 26.04.1 LTS |
| Virtualization | VirtualBox |
| Ubuntu Hostname | `blue-notebook` |
| Ubuntu NAT IP | `10.0.2.15` |
| Ubuntu Host-Only IP | `192.168.96.101` |
| Windows Host-Only IP | `192.168.96.1` |

### Tools Used

- Windows PowerShell
- Windows Firewall
- VirtualBox
- Ubuntu Linux
- `ip`
- `ping`
- `ss`
- `systemctl`
- `journalctl`
- UFW
- SSH
- `Test-NetConnection`

---

## 🔍 Troubleshooting Methodology

For each case, I followed:

```text
Identify the symptom
        ↓
Check configuration
        ↓
Test connectivity / service
        ↓
Collect evidence
        ↓
Identify root cause
        ↓
Apply controlled fix
        ↓
Verify the result
        ↓
Document the findings
```

---

# 🛠️ Investigation Cases

## Case 01 — Host-Only Network Connectivity

### Problem

The Ubuntu VM was initially unable to ping the Windows Host-Only adapter.

### Investigation

I checked:

- IP configuration
- Routing information
- Windows → Ubuntu connectivity
- Ubuntu → Windows connectivity
- Windows inbound ICMP firewall rules

The Windows **File and Printer Sharing (Echo Request - ICMPv4-In)** rule was disabled.

### Resolution

Enabled the required Windows Firewall ICMPv4 Echo Request rule.

### Verification

Ubuntu successfully reached:

```text
192.168.96.1
```

with:

```text
4 packets transmitted
4 received
0% packet loss
```

### Key Learning

A correctly configured network can still fail connectivity testing when host-side firewall rules block the traffic.

---

## Case 02 — DNS Troubleshooting

### Problem

The Ubuntu VM could reach the Internet by IP address, but domain-name resolution was failing.

### Investigation

I compared:

```text
Gateway connectivity
Internet connectivity by IP
DNS name resolution
```

The DNS server configured on `enp0s3` had been deliberately changed to:

```text
192.0.2.1
```

DNS queries subsequently timed out.

### Resolution

Restored the DNS configuration to:

```text
192.168.0.1
```

### Verification

`resolvectl query google.com` successfully returned DNS information after the correction.

### Key Learning

Testing an IP address separately from a hostname helps determine whether a connectivity problem is actually a DNS problem.

---

## Case 03 — SSH Connectivity Troubleshooting

### Problem

SSH connectivity was deliberately interrupted in the Ubuntu VM.

### Investigation

Stopping `ssh.service` alone did not prevent a new SSH connection because `ssh.socket` was still active.

I then checked the actual listening ports using:

```bash
sudo ss -lntp | grep ':22'
```

After stopping both the SSH socket and service, port 22 was no longer listening.

A connection attempt from Windows produced:

```text
ssh: connect to host 192.168.96.101 port 22: Connection refused
```

### Resolution

Restored the SSH socket/service.

### Verification

SSH connectivity from the Windows host was successfully restored.

### Key Learning

Service state and listening socket state are not always identical. Checking the actual listening port and testing from the client provides better evidence during troubleshooting.

---

## Case 04 — Firewall & TCP Port Connectivity

### Problem

The Ubuntu host remained reachable, but TCP port 22 became inaccessible.

### Investigation

The initial TCP test showed:

```text
TcpTestSucceeded : True
```

UFW was then enabled with:

```text
Default: deny (incoming)
```

The explicit SSH allow rule was removed.

A new test from Windows showed:

```text
PingSucceeded    : True
TcpTestSucceeded : False
```

### Resolution

Restored the UFW rule allowing:

```text
22/tcp
```

### Verification

TCP connectivity to port 22 was restored.

### Key Learning

A successful ping does not mean that every TCP service on the host is reachable. ICMP connectivity and TCP port accessibility should be tested separately.

---

## Case 05 — Linux Service Troubleshooting

### Problem

The SSH service was deliberately stopped to simulate a service failure.

### Investigation

I checked the service state using:

```bash
systemctl status ssh
```

The service reported:

```text
Active: inactive (dead)
```

I then examined the service journal:

```bash
sudo journalctl -u ssh.service -n 20 --no-pager
```

The logs showed the service receiving signal 15 and being cleanly stopped.

### Resolution

Restarted the SSH service:

```bash
sudo systemctl start ssh.service
```

### Verification

`systemctl status ssh` showed:

```text
Active: active (running)
```

and the SSH server was listening on port 22.

A final Windows-side TCP test returned:

```text
TcpTestSucceeded : True
```

### Key Learning

Checking both service status and system logs provides useful evidence before applying a recovery action.

---

# 📚 Skills Practiced

### Networking

- IPv4 addressing
- Host-Only networking
- NAT
- Routing
- ICMP
- DNS
- TCP ports
- Connectivity testing

### Windows

- PowerShell
- Windows Firewall
- ICMP troubleshooting
- `Test-NetConnection`

### Linux

- Ubuntu
- `ip`
- `ping`
- `ss`
- `systemctl`
- `journalctl`
- UFW
- SSH

### Infrastructure Troubleshooting

- Symptom identification
- Configuration checking
- Evidence collection
- Root-cause investigation
- Controlled remediation
- Service troubleshooting
- Client-side verification
- Technical documentation

---

## 💡 Key Takeaways

This lab helped me understand that infrastructure troubleshooting should not rely on a single test.

For example:

```text
Ping works
    ≠
TCP service works

Service stopped
    ≠
Socket necessarily unavailable

Internet by IP works
    ≠
DNS resolution works
```

The main lesson from this project was to troubleshoot systematically, collect evidence at each stage, make one controlled change at a time, and verify the result from the appropriate side.

---

## 📸 Evidence

The lab evidence is currently documented through the investigation notes above. Screenshot files are not yet organized in this repository.

The evidence numbering below is the chronological order used during the lab and can be used later when the screenshots are added:

```text
01–08   Case 01 — Host-Only Connectivity
09–15   Case 02 — DNS Troubleshooting
16–22   Case 03 — SSH Connectivity
23–28   Case 04 — Firewall / Port Connectivity
29–33   Case 05 — Linux Service Troubleshooting
```

---

## ⚠️ Lab Safety

All troubleshooting activities were performed in my own controlled virtual lab environment.

No unauthorized systems or networks were targeted.

---

## 👤 Author

**Mayur Tayade**

Cybersecurity learner | IT Infrastructure & SOC path

GitHub: https://github.com/thebluenotebookcyber
