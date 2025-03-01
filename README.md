
# Microsoft Defender for Endpoint Advanced Hunting Add-on for Splunk

## Introduction

This add-on provides field extractions and CIM compatibility for the Endpoint datamodel for Microsoft Defender Advanced Hunting data.   
It also maps Device Alert events to the Alerts datamodel.   
The data is similar in content to Sysmon data and can be used by Detection Searches in i.e. Splunk Enterprise Security Content Update.   

Future versions may include support for Microsoft Defender for Office 365, Microsoft Defender for Identity and other products in the Microsoft 365 suite.


### How is this different from other Microsoft Defender Add-ons for Splunk?
#### [Microsoft 365 Defender Add-on for Splunk](https://splunkbase.splunk.com/app/4959/)
* The above uses REST API to get data, and the REST API is rate limited, whereas this TA uses Event Hub
* The above will only give you Alerts and Incidents, not the raw logs
* The above maps to the Alerts CIM data model, not the Endpoint CIM data model

My advice: Use both Microsoft Defender Advanced Hunting Add-on and Microsoft 365 Defender Add-on for Splunk, in order to get both alerts and the raw logs!

#### [TA for Defender ATP hunting API](https://splunkbase.splunk.com/app/4623/#/details)
* The above uses REST API to pull similar data at intervals, and the REST API is rate limited
* The above is not CIM compliant

My advice: You should not need this TA if you are using Microsoft Defender Advanced Hunting Add-on for Splunk.


## Installation

1. Configure Microsoft Defender for Endpoint to stream Advanced Hunting events to an Azure Event Hub:
   *  https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/raw-data-export-event-hub?view=o365-worldwide

2. Install this add-on on your Search Heads, Indexers and Heavy Forwarders (if part of your data collection topology)

