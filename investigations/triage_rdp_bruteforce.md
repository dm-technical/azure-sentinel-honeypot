# Triage Playbook — RDP Brute Force (4625)

## Objective
Confirm whether repeated failed logons represent internet spray/bruteforce and determine if any success occurred.

## Quick Checks
1. Identify top IPs and usernames.
2. Check if failures transitioned into a success (4624) from same IP.
3. Determine time range and rate (bursty vs steady).

## Queries
- Top sources/users:
  - Use `detections/rdp_bruteforce_4625.kql`
- Any success from same IP:
  - Filter `SecurityEvent` for `EventID == 4624` and match `IpAddress`

## Escalation Criteria
Escalate if:
- 4624 successful logon from an external IP appears
- Follow-on indicators: new admin user, service install, suspicious process execution

## Containment (Lab context)
- Snapshot evidence (screenshots + exported query results)
- If successful compromise: isolate VM (remove NSG inbound) and preserve disk for analysis
