# Architecture

## Overview

This lab conssits of three primary systems:

1. Kali Linux attacker VM
2. Windows endpoint VM
3. Splunk Enterprise SIEM

The purpose of this structure is to simulate attacker activity while collecting and analyzing logs from the target system.

Kali Linux -- (Simulated Attacks) --> Windows Endpoint -- (Sysmon Events) --> Splunk Universal Forwarder -- (Forwarded Telementry) --> Splunk Enterprise

## Components

### Kali Linux

Used as the attacker host machine, initiating HTTP file transfers, authentication requests, network reconnaissance, PowerShell-related scenarios, etc.

### Windows Endpoint

Acts as the target system, generating Security Event Logs, Process Creation Events, Network Connection Events, File Creation Events, etc.

### Sysmon

Extends native Windows logging by providing process creation telementry, network connection telementry, file creation telementry, and process lineage info.

### Splunk Universal Forwarder

Collects Windows event logs and forwards them to Splunk Enterprise, namely Security and Microsoft-Windows-Sysmon/Operational.

### Splunk Enterprise

Serves as the central analysis platform, used to search telementry, correlate events, investigate alerts, build attack timelines, and validate detections.

### Disclaimer

The focus of this repository is on investigative methodology rather than infrastructure details. Screenshots and documentation have been sanitized to remove internal IP addresses and identifiers.


