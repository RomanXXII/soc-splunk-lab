# SOC Homelab Investigations

A cybersecurity homelab built to simulate attacker behavior and investigate resulting telemetry using Sysmon and Splunk. The goal of this project is to develop practical blue-team skills in a toy environment by recreating common basic attacks, analyzing resulting evidence, and documenting the investigation process.

## Environment

- Kali Linux attacker VM
- Windows endpoint VM
- Sysmon telemetry collection
- Splunk Enterprise SIEM
- Splunk Universal Forwarder

## Architecture

Kali Linux is used to simulate attacker activity against a Windows endpoint. Sysmon collects endpoint telementry on the Windows system, which is forwarded to Splunk Enterprise for analysis and investigation. 

See docs/architecture.md for details.

## Investigations

| MITRE ATT&CK Technique | Investigation |
|-----------------|--------------|
| T1105 | Ingress Tool Transfer |
| T1110 | Failed Authentication |

## Retrospective

## Repository Structure

Each stage of this lab's development logs troubleshooting and subsequent lessons in their respective documents. 

```text
.
|-- README.md
|-- docs
|   |-- architecture.md
|   |-- setup.md
|   `-- investigations
|       |-- t1105-ingress-tool-transfer.md
|       `-- t1110-failed-authentication.md
`-- screenshots
    |-- t1105
    |   |-- 01-host-payload-download-log.png
    |   |-- 02-powershell-process-search.png
    |   |-- 03-powershell-download-event.png
    |   `-- 04-network-connection-event.png
    `-- t1110
        |-- 01-security-event-review.png
        |-- 02-failed-logon-alerts.png
        |-- 03-local-failed-logon-details.png
        |-- 04-remote-logon-events.png
        `-- 05-remote-failed-logon-details.png
```

## Disclaimer

All activites were performed in an isolated lab environment using virtual machines owned and controlled by the author. No unauthorized testing was performed against external systems.

The focus of this repository is on investigative methodology rather than infrastructure details. Screenshots and documentation have been sanitized to remove internal IP addresses and identifiers.
