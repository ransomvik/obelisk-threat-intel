	Language:       Python

	Version:        3.40

	Original Date:  05-02-2015

	Author:         Derek Arnold

 	Company:        Obelisk Security

 	Purpose:        Gathers various IPs from open source threat lists and parses them into a Splunk-friendly key/value pair format.

  	Syntax:         python ./obelisk_threat_intel.py

   	Copyright (C):  2018 Derek Arnold (ransomvik)

 	License:	This program is free software: you can redistribute it and/or modify
				it under the terms of the GNU General Public License as published by
				the Free Software Foundation, either version 3 of the License, or
                any later version.

            	This program is distributed in the hope that it will be useful,
                but WITHOUT ANY WARRANTY; without even the implied warranty of
               	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
               	GNU General Public License for more details. See <http://www.gnu.org/licenses/>

	  Support:	This is an open source project, no support provided, public repository available.
 			https://github.com/ransomvik/obelisk-threat-intel

          Change Log:	05-01-2015 DPA	Created.
                    	08-08-2015 DPA	Added TOR, OpenBL, split out Palevo into its own function.
			10-07-2015 DPA	Many new features and functionality.
		 	02-12-2016 DPA	Added email alerting feature, improved search speed.
			05-16-2016 DPA	Incorporated lookup tables to improve search speed further.
			08-27-2016 DPA	Split the app into obelisk_threat_intel and obelisk_TA_threat
			03-29-2017 DPA	Cloud certification changes.
                        03-25-2017 DPA  Re-branding to Obelisk
                                          

Overview:
---------
Obelisk Threat Intel is a Splunk App that automatically correlates your data
with several popular open threat lists. After a few mouse clicks we can start
hunting for log sources that are reaching out to, or being attacked from,
known attackers. The app can provide increased visibility to potentially
malicious activity going on in the organization.

Features:
--------
*Threat list visualization that shows where most of the attackers are located on a globe.
*Easily choose indexes, sourcetypes, or hosts for log entries that match
threat list destination IPs, domains, and URLs
*Email alerting feature.
*Easy setup screen.
*IP search feature that displays threat list activity.
*Domain search feature that displays threat list activity.
*Updated information is pulled down from the web every 8 hours.

Prerequisites:
--------------
*Splunk 6.x or 7.x
*Linux or Windows Operating System
*If there is a distributed environment, install the app on the ad hoc search head only.
*Web access is required to several threat lists.

Special note:
-------------
As of version 3.10, the app has split into two: obelisk_threat_intel (this app) and obelisk_TA_threat.
The obelisk_threat_intel app continues to be installed on search heads. the obelisk_TA_threat app
is to be installed on a single heavy forwarder in the environment only.

Install matrix:
--------------

+-----------------+----------------------+
| Splunk role     | App to install       |
+=================+======================+
| Indexer         | none*                |
+-----------------+----------------------+
| Search head     | obelisk-threat-intel |
+-----------------+----------------------+
| Heavy forwarder | TA_obelisk-threat    |
+-----------------+----------------------+
*create an "obelisk" index on each indexer
*if no heavy forwarder exists in your environment, install TA_obelisk-threat on a single search head.

Install:
--------
*Login to Splunk as an admin.
*Go to Apps->Manage apps
*Click Install app from file.
*Browse to the file folder with the app .tar.gz file.
*Choose the file and click OK.
*After the app is uploaded and installed, restart Splunk.
*Launch the app to complete the setup screen.

Upgrade Instructions:
---------------------
*Stop Splunk
*Remove the app from the directory structure on Linux:
rm –rf /opt/splunk/etc/apps/obelisk-threat-intel
rm -rf /opt/splunk/etc/apps/optiv_threat_intel
rm -rf /opt/splunk/etc/apps/TA_obelisk-threat
or on Windows:
c:\Program Files\Splunk\etc\apps\obelisk-threat-intel
c:\Program Files\Splunk\etc\apps\optiv_threat_intel
c:\Program Files\Splunk\etc\apps\TA_obelisk-threat
*Start Splunk
*Install using the steps shown in the Install section.
*After the app is uploaded and installed, restart Splunk.

Support:
--------
This is an open source project, no support provided, public repository available.
                        https://github.com/ransomvik/obelisk-threat-intel

Dependencies:
-------------
This app depends on TA_obelisk-threat being installed on one Splunk server in the environment.
See the Install Matrix section above for details.

Saved Search Documentation:
---------------------------
There are 19 saved searches included in this app in the file savedsearches.conf, described below.
Number- 1
Name- Obelisk - Populate Summary Index 1
Purpose- Populate summary index with data on AlienVault_IP_Blocklist for fast dashboard load times
Schedule- 5x/day at 12 past the hour
Required- Yes

