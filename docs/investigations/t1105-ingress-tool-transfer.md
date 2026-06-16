# T1105 - Ingress Tool Transfer

## Objective

Simulate an attacker downloading a file from a remote host and investigate the resulting telemetry using Sysmon and Splunk.

## Attack Scenario

A Kali Linux system hosted a file using a Python HTTP server, and a PowerShell command executed on the Windows endpoint downloaded the file from the remote host.

The goal was to determine whether the activity could be identified and reconstructed using endpoint telemetry.

## Investigation

### Initial Observation

The investigation began with a Sysmon Event ID 3 (Network Connection) event indicating that PowerShell established an outbound connection to a particular web server.

Relevant observations included:

* Process: powershell.exe
* Destination Host: ATTACKER_HOST
* Destination Port: 8000
* User Context: LAB_USER

Because PowerShell rarely communicates with arbitrary web servers during normal user activity, this event warranted further investigation.

### Process Analysis

To determine what PowerShell was actually doing, a search was performed for EventCode=1 and Image=*powershell.exe. 

Initially, most results related to splunk-powershell.exe, which related to Splunk's internal helper process. After timestamp isolation, one process with a ParentImage unique from splunkd.exe suggested relation to the PowerShell request.

Opening the Event ID 1 record revealed Invoke-WebRequest, along with http://ATTACKER_HOST:8000/attacktest2.txt and -Outfile C:\Temp\attack2.txt, revealing that the command line itself explicitly invoked a request to download attacktest2.txt from the remote host and save it locally.

Both the Event ID 1 and Event ID 3 processes showed ProcessID: 10440 and similar Image and Destination Port, establishing that the PowerShell Request, Invoke-WebRequest, HTTP Connection, and ATTACKER_HOST:8000 stemmed from the same activity.

The corresponding Event ID 11 was unabled to be located, assumedly due to Splunk filtering, but since attack2.txt was present on disk, the process creation event, network event, and resulting file artifact were sufficient to confirm the transfer.

## Timeline

14:09:35
Sysmon Event ID 1
powershell.exe launched by explorer.exe

14:09:35
Command line reveals Invoke-WebRequest
to ATTACKER_HOST:8000

14:09:40
Sysmon Event ID 3
powershell.exe establishes TCP connection
to ATTACKER_HOST on port 8000

14:09:40+
attacktest2.txt appears on disk

## Findings

A PowerShell process executed a file download from a remote host using HTTP.

The activity was successfully reconstructed using Sysmon process creation and network connection telemetry, and the observed behavior is consistent with ATT&CK T1105 (Ingress Tool Transfer).

## Lessons Learned

This investigation demonstrated the importance of correlating multiple telemetry sources rather than relying on a single event. Additionally, it conveyed the importance of utilizing timeframes when investigating logs, especially within a sea of typical processes. Even without a file creation event, process creation telemetry and network telemetry were sufficient to establish what occurred and reconstruct the attack chain.
