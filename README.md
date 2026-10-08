**Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab**

Overview

This lab was designed to simulate the daily responsibilities of a Tier 1 Security Operations Center (SOC) Analyst using Splunk Enterprise. The project consists of twenty security event datasets representing common enterprise security telemetry sources, including authentication logs, firewall events, web server activity, DNS queries, VPN access logs, Windows security events, endpoint security alerts, and network traffic data.

To create a realistic SOC environment, all datasets were custom-generated using Python scripts. The Python-based log generator was used to produce structured security events that simulate real-world enterprise activity and common security monitoring scenarios. Generating the datasets programmatically provided complete control over the event content while ensuring that each dataset contained sufficient data for meaningful Splunk analysis and investigation.

After generating the datasets, each log file was ingested into Splunk Enterprise and assigned to its appropriate index based on the security domain it represented. The datasets were then analyzed using Splunk Search Processing Language (SPL) to perform statistical investigations and event analysis.

The primary objective of this lab is to develop practical experience with log ingestion, indexing, event monitoring, and statistical analysis using Splunk Enterprise. Unlike dashboard-focused projects, this lab intentionally emphasizes raw event analysis and SPL-based investigations. No dashboards, visualizations, panels, reports, alerts, lookups, or correlation searches were used. Instead, the focus remained on understanding security events, identifying patterns and trends, and developing the search and analysis skills commonly used by Tier 1 SOC analysts during daily monitoring and triage activities.

For example, the Failed Login Attempts dataset was ingested into the authentication index and analyzed using SPL to count failed password attempts by source IP address and user account. Similar statistical investigations were performed across all twenty security event datasets, allowing the analyst to practice working with multiple security data sources commonly found in enterprise environments.

### Security Event Categories

This lab covers the following twenty security monitoring scenarios:

1.  Failed Login Attempts

2.  Successful Logins

3.  Top Source IP Addresses

4.  Top Destination IP Addresses

5.  Firewall Blocked Traffic

6.  Firewall Allowed Traffic

7.  HTTP 404 Errors

8.  HTTP 500 Errors

9.  DNS Query Monitoring

10. VPN Login Activity

11. After-Hours Login Detection

12. New User Account Creation

13. User Account Lockouts

14. Disabled Account Usage

15. Antivirus Detections

16. Top Talkers by Bandwidth

17. Critical Security Events

18. Most Active Hosts

19. Login Failures by User

20. Event Counts by Sourcetype

### Skills Practiced

Throughout this lab, the following skills were developed and reinforced:

- Python log generation and dataset creation

- Security data engineering fundamentals

- Splunk Enterprise administration

- Log ingestion and indexing

- Search Processing Language (SPL)

- Statistical analysis using SPL commands

- Security event monitoring

- Authentication analysis

- Firewall traffic analysis

- DNS activity investigation

- VPN monitoring and access analysis

- Windows security event analysis

- Endpoint security monitoring

- Network traffic analysis

- Security operations workflows

- Event triage and investigation

- Threat detection fundamentals

- SOC Tier 1 analyst methodologies

- Security telemetry analysis

- Log source management and organization

### Outcome

Upon completion of this lab, the analyst gained hands-on experience creating realistic security datasets using Python, ingesting multiple log sources into Splunk Enterprise, and performing statistical investigations using SPL. The project provided practical exposure to authentication monitoring, firewall analysis, web log investigation, DNS monitoring, VPN activity analysis, Windows security events, endpoint security alerts, and network traffic analysis.

By working directly with raw security events rather than dashboards or visualizations, the lab strengthened the analyst's ability to interpret log data, identify security-relevant activity, and perform the types of investigations commonly conducted within a Security Operations Center. The completed project serves as a foundational SOC monitoring portfolio piece and prepares the analyst for more advanced Tier 2 activities such as threat hunting, incident response, malware analysis, persistence detection, and adversary behavior investigations.

1. Python script file (generate_tier1_soc_datasets.py)

2\. Terminal execution showing the datasets being created

