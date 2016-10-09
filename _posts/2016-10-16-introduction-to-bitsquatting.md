---
layout: post
title:  "Introduction to bitflipping"
description: "Part one of four"
date:   2016-10-01 16:35:58 +0100
categories: DNS Configuring
---
### Introduction
As part of my day-to-day job, I'm assigned to developing a tool for monitoring [domainsquatting][wiki-cybersquatting] for a client. One narrow, but very interesting and nerdy, technique for domainsquatting is bitflipping where indiviual bits get flipped in the domain and an attacker tries to fetch misdirected traffic. This series of blogpost intends to describe the attack vector, how to configure BIND for bitflipping and how to detect it.

### What is bitflipping
So strings are stored in memory and on disk in bits and bytes, 1's and 0's, right? A capital A is represented as `0100 0001` while a lower case a is `0110 0001`.
```python
>>> bin(ord('A'))
'0b1000001'
>>> bin(ord('a'))
'0b1100001'
```
A bitflip occurs when a 0 turns into a 1 or a 1 turns into a 0. So for instance if the sixth digit, a 0, turns into a 1 in a lower case a the new byt instead represents a lower case e.
```python
>>> chr(int('01100001', 2))
'a'
>>> chr(int('01100101', 2))
'e'
```

A bitflip can occur for a number of reasons; when writing a file to disk, due to overheating in memory or by interference in transit etc. According to research made by Artem Dinaburg back in 2011 (I would highly recommend watching his presentation on [DEF CON 19][dinaburg-defcon19] and Jaeson Schultz presentation from [DEF CON 21][schultz-defcon21])

Do normal disk writes have checksums?
UDP DNS checksum?


### Tools

#### BitSquat

#### FitBlipper

### References
* [Wikipedia: Cybersquatting][wiki-cybersquatting]
* [Artem Dinaburg: Bitsquatting][dinaburg-bitsquatting]
* [Artem Dinaburg at DEF CON 19 - Bit-squatting: DNS Hijacking Without Exploitation][dinaburg-defcon19]
* [Artem Dinaburg at black hat 2011 - Bit-squatting: DNS Hijacking Without Exploitation][dinaburg-blackhat2011]
* [Jaeson Schultz at DEF CON 21 - Examining the Bitsquatting Attack Surface][schultz-defcon21]
* [Nick Nikiforakis, Steven Van Acker, Wannes Meert, Lieven Desmet, Frank Piessens, Wouter Joosen - Bitsquatting: Exploiting Bit-flips for Fun, or Profit?][bitsquatting_www2013]
* [GitHub: BitSquat][tool-bitsquat]
* [GitHub: FitBlipper][tool-fitblipper]

[wiki-cybersquatting]: https://en.wikipedia.org/wiki/Cybersquatting
[tool-bitsquat]: https://github.com/artemdinaburg/bitsquat-script
[tool-fitblipper]: https://github.com/chadillac/FitBlipper
[dinaburg-bitsquatting]: http://dinaburg.org/bitsquatting.html
[dinaburg-defcon19]: https://www.youtube.com/watch?v=lZ8s1JwtNas
[dinaburg-blackhat2011]: https://www.youtube.com/watch?v=_si0FYl_IOA/
[schultz-defcon21]: https://www.youtube.com/watch?v=j2FVFVHVvgg
[bitsquatting_www2013]: https://securitee.org/files/bitsquatting_www2013.pdf
