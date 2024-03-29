# Splunk Index data splitting and naming convention v1
## General info
### Definitions
* **event type** - group of events generated by event source that that can be distinguished from others by a set of characteristics. E.g. firewall events, 
* **event source** - system that the events originate from
* **event source type** - type of event sources that share common characteristics and generate the same or very similar event types

## Why split indexes?
There are three main reasons to split indexes: data retention, RBAC and performance considerations. Splitting data into multiple indexes is required to narrow down the data searched, which is especially important for data model acceleration searches.

### Data Retention
Splitting data from a single source into multiple indexes allows to set up different retention periods. E.g. a very high volume firewall logs might be kept only for 30 days, but audit and authentication data will be kept for 3 years, either by choice or because of regulations.

### Role Based Access Control
Having different types of data in different indexes allows assigning the rights to indexes based on predefined roles. Following previous example, firewall logs might be accessible to multiple teams, but audit and authentication are only visible to audit and security teams.

### Performance
Splitting indexes allows separating different types of data, which in turn allows constructing queries that are faster. This is especially the case where different event type sets have vastly different cardinality.
Additionally, if data model acceleration is used, splitting indexes based on data model specification allows optimizing the data model acceleration searches.

## Naming convention
The following naming shall be used for Splunk Indexes:
```
<prefix1>...<prefixN>_[<context_prefix>_]<event_source_group>[_<suffix>]
```

* **prefix** - defines an event type; multiple can be specified in certain situations (e.g. op and app to denote operational logs from custom app)
* **context_prefix** - optional; if the events originate from multiple data context, e.g. localizations or business units, this can be used to differentiate the contexts
* **event_source_group** - name identifying the sources that the events in this index originate from
* **suffix** - optional, additional specifiers; best to use for separating special data sets which require special access rules

### Predefined Prefixes

| Prefix  |  Full Name                            |  Definition                                                                                                                             |  Examples                                     |
|---------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| acc     | Accounting                            | Network accounting data                                                                                                                 | RADIUS Accounting                             |
| app     |                                       | Third-party applications, used with the application name                                                                                |                                               |
| auth    | Authentication                        | Authentication (success, failure) events                                                                                                |                                               |
| authz   | Authorization                         | Access to resources authorization (allowed, blocked)                                                                                    |                                               |
| audit   | Audit                                 | Object and system access events (creation, modification, deletion, etc.)                                                                |                                               |
| av      | Antivirus / Malware                   |                                                                                                                                         |                                               |
| dlp     | Data Leak Prevention                  | Events from DLP systems                                                                                                                 |                                               |
| dns     | DNS Queries                           | DNS Queries and responses                                                                                                               | Query/Response: A, AAA, TXT                   |
| dhcp    | DHCP lease                            | DHCP lease events                                                                                                                       | DHCP IP Request, offer                        |
| fw      | Firewall                              | ISO/OSI layer 3 connectivity (allow, drop, teardown) events generated based on firewall rules and connection tracking                   | Connection or packet accepted/denied/teardown |
| ips     | Intrusion Prevention System (network) | Security events based on rules and heuristics applied to network data                                                                   |                                               |
| mail    | e-mail                                | E-mail content/message tracking                                                                                                         |                                               |
| op      | Operational                           | Operational events                                                                                                                      | Updates, restarts                             |
| os      | Operating System                      | Events from operating system not matching other categories                                                                              |                                               |
| perf    | Performance                           | Performance related events and statistics                                                                                               |                                               |
| proxy   | Web proxy                             | Web proxy events for web protocols (HTTP, FTP, etc.)                                                                                    |                                               |
| rproxy  | Reverse web proxy                     | Reverse Proxy/Load Balancer for web protocols (HTTP, FTP, etc.)                                                                         |                                               |
| sandbox | File Sandbox                          | File analysis in sandbox system events                                                                                                  |                                               |
| sec     | Security                              | Alerts from security software and devices, based on internal detection capabilities, rules, etc. not matching other seciruty categories | malware, port scan, spoofing                  |
| vpn     | VPN                                   | VPN tunnel and access events                                                                                                            |                                               |
| vuln    | Vulnerabilities                       | Vulnerabilities found                                                                                                                   |                                               |
| waf     | Web Application Firewall              | Access to web applications with added security context                                                                                  |                                               |
| wifi    | Wireless IEEE 802.11 session          | Wireless IEEE 802.11 sessions, client connections, wireless station information                                                         |                                               |
|         |                                       |                                                                                                                                         |                                               |
| default | Default                               | All events that were not assigned to other indexes                                                                                      |                                               |
| misc    | Miscellaneous                         | Events not matching other prefixes                                                                                                      |                                               |


### Proposed Suffixes

| Suffix | Suffix Name      | Definition                                                                                             | Examples|
|--------|------------------|--------------------------------------------------------------------------------------------------------|---|
|pii     | Personally Identifiable Information| Events containing PII                                                                |-|
|pcidss  | PCI DSS          | Object and system access                                                                               |-|

### Examples

```
* fw_asa - firewall logs from Cisco ASA devices
* audit_asa - audit logs from Cisco ASA devices
* sec_ftd - security logs from Cisco FTD devices
* fw_central_ftd - firewall logs from Cisco FTD devices in central context
* app_examplecom - application logs from example.com application
* app_examplecom_pii - application logs from example.com application that contain PII
* op_app_examplecom - operational logs from application example.com
* perf_rsyslog - rsyslog performance logs
```

## Indexes Macros
All user interaction with indexes should be based on macros. Macros allow to specify a list of indexes that will be used in search. See macros for more info.