3\. The folder containing the 20 generated datasets

4\. Splunk ingestion\
5. Splunk searches and results\
\
(Images 1 through 3)



| \#  | Dataset File               | Splunk Index |
|-----|----------------------------|--------------|
| 1   | Failed_logins.log          | auth         |
| 2   | successful_logins.log      | auth         |
| 3   | top_source_ips.log         | network      |
| 4   | top_destination_ips.log    | network      |
| 5   | firewall_blocked.log       | firewall     |
| 6   | Firewall_allowed.log       | Firewall     |
| 7   | http_404.log               | web          |
| 8   | http_500.log               | web          |
| 9   | dns_queries.log            | dns          |
| 10  | vpn_logins.log             | vpn          |
| 11  | after_hours_logins.log     | auth         |
| 12  | new_users.log              | windows      |
| 13  | account_lockouts.log       | windows      |
| 14  | disabled_account_usage.log | windows      |
| 15  | antivirus_detections.log   | endpoint     |
| 16  | bandwidth_top_talkers.log  | network      |
| 17  | critical_events.log        | security     |
| 18  | active_hosts.log           | security     |
| 19  | login_failures_by_user.log | auth         |
| 20  | sourcetype_count.log       | security     |

Note: This repository contains one security event investigation from the Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab series. For the remaining security event investigations, please refer to the corresponding repositories in this project collection.

### Lab: Top_destination_ips.log security Event

Dataset: top_destination_ips.log\
Index: network\
Sourcetype: top_destination_ips

### 1. Confirm Dataset Ingestion

Query:\
index=network sourcetype=top_destination_ips\
\| stats count by source index sourcetype

Was the dataset successfully ingested into Splunk?

Verifying:\
source\
index\
sourcetype\
event count (Image 4)

### 2. View Raw Events

Query:\
index=network sourcetype=top_destination_ips\
\| table \_time src_ip dest_ip bytes

What source IPs communicated with which destination IPs and how much data was transferred?

Purpose: Review the actual network communications. (Image 5)

### 3. Most Contacted Destination IPs

Query:\
index=network sourcetype=top_destination_ips\
\| stats count by dest_ip\
\| sort -count

Which destination IP addresses received the highest number of network events?

Purpose: Identify the most frequently contacted destinations. (Image 6)

### 4. Total Bytes by Destination IP

Query:\
index=network sourcetype=top_destination_ips\
\| stats sum(bytes) as total_bytes by dest_ip\
\| sort -total_bytes

Which destination IP addresses received the largest total amount of data?

Purpose: Identify the highest-volume destinations. (Image 7)

### 5. Source-to-Destination Communication

Query:\
index=network sourcetype=top_destination_ips\
\| stats count by src_ip dest_ip\
\| sort -count

Which source IP and destination IP pairs communicated most frequently?

Purpose: Identify the strongest communication relationships. (Image 8)

### 6. Unique Source IPs per Destination

Query:\
index=network sourcetype=top_destination_ips\
\| stats dc(src_ip) as unique_sources by dest_ip\
\| sort -unique_sources

Which destination IP addresses were contacted by the greatest number of unique source IP addresses?

Purpose: Identify the most broadly accessed destinations. (Image 9)

### 7. Destination Traffic Summary

Query:\
index=network sourcetype=top_destination_ips\
\| stats count as total_events sum(bytes) as total_bytes by dest_ip\
\| sort -total_bytes

Which destination IP addresses received the highest overall activity based on event count and traffic volume? (Image 10)

Purpose: Combine frequency and bandwidth into a single view.

### 8. Executive Summary

Query:\
index=network sourcetype=top_destination_ips\
\| stats dc(src_ip) as unique_sources dc(dest_ip) as unique_destinations sum(bytes) as total_bytes count as total_events

What is the overall summary of network communication activity in this dataset?

Purpose: Provide a high-level SOC summary:

Unique source IPs\
Unique destination IPs\
Total bytes transferred\
Total events (Image 11)

