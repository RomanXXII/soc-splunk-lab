# Setup

This document summarizes the environment configuration process and the primary issues encountered while building the lab.

## Host System

* Windows 10
* VMWare Workstation
* Splunk Enterprise

## Windows Endpoint VM

* Windows 11
* Sysmon
* Splunk Universal Forwarder

## Attacker VM

* Kali Linux

# Major Challenges

## 1. VM Connectivity

Initial communication between the Windows and Kali virtual machines was inconsistent. 

Connectivity was validated using:

```bash
ping
```

and:

```powershell
Test-NetConnection
```

Eventually, verified both virtual machines were attached to the same VMware NAT network and confirmed bidirectional connectivity.

---

## 2. Splunk Receiving No Events

Log searches returned no endpoint telemetry despite Splunk being operational.

Verified:

* Splunk Enterprise was running
* Port 9997 was listening
* Universal Forwarder service was running

Discovered that event log collection inputs had not been configured properly. After adjusting inputs.conf and outputs.conf, telementry began flowing correctly to Splunk.

---

## 3. PowerShell Investigation Difficulties

Known PowerShell download activity could not initially be located through Sysmon process creation events.

Searches primarily returned Splunk's internal PowerShell helper process.

The original command had been executed inside an already-running PowerShell session, resulting in no new process creation event.

Subsequent tests launched PowerShell as a new process, generating clean Event ID 1 telemetry.

---

# Key Takeaways

Building a functioning SOC lab required significantly more troubleshooting than attack simulation itself, and cross-network connectivity proved to be more complex and detail-oriented than expected. The majority of issues involved telemetry validation, service configuration, networking, and event collection rather than Splunk searches or attack execution.

Developing a reliable data collection pipeline proved to be a prerequisite for all subsequent investigations.
