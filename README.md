
# Microsoft 365 Defender Advanced Hunting Add-on for Splunk

## Introduction

This add-on provides field extractions and CIM compatibility for the Endpoint datamodel for Microsoft 365 Defender Advanced Hunting data.

Future versions will include support for Microsoft Defender for Office 365, Microsoft Defender for Identity and other products in the Microsoft 365 suite.

## Installation

1. Configure Microsoft Defender for Endpoint to stream Advanced Hunting events to an Azure Event Hub:
   *  https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/raw-data-export-event-hub?view=o365-worldwide

2. Install this add-on on your Search Heads and Indexers

3. Install and use one of these two Splunk add-ons to ingest the data:
   * Splunk Add-on for Microsoft Cloud Services (https://splunkbase.splunk.com/app/3110/)
   * Microsoft Azure Add on for Splunk (https://splunkbase.splunk.com/app/3757/)

4. When setting the sourcetype/source choose either:
   * Source: `azure_event_hub://Defender_Security_Center` (If using Microsoft Azure Add-on for Splunk)
   * Sourcetype: `mscs:azure:eventhub:defender:advancedhunting` (If using Splunk Add-on for Microsoft Cloud Services)

5. Test that data is arriving by running the following search: `index=* eventtype="ms_defender_advanced_hunting_sourcetypes"`

## Data Models

Data Model Compatibility:

| Product | Dataset | Category | Status |
|---|---|---|---|
| MS Defender for Endpoint | Endpoint.Ports | AdvancedHunting-DeviceNetworkEvents | Completed |
| MS Defender for Endpoint | Endpoint.Processes | AdvancedHunting-DeviceProcessEvents | Completed |
| MS Defender for Endpoint | Endpoint.Services | N/A |
| MS Defender for Endpoint | Endpoint.Filesystem | AdvancedHunting-DeviceFileEvents | Completed |
| MS Defender for Endpoint | Endpoint.Registry | AdvancedHunting-DeviceRegistryEvents | Completed |

Schema reference: https://docs.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-schema-tables?view=o365-worldwide

## Support

This add-on is provided without official support, but is supported on a best-effort basis by the community.  
Contributions and pull requests are more than welcome.
Official Git repo: https://github.com/inspired/TA-microsoft-defender-for-endpoint-advanced-hunting-add-on-for-splunk

## Contact

Authored by Mikael Bjerkeland (mbjerkeland@splunk.com)