3. Install and use one of these two Splunk add-ons to ingest the data:
   * Splunk Add-on for Microsoft Cloud Services (https://splunkbase.splunk.com/app/3110/)
   * Microsoft Azure Add on for Splunk (https://splunkbase.splunk.com/app/3757/)

4. When setting the sourcetype/source choose either:
   * Sourcetype: `mscs:azure:eventhub:defender:advancedhunting` (If using Splunk Add-on for Microsoft Cloud Services)
   * Source: `azure_event_hub://Defender_Security_Center` (If using Microsoft Azure Add-on for Splunk)

6. Enable the scheduled saved search **Summary - Defender Advanced Hunting Malware Summary** on one Search Head (preferably ES) in order to populate the Malware data model.

7. Verify that raw data is arriving by running the following search: `index=* eventtype="ms_defender_advanced_hunting_sourcetypes"`

8. Verify that data for the **Malware** data model is populated by the scheduled saved search by running the following search: `index* sourcetype=defender:advancedhunting:malware`

## Data Models

Data Model Compatibility:

| Product | Dataset | Category | Status |
|---|---|---|---|
| MS Defender for Endpoint | Endpoint.Ports | AdvancedHunting-DeviceNetworkEvents | Completed |
| MS Defender for Endpoint | Endpoint.Processes | AdvancedHunting-DeviceProcessEvents | Completed |
| MS Defender for Endpoint | Endpoint.Services | N/A |
| MS Defender for Endpoint | Endpoint.Filesystem | AdvancedHunting-DeviceFileEvents | Completed |
| MS Defender for Endpoint | Endpoint.Registry | AdvancedHunting-DeviceRegistryEvents | Completed |
| MS Defender for Endpoint | Alerts | AdvancedHunting-DeviceAlertEvents <br/> AdvancedHunting-AlertInfo <br/>AdvancedHunting-AlertEvidence | Completed |
| MS Defender for Endpoint | Email | AdvancedHunting-EmailEvents <br/>AdvancedHunting-EmailAttachmentInfo | Completed |
| MS Defender for Endpoint | Malware | AdvancedHunting-AlertInfo <br/>AdvancedHunting-AlertEvidence | Completed |
| MS Defender for Endpoint | Authentication | AdvancedHunting-IdentityLogonEvents <br/>AdvancedHunting-DeviceLogonEvents | Under Consideration |
| MS Defender for Endpoint | Change | AdvancedHunting-IdentityDirectoryEvents | Under Consideration |


Schema reference: https://docs.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-schema-tables?view=o365-worldwide

## Data Samples

So how does this data look like when it's ingested into Splunk? Prettified, of course:

```json
{
  "time": "2021-04-14T18:50:42.1532345Z",
  "tenantId": "4a653e38-cdfe-1337-beef-abcdefabcdef",
  "operationName": "Publish",
  "category": "AdvancedHunting-DeviceProcessEvents",
  "properties": {
    "ProcessVersionInfoCompanyName": "Microsoft Corporation",
    "ProcessVersionInfoProductName": "Microsoft® Windows® Operating System",
    "ProcessVersionInfoProductVersion": "10.0.18362.1",
    "ProcessVersionInfoInternalFileName": "whoami.exe",
    "ProcessVersionInfoOriginalFileName": "whoami.exe",
    "ProcessVersionInfoFileDescription": "whoami - displays logged on user information",
    "ProcessIntegrityLevel": "System",
    "AccountSid": "S-1-5-18",
    "LogonId": 999,
    "AccountName": "system",
    "AccountDomain": "nt authority",
    "AccountUpn": null,
    "AccountObjectId": null,
    "ProcessTokenElevation": "TokenElevationTypeDefault",
    "ProcessCreationTime": "2021-04-14T18:49:17.528333Z",
    "ProcessId": 40288,
    "FileName": "whoami.exe",
    "ProcessCommandLine": "\"whoami.exe\" /user",
    "FolderPath": "C:\\Windows\\System32\\whoami.exe",
    "FileSize": 71168,
    "MD5": "2eeeec89e705f73ffbcae014e1828788",
    "SHA256": "a8a4c4719113b071bb50d67f6e12c188b92c70eeafdfcd6f5da69b6aaa99a7fd",
    "SHA1": "8f1f9e265911956c7f8f3861c34def9b8fa63813",
    "AdditionalFields": null,
    "InitiatingProcessVersionInfoCompanyName": "Microsoft Corporation",
    "InitiatingProcessVersionInfoProductName": "Microsoft® Windows® Operating System",
    "InitiatingProcessVersionInfoProductVersion": "10.0.18362.1",
    "InitiatingProcessVersionInfoInternalFileName": "POWERSHELL",
    "InitiatingProcessVersionInfoOriginalFileName": "PowerShell.EXE",
    "InitiatingProcessVersionInfoFileDescription": "Windows PowerShell",
    "InitiatingProcessSignatureStatus": "Valid",
    "InitiatingProcessSignerType": "OsVendor",
    "InitiatingProcessFolderPath": "c:\\windows\\system32\\windowspowershell\\v1.0\\powershell.exe",
    "InitiatingProcessFileSize": 451584,
    "InitiatingProcessMD5": "cda48fc75952ad12d99e526d0b6bf70a",
    "InitiatingProcessSHA256": "908b64b1971a979c7e3e8ce4621945cba84854cb98d76367b791a6e22b5f6d53",
    "InitiatingProcessSHA1": "36c5d12033b2eaf251bae61c00690ffb17fddc87",
    "InitiatingProcessLogonId": 999,
    "InitiatingProcessAccountSid": "S-1-5-18",
    "InitiatingProcessAccountDomain": "nt authority",
    "InitiatingProcessAccountName": "system",
    "InitiatingProcessAccountUpn": null,
    "InitiatingProcessAccountObjectId": null,
    "InitiatingProcessCreationTime": "2021-04-14T18:49:15.9040226Z",
    "InitiatingProcessId": 39240,
    "InitiatingProcessFileName": "powershell.exe",
    "InitiatingProcessCommandLine": "\"powershell.exe\" -executionPolicy bypass -file  \"C:\\Program Files (x86)\\Microsoft Intune Management Extension\\Content\\DetectionScripts\\7bdb56a9-0db0-4fd3-a373-4a50613270bc_1.ps1\" ",
    "InitiatingProcessParentCreationTime": "2021-04-14T18:49:15.6500648Z",
    "InitiatingProcessParentId": 42028,
    "InitiatingProcessParentFileName": "AgentExecutor.exe",
    "InitiatingProcessIntegrityLevel": "System",
    "InitiatingProcessTokenElevation": "TokenElevationTypeDefault",
    "DeviceId": "df5b716f594eb30d61bf2b73998fea62a9b448e3",
    "AppGuardContainerId": "",
    "MachineGroup": null,
    "Timestamp": "2021-04-14T18:49:17.7849324Z",
    "DeviceName": "bobslaptop.example.com",
    "ReportId": 30779,
    "ActionType": "ProcessCreated"
  }
}
```

```json
{  
   "Tenant": "DefaultTenant",
   "category": "AdvancedHunting-EmailEvents",
   "operationName": "Publish",
   "properties": { 
     "AttachmentCount": 0,
     "ConfidenceLevel": {"Phish":"Normal","Spam":"Moderate"},
     "Connectors": null,
     "DeliveryAction": "Junked",
     "DeliveryLocation": "Junk folder",
     "DetectionMethods": {"Phish":["Spoof external domain"],"Spam":["Advanced filter"]},
     "EmailAction": "Move to junk mail folder",
     "EmailActionPolicy": "Anti-phishing spoof",
     "EmailActionPolicyGuid": "3c810b43-bc2c-4e2a-8a8f-113b5ba6a790",
     "EmailClusterId": 2887796428,
     "EmailDirection": "Inbound",
     "EmailLanguage": "en",
     "InternetMessageId": "<1624300224965.52303495.145763391.18243936023@abcsend.com>",
     "NetworkMessageId": "1ecd7b63-c817-437f-a571-2615776a3eb9",
     "OrgLevelAction": null,
     "OrgLevelPolicy": null,
     "RecipientEmailAddress": "charlie.brown@splunk.com",
     "RecipientObjectId": "0075753e-5633-4063-9ccd-00631a7b8073",
     "ReportId": "1ecd7b63-c817-437f-a571-2615776a3eb9-14781866014961308009-1",
     "SenderDisplayName": "Charles Schultz",
     "SenderFromAddress": "snoopy@peanuts.org",
     "SenderFromDomain": "peanuts.org",
     "SenderIPv4": "142.21.91.241",
     "SenderIPv6": null,
     "SenderMailFromAddress": "bounce@peanuts.org",
     "SenderMailFromDomain": "peanuts.org",
     "SenderObjectId": null,
     "Subject": "Need dogfood ASAP? click here!",
     "ThreatNames": null,
     "ThreatTypes": "Phish, Spam",
     "Timestamp": "2021-06-21T18:34:06Z",
     "UrlCount": 10,
     "UserLevelAction": null,
     "UserLevelPolicy": null,
   },
   "tenantId": "fd794b87-38b3-41bf-8055-1a790cf2efd4",
   "time": "2021-06-21T18:39:44.2979096Z"
}
```

```json
{
  "time": "2021-09-12T17:13:58.0041354Z",
  "tenantId": "2f6f2e73-daa8-4198-8b75-748a977c6016",
  "operationName": "Publish",
  "category": "AdvancedHunting-AlertInfo",
  "properties": {
    "AlertId": "da637670636373905367_-2042055267",
    "Timestamp": "2021-09-12T16:36:48.7090029Z",
    "Title": "Suspicious process injection observed",
    "ServiceSource": "Microsoft Defender for Endpoint",
    "Category": "DefenseEvasion",
    "Severity": "Medium",
    "DetectionSource": "EDR",
    "MachineGroup": null,
    "AttackTechniques": "[\"Process Injection (T1055)\",\"Portable Executable Injection (T1055.002)\"]"
  }
}
```

```json
{
  "time": "2021-09-12T17:13:58.5882514Z",
  "tenantId": "2f6f2e73-daa8-4198-8b75-748a977c6016",
  "operationName": "Publish",
  "category": "AdvancedHunting-AlertEvidence",
  "properties": {
    "AlertId": "da637670636373905367_-2042055267",
    "ServiceSource": "Microsoft Defender for Endpoint",
    "Timestamp": "2021-09-12T16:36:48.7090029Z",
    "EntityType": "Machine",
    "EvidenceRole": "Impacted",
    "SHA1": null,
    "SHA256": null,
    "ThreatFamily": null,
    "RemoteIP": null,
    "LocalIP": null,
    "RemoteUrl": null,
    "AdditionalFields": "{\"$id\":\"1\",\"HostName\":\"win-dspitz\",\"IsDomainJoined\":false,\"Type\":\"host\",\"MachineId\":\"a119946e5cbc8a4c5bde193bb12347c62a6dd3e8\",\"MachineIdType\":3,\"IsIoc\":false}",
    "AccountName": null,
    "AccountDomain": null,
    "AccountSid": null,
    "AccountObjectId": null,
    "DeviceId": "a119946e5cbc8a4c5bde193bb12347c62a6dd3e8",
    "DeviceName": "win-dspitz",
    "NetworkMessageId": null,
    "EvidenceDirection": null,
    "MachineGroup": null,
    "FileName": null,
    "FolderPath": null,
    "ProcessCommandLine": null,
    "EmailSubject": null,
    "ApplicationId": null,
    "Application": null,
    "FileSize": null,
    "RegistryKey": null,
    "RegistryValueName": null,
    "RegistryValueData": null,
    "AccountUpn": null,
    "OAuthApplicationId": null
  }
}
```

```json
{
  "action": "unknown",
  "category": "credential theft malware",
  "dest": "win-dspitz",
  "file_hash": "15c8ecf4e91a523333193e95f7a5b9729ea6142c",
  "file_name": "mimikatz-1.bat",
  "file_path": "C:\\temp\\APTSimulator-master\\APTSimulator-master\\test-sets\\credential-access",
  "id": "da637674644562536345_-97009046",
  "severity": "low",
  "signature": "Populf",
  "time": 1631867656.623709,
  "vendor_product": "Microsoft Defender for Endpoint"
}
```
## Support

This add-on is provided without official support, but is supported on a best-effort basis by the community.  
Contributions and pull requests are more than welcome.   
Git repository: https://github.com/splunk/TA-microsoft-365-defender-advanced-hunting-add-on

## Contact

Authored by Mikael Bjerkeland (mbjerkeland@splunk.com).  

### Contributors
* Mikael Bjerkeland (Splunk)
* Thomas Hillesøy (Sens Consulting)

## Copyright

Copyright 2021 Splunk Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
