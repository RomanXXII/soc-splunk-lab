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

## Lessons Learned

Building a functional SOC lab required significantly more effort than simply installing security tools and running tests. The majority of challenges involved validating telemetry, troubleshooting event collection, configuring Windows logging, and ensuring reliable communication between systems.

Several key lessons emerged throughout development:

* Security monitoring is only as effective as the underlying telemetry pipeline. Before investigations could begin, Sysmon, Splunk Universal Forwarder, and Splunk Enterprise all had to be validated and tested.
* Many investigations began with a single event and required correlation across multiple log sources to reconstruct what occurred. Process creation, network connection, and authentication events often provided complementary pieces of the same story.
* The same event type can represent very different activities depending on context. For example, Windows Event ID 4625 may indicate a harmless local login failure or a potentially suspicious remote authentication attempt.
* Simulating attacks is only one part of security analysis. Understanding how to investigate activity, interpret metadata, and distinguish between benign and suspicious behavior proved equally important.
* Troubleshooting infrastructure issues such as firewall policies, network configuration, event subscriptions, and log forwarding provided valuable experience in maintaining security monitoring systems.

This project developed practical experience with endpoint telemetry, SIEM workflows, attack simulation, incident investigation, and the challenges involved in building a reliable monitoring environment from the ground up.

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