Number- 2
Name- Obelisk - Populate Summary Index 2
Purpose- Populate summary index with data on HP_Hosts_By_MalwareBytes blocklist for fast dashboard load times
Schedule- 5x/day at 15 past the hour
Required- Yes

Number- 3
Name- Obelisk - Populate Summary Index 3
Purpose- Populate summary index with data on Binary_Defense_IPs blocklist for fast dashboard load times
Schedule- 5x/day at 17 past the hour
Required- No

Number- 4
Name- Obelisk - Populate Summary Index 4
Purpose- Populate summary index with data on Phish_Tank_URLs blocklist for fast dashboard load times
Schedule- 5x/day at 17 past the hour
Required- No

Number- 5
Name- Obelisk - Threat List Hit on Destination IP - Summary 1
Purpose- Populate summary index by searching the first defined network index for threat list matches on destination IP
Schedule- 5x/day at 44 past the hour
Required- No

Number- 6
Name- Obelisk - Threat List Hit on Destination IP - Summary 2
Purpose- Populate summary index by searching the second defined network index for threat list matches on destination IP
Schedule- 5x/day at 49 past the hour
Required- No

Number- 7
Name- Obelisk - Threat List Hit on Destination IP - Summary 3
Purpose- Populate summary index by searching the third defined network index for threat list matches on destination IP
Schedule- 5x/day at 54 past the hour
Required- No

Number- 8
Name- Obelisk Populate Lookup - AlienVault - dest_ip
Purpose- Populate a lookup with AlienVault_IP_Blocklist destination IPs for fast searching
Schedule- 4x/day at 5 past the hour
Required- Yes

Number- 9
Name- Obelisk Populate Lookup - Binary Defense - dest_ip
Purpose- Populate a lookup with Binary_Defense_IPs destination IPs for fast searching
Schedule- 4x/day at 10 past the hour
Required- Yes

Number- 10
Name- Obelisk Populate Lookup - CI_Army_Badguys - dest_ip
Purpose- Populate a lookup with CI_Army_Badguys destination IPs for fast searching
Schedule- 4x/day at 30 past the hour
Required- Yes

Number- 11
Name- Obelisk Populate Lookup - EmergingThreats - dest_ip
Purpose- Populate a lookup with Emerging_Threats_Compromised_IPs destination IPs for fast searching
Schedule- 4x/day at 25 past the hour
Required- Yes

Number- 12
Name- Obelisk Populate Lookup - OpenBL_1day - dest_ip
Purpose- Populate a lookup with OpenBL_1day destination IPs for fast searching
Schedule- 4x/day at 35 past the hour
Required- Yes

Number- 13
Name- Obelisk Populate Lookup - Spamhaus_Drop_Nets - dest_ip
Purpose- Populate a lookup with Spamhaus_Drop_Nets destination IPs for fast searching
Schedule- 4x/day at 40 past the hour
Required- Yes

Number- 14
Name- Obelisk Populate Lookup - Talos Intel IPs - dest_ip
Purpose- Populate a lookup with talos_intel_IPs destination IPs for fast searching
Schedule- 4x/day at 15 past the hour
Required- Yes

Number- 15
Name- Obelisk Populate Lookup - TorExitNodes - dest_ip
Purpose- Populate a lookup with TorExitNodes destination IPs for fast searching
Schedule- 4x/day at 20 past the hour
Required- Yes

Number- 16
Name- Obelisk Populate Lookup - all threat lists
Purpose- Populate a lookup aggregating all other lookups into a single table for fast searching
Schedule- 4x/day at 56 past the hour
Required- Yes

Number- 17
Name- Obelisk Populate Lookup - ransomware Abuse.ch IPs - dest_ip
Purpose- Populate a lookup with ransomware Abuse.ch IPs destination IPs for fast searching
Schedule- 4x/day at 45 past the hour
Required- Yes

Number- 18
Name- Obelisk Populate Lookup - bambenekIPs IPs - dest_ip
Purpose- Populate a lookup with ransomware bambenekIPs\ destination IPs for fast searching
Schedule- 4x/day at 52 past the hour
Required- Yes

Number- 18
Name- Obelisk Populate Lookup - bambenekIPs IPs - dest_ip
Purpose- Populate a lookup with ransomware bambenekIPs\ destination IPs for fast searching
Schedule- 4x/day at 52 past the hour
Required- Yes

Number- 19
Name- Obelisk Threat list activity 3 network indexes by dest_ip
Purpose- Search all three defined network indexes for threat list matches then display and enrich the results.
Schedule- N/A
Required- No
