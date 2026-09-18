# Case 02 — DNS Troubleshooting

## Objective
Diagnose a controlled DNS failure and distinguish name-resolution problems from general network connectivity problems.

## Scenario
Internet connectivity by IP continued to work, while hostname resolution failed after a controlled DNS misconfiguration.

## Investigation
1. Established a DNS baseline.
2. Verified connectivity to the NAT gateway.
3. Verified connectivity to a public IP address.
4. Confirmed that hostname resolution initially worked.
5. Inspected DNS configuration with `resolvectl status`.
6. Introduced a controlled invalid DNS server configuration.
7. Tested hostname resolution and direct IP connectivity separately.
8. Queried DNS directly with `resolvectl query`.

## Resolution
Restored the working DNS server configuration on the Ubuntu NAT interface.

## Verification
Hostname resolution succeeded again after the DNS configuration was restored.

## Evidence

### 09 — DNS Baseline
![DNS Baseline](09-dns-baseline.png)

### 10 — Internet IP Baseline
![Internet IP Baseline](10-internet-ip-baseline.png)

### 11 — DNS Misconfiguration
![DNS Misconfiguration](11-dns-misconfiguration.png)

### 12 — DNS Failure — IP Still Works
![DNS Failure — IP Still Works](12-dns-failure-ip-still-works.png)

### 13 — DNS Query Timeout
![DNS Query Timeout](13-dns-query-timeout.png)

### 14 — DNS Restored
![DNS Restored](14-dns-restored.png)

### 15 — DNS Fix Verified
![DNS Fix Verified](15-dns-fix-verified.png)

## Key Learning
When an IP address works but a hostname does not, DNS should be investigated separately from basic network connectivity.
