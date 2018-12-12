| Title        | Suspicious SYSVOL Domain Group Policy Access |
|:-------------------|:------------------|
| Description        | Detects Access to Domain Group Policies stored in SYSVOL |
| Tags               |   |
| ATT&amp;CK Tactic | ('Credential Access', 'TA0006')  |
| ATT&amp;CK Technique | T1003  |
| Dataneeded         | DN_0003_windows_sysmon_process_creation_1, DN_0002_windows_process_creation_with_commandline_4688 |
| Triggering         | T1003 |
| Severity Level     | medium       |
| False Positives    | administrative activity |
| Development Status | experimental      |
| References         | https://adsecurity.org/?p=2288, https://www.hybrid-analysis.com/sample/f2943f5e45befa52fb12748ca7171d30096e1d4fc3c365561497c618341299d5?environmentId=100 |
| Author             | Markus Neis      |


## Detection Rules

### Sigma rule

```
---
action: global
title: Suspicious SYSVOL Domain Group Policy Access
status: experimental
description: Detects Access to Domain Group Policies stored in SYSVOL
references:
    - https://adsecurity.org/?p=2288
    - https://www.hybrid-analysis.com/sample/f2943f5e45befa52fb12748ca7171d30096e1d4fc3c365561497c618341299d5?environmentId=100
author: Markus Neis
date: 2018/04/09
tags:
    - attack.credential_access
    - attack.t1003
detection:
    selection:
        CommandLine: '*\SYSVOL\*\policies\*'
    condition: selection
falsepositives: 
    - administrative activity 
level: medium
---
logsource:
    product: windows
    service: sysmon
detection:
    selection:
        EventID: 1
---
logsource:
    product: windows
    service: security
    description: 'Requirements: Audit Policy : Detailed Tracking > Audit Process creation, Group Policy : Administrative Templates\System\Audit Process Creation'
detection:
    selection:
        EventID: 4688

```











Graylog

```
b'(EventID:"1" AND CommandLine:"*\\\\SYSVOL\\*\\\\policies\\*")\n(EventID:"4688" AND CommandLine:"*\\\\SYSVOL\\*\\\\policies\\*")\n'
```
