---
layout: post
title:  "Burp Intruder Attack Types"
description: ""
date:   2017-02-04 12:05:32 +0100
categories: [Burp, Fuzzing, Mental notes]
---

### Introduction

This is just a mental note since I keep forgetting how the different Burp Suite intruder attack types work.

### Wordlists

| eng   | ger  | fr    | ger2 | fr2 |
| ----- | ---- | ----- | ---- | --- |
| one   | ein  | un    | ein  | un  |
| two   | zwei | deux  | zwei |     |
| three | drei | trois |      |     |

### Basic request
```
GET /page?param1=§p1§&param2=§p2§&param3=§p3§
```

### Sniper

> This uses a single set of payloads. It targets each payload position in turn, and places each payload into that position in turn. Positions that are not targeted for a given request are not affected - the position markers are removed and any enclosed text that appears between them in the template remains unchanged. This attack type is useful for fuzzing a number of request parameters individually for common vulnerabilities. The total number of requests generated in the attack is the product of the number of positions and the number of payloads in the payload set.

#### Wordlist

Single (eng)

#### Parameters

| p1    | p2    | p3    |
| ----- | ----- | ----- |
| p1    | p2    | p3    |
| one   | p2    | p3    |
| two   | p2    | p3    |
| three | p2    | p3    |
| p1    | one   | p3    |
| p1    | two   | p3    |
| p1    | three | p3    |
| p1    | p2    | one   |
| p1    | p2    | two   |
| p1    | p2    | three |


#### Requests

```
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=p2&param3=p3
GET /page?param1=two&param2=p2&param3=p3
GET /page?param1=three&param2=p2&param3=p3
GET /page?param1=p1&param2=one&param3=p3
GET /page?param1=p1&param2=two&param3=p3
GET /page?param1=p1&param2=thee&param3=p3
GET /page?param1=p1&param2=p2&param3=one
GET /page?param1=p1&param2=p2&param3=two
GET /page?param1=p1&param2=p2&param3=three
```

### Battering ram

> This uses a single set of payloads. It iterates through the payloads, and places the same payload into all of the defined payload positions at once. This attack type is useful where an attack requires the same input to be inserted in multiple places within the request (e.g. a username within a Cookie and a body parameter). The total number of requests generated in the attack is the number of payloads in the payload set.

#### Wordlist

Single (eng)

#### Parameters

| p1    | p2    | p3    |
| ----- | ----- | ----- |
| p1    | p2    | p3    |
| one   | one   | one   |
| two   | two   | two   |
| three | three | three |


#### Requests

```
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=one&param3=one
GET /page?param1=two&param2=two&param3=two
GET /page?param1=three&param2=three&param3=three
```

### Pitchfork

> This uses multiple payload sets. There is a different payload set for each defined position (up to a maximum of 20). The attack iterates through all payload sets simultaneously, and places one payload into each defined position. In other words, the first request will place the first payload from payload set 1 into position 1 and the first payload from payload set 2 into position 2; the second request will place the second payload from payload set 1 into position 1 and the second payload from payload set 2 into position 2, etc. This attack type is useful where an attack requires different but related input to be inserted in multiple places within the request (e.g. a username in one parameter, and a known ID number corresponding to that username in another parameter). The total number of requests generated in the attack is the number of payloads in the smallest payload set.

#### Wordlist:

Multiple (eng, ger, fr)

#### Parameters:

| p1    | p2   | p3    |
| ----- | ---- | ----- |
| p1    | p2   | p3    |
| one   | ein  | un    |
| two   | zwei | deux  |
| three | drei | trois |


#### Requests:

```
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=ein&param3=un
GET /page?param1=two&param2=zwei&param3=deux
GET /page?param1=three&param2=drei&param3=trois
```

#### Wordlist:

Multiple (eng, ger2, fr2)

#### Parameters:

| p1    | p2   | p3    |
| ----- | ---- | ----- |
| p1    | p2   | p3    |
| one   | ein  | un    |

#### Requests:

```
Wordlist: eng, ger2, fr2
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=ein&param3=un
```


