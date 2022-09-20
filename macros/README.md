
# Macros

## Indexes Macros
All user interaction with indexes should be based on macros. Macros allow to specify a list of indexes that will be used in search.

## Macro Naming Convention
```
<macro_type>_<prefix1>...<prefixN>_<macro_name>
```

### Macro Types

| Type   | Type Name        | Definition                                                                                             |
|--------|------------------|--------------------------------------------------------------------------------------------------------|
|i       | Indexes          | Specifies an index list                                                                                |
|f       | Filter           | Events associated                                                                                      |
|s       | Search           | Predefined searches                                                                                    |

### Macro Prefixes

| Prefix       | Prefix Name      | Definition                                                                                             |
|--------------|------------------|--------------------------------------------------------------------------------------------------------|
|<event_type>  | Event Type       | Events of particular type. See Index naming for more info.                                             |
|<source_name> | Source           | Events associated with particular source.                                                              |

### Examples
```
* i_fw - all indexes containing firewall events
* i_sec - all indexes containing security events
* i_src_asa - all indexes containing events from Cisco ASA devices
* i_src_central_ftd - all indexes containing events from central Cisco FTD devices
* i_src_windows - all indexes containing events from windows sources
* f_admins - filter users to obtain only admins
```
