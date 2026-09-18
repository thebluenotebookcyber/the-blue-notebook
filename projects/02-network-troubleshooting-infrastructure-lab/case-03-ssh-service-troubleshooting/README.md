# Case 03 — SSH Service Troubleshooting

## Objective
Troubleshoot an SSH connectivity failure caused by the SSH service/socket state.

## Scenario
SSH connectivity from Windows to Ubuntu was working initially. The SSH service/socket was then stopped in a controlled lab test, causing connection attempts to fail.

## Investigation
1. Confirmed the SSH baseline.
2. Checked the SSH service state.
3. Checked TCP port 22 with `ss`.
4. Stopped the SSH service/socket.
5. Verified that port 22 was no longer available.
6. Tested SSH from Windows and observed connection refusal.
7. Restored the SSH socket/service.
8. Verified that TCP port 22 was listening again.

## Resolution
Restored the SSH socket/service so that the SSH daemon could accept connections.

## Verification
The SSH connection from Windows to Ubuntu succeeded after the service/socket was restored.

## Evidence
Evidence screenshots will be added to this section.

- 16 — SSH baseline success
- 17 — SSH service stopped
- 18 — SSH port listening
- 19 — SSH port not listening
- 20 — SSH connection refused
- 21 — SSH port restored
- 22 — SSH connection restored

## Key Learning
A connection refusal can indicate that the destination service is not listening. Checking both service state and listening ports helps separate service problems from network problems.