### Cluster bomb

> This uses multiple payload sets. There is a different payload set for each defined position (up to a maximum of 20). The attack iterates through each payload set in turn, so that all permutations of payload combinations are tested. I.e., if there are two payload positions, the attack will place the first payload from payload set 2 into position 2, and iterate through all the payloads in payload set 1 in position 1; it will then place the second payload from payload set 2 into position 2, and iterate through all the payloads in payload set 1 in position 1. This attack type is useful where an attack requires different and unrelated or unknown input to be inserted in multiple places within the request (e.g. when guessing credentials, a username in one parameter, and a password in another parameter). The total number of requests generated in the attack is the product of the number of payloads in all defined payload sets - this may be extremely large.

#### Wordlist:

Multiple (eng, ger, fr)

#### Parameters:

| p1    | p2   | p3    |
| ----- | ---- | ----- |
| one   | ein  | un    |
| two   | ein  | un    |
| three | ein  | un    |
| one   | zwei | un    |
| two   | zwei | un    |
| three | zwei | un    |
| one   | drei | un    |
| two   | drei | un    |
| three | drei | un    |
| one   | ein  | deux  |
| two   | ein  | deux  |
| three | ein  | deux  |
| one   | zwei | deux  |
| two   | zwei | deux  |
| three | zwei | deux  |
| one   | drei | deux  |
| two   | drei | deux  |
| three | drei | deux  |
| one   | ein  | trois |
| two   | ein  | trois |
| three | ein  | trois |
| one   | zwei | trois |
| two   | zwei | trois |
| three | zwei | trois |
| one   | drei | trois |
| two   | drei | trois |
| three | drei | trois |

#### Requests:

```
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=ein&param3=un
GET /page?param1=two&param2=ein&param3=un
GET /page?param1=three&param2=ein&param3=un
GET /page?param1=one&param2=zwei&param3=un
GET /page?param1=two&param2=zwei&param3=un
GET /page?param1=three&param2=zwei&param3=un
GET /page?param1=one&param2=drei&param3=un
GET /page?param1=two&param2=drei&param3=un
GET /page?param1=three&param2=drei&param3=un
GET /page?param1=one&param2=ein&param3=deux
GET /page?param1=two&param2=ein&param3=deux
GET /page?param1=three&param2=ein&param3=deux
GET /page?param1=one&param2=zwei&param3=deux
GET /page?param1=two&param2=zwei&param3=deux
GET /page?param1=three&param2=zwei&param3=deux
GET /page?param1=one&param2=drei&param3=deux
GET /page?param1=two&param2=drei&param3=deux
GET /page?param1=three&param2=drei&param3=deux
GET /page?param1=one&param2=ein&param3=trois
GET /page?param1=two&param2=ein&param3=trois
GET /page?param1=three&param2=ein&param3=trois
GET /page?param1=one&param2=zwei&param3=trois
GET /page?param1=two&param2=zwei&param3=trois
GET /page?param1=three&param2=zwei&param3=trois
GET /page?param1=one&param2=drei&param3=trois
GET /page?param1=two&param2=drei&param3=trois
GET /page?param1=three&param2=drei&param3=trois
```

#### Wordlist:

Multiple (eng, ger2, fr2)

#### Parameters:

| p1    | p2   | p3    |
| ----- | ---- | ----- |
| one   | ein  | un    |
| two   | ein  | un    |
| three | ein  | un    |
| one   | zwei | un    |
| two   | zwei | un    |
| three | zwei | un    |

#### Requests:

```
GET /page?param1=p1&param2=p2&param3=p3
GET /page?param1=one&param2=ein&param3=un
GET /page?param1=two&param2=ein&param3=un
GET /page?param1=three&param2=ein&param3=un
GET /page?param1=one&param2=zwei&param3=un
GET /page?param1=two&param2=zwei&param3=un
GET /page?param1=three&param2=zwei&param3=un
```

### References
* [Portwsigger: Payload Positions][portswigger-intruder_positions]

[portswigger-intruder_positions]: https://portswigger.net/burp/help/intruder_positions.html
