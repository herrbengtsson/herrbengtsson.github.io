---
layout: post
title:  "Introduction to bitflipping"
description: "Part one of four"
date:   2016-12-04 14:45:58 +0100
categories: DNS Configuring Bitsquatting Bitflipping
cover: /assets/images/old-memories-1458085-10.jpg
---

As part of my day-to-day job, I'm assigned to developing a tool for monitoring [domainsquatting][wiki-cybersquatting] for a client. One narrow, but very interesting and nerdy, technique for domainsquatting is bitflipping where indiviual bits get flipped in the domain and an attacker tries to fetch misdirected traffic. This series of blogpost intends to describe the attack vector, how to configure BIND for bitflipping and how to detect it.

### What is bitflipping
So strings are stored in memory and on disk in bits and bytes, ones and zeros, right? A capital `A` is represented as `0100 0001` while a lower case `a` is `0110 0001`.

```ruby
irb(main)> 'A'.ord.to_s(2)
=> "1000001"
irb(main)> 'a'.ord.to_s(2)
=> "1100001"
```

A bitflip occurs when a 0 turns into a 1 or a 1 turns into a 0. So for instance if the sixth digit in a lower case `a`, a zero, turns into a one the new byte instead represents a lower case `e`.

```ruby
irb(main)> "01100001".to_i(2).chr
=> "a"
irb(main)> "01100101".to_i(2).chr
=> "e"
```

A bitflip can occur for a number of reasons; when writing a file to disk, due to overheating in memory or by interference in transit etc. With recent techniques, such as [rowhammer][googlezero-rowhammer] and [FFS][usenix-ffs], it is possible to force a bit error in memory. With the growing occurence of shared resources in the cloud, an attacker can utilize the bit errors for privilege escalation, in-browser exploitation, and selective information disclosure.

### What is bitsquatting
Bitsquatting is the act of registering bitflipped domains in an attempt to abuse the misdirected traffic trickling in to the server, caused by bitflips in the domain name at the client or supporting servers (such as proxies, DNS or Web servers). First introduced by Artem Dinaburg back in 2011 on [DEF CON 19][dinaburg-defcon19], the attack vector has been proven to be [used][bitsquatting_www2013] and [useful][schultz-defcon21] in the wild.

Artem Dinaburg conducted empirical research by registering 32 bitflipped versions of high-traffic domains belonging to Facebook, Microsoft, Google, Amazon, and Akamai. Except for three events that caused significant traffic spikes, an average of 59 request per day trickled in to the server. This was back in 2011 and [according to Wikipedia][wiki-traffic] the global traffic has increased 2.3 times between 2011 and 2015. Dinaburg’s research suggests that mobile devices are particularly susceptible for bitflipping in memory and the mobile traffic has increased 6.2 times since the research.

Jaeson Schultz presented on [DEF CON 21][schultz-defcon21] (and published a [whitepaper][schultz-whitepaper]) two years after Artem and showed that some characters gets bitflipped to special characters in a URL and even more increase the attack surface. Another [research paper][bitsquatting_www2013] proves that the presence of bitsquatting domains increased after Artem Dinaburg presented his findings.

[//]: Do normal disk writes have checksums?
[//]: UDP DNS checksum?


### Tools
There exist a couple of ready-to-use tools to generate bitflipped domains. Here are a couple of them.

#### BitSquat
Created by the original researcher, Artem Dinaburg, this tool is capable of generating bitflipped permutations of a single domain and verify if the domain resolves (translates from domain to IP) or not.

[http://dinaburg.org/data/bitsquat.py][tool-bitsquat]  
[https://github.com/artemdinaburg/bitsquat-script][github-bitsquat]


#### FitBlipper
Similar to BitSquat, but instead of resolving the domain it verifies if the domain is registered or not through a WHOIS lookup. This is more reliable since a domain can be registered but don’t resolve to anything yet, but keep in mind that automated WHOIS lookups against the WHOIS servers are usually not allowed.

[https://github.com/chadillac/FitBlipper][github-fitblipper]

#### bitsquat_rpz.pl
Created by Jaeson Schultz, this tool is able to generate [Response Policy Zones (RPZ)][wiki-rpz] to be loaded in your organizations internal DNS, to protect your users for potentially malicious upstream lookups.

The original URL is dead, but a mirror exist on GitHub: [https://gist.github.com/symm/7856106][github-schultz-mirror]

#### dnstwist
dnstwist is a generic tool capable of generating a wide range of domain permutations, including bitflipped domains. It also verify if the domain is registered or not.

[https://github.com/elceef/dnstwist][github-dnstwist]

#### URLCrazy
Just like dnstwist, this tool is a generic domain generating and resolving tool capable of, among others, checking bitflipped domains. Included in a standard version of Kali.

[https://www.morningstarsecurity.com/research/urlcrazy][tool-urlcrazy]

### References
* [Wikipedia: Cybersquatting][wiki-cybersquatting]
* [Wikipedia: Internet_traffic][wiki-traffic]
* [Wikipedia: Response policy zone][wiki-rpz]
* [Artem Dinaburg: Bitsquatting][dinaburg-bitsquatting]
* [Artem Dinaburg at DEF CON 19 - Bit-squatting: DNS Hijacking Without Exploitation][dinaburg-defcon19]
* [Artem Dinaburg at black hat 2011 - Bit-squatting: DNS Hijacking Without Exploitation][dinaburg-blackhat2011]
* [Jaeson Schultz at DEF CON 21 - Examining the Bitsquatting Attack Surface][schultz-defcon21]
* [Jaeson Schultz "Examining the Bitsquatting Attack Surface" Whitepaper][schultz-whitepaper]
* [Nick Nikiforakis, Steven Van Acker, Wannes Meert, Lieven Desmet, Frank Piessens, Wouter Joosen - Bitsquatting: Exploiting Bit-flips for Fun, or Profit?][bitsquatting_www2013]
* [GitHub: BitSquat][github-bitsquat]
* [GitHub: FitBlipper][github-fitblipper]
* [GitHub: dnstwitst][github-dnstwist]
* [Github: bitsquat_rpz (mirror)][schultz-bitsquat_rpz]
* [Morningstar URLCrazy][tool-urlcrazy]

Cover photo "Old memories" by Csaba J. Szabo, from [FreeImages.com](http://www.freeimages.com/photo/old-memories-1458085)

[wiki-cybersquatting]: https://en.wikipedia.org/wiki/Cybersquatting
[wiki-traffic]: https://en.m.wikipedia.org/wiki/Internet_traffic#Global_Internet_traffic
[wiki-rpz]: https://en.wikipedia.org/wiki/Response_policy_zone
[github-bitsquat]: https://github.com/artemdinaburg/bitsquat-script
[tool-bitsquat]: http://dinaburg.org/data/bitsquat.py
[github-fitblipper]: https://github.com/chadillac/FitBlipper
[github-dnstwist]: https://github.com/elceef/dnstwist
[tool-urlcrazy]: https://www.morningstarsecurity.com/research/urlcrazy
[dinaburg-bitsquatting]: http://dinaburg.org/bitsquatting.html
[dinaburg-defcon19]: https://www.youtube.com/watch?v=lZ8s1JwtNas
[dinaburg-blackhat2011]: https://www.youtube.com/watch?v=_si0FYl_IOA/
[schultz-defcon21]: https://www.youtube.com/watch?v=j2FVFVHVvgg
[schultz-bitsquat_rpz]: http://data.0xfeedcafe.com/bitsquat_rpz.pl
[schultz-whitepaper]: http://blogs.cisco.com/wp-content/uploads/Schultz-Examining_the_Bitsquatting_Attack_Surface-whitepaper.pdf
[github-schultz-mirror]: https://gist.github.com/symm/7856106
[bitsquatting_www2013]: https://securitee.org/files/bitsquatting_www2013.pdf
[wikipedia-rowhammer]: https://en.wikipedia.org/wiki/Row_hammer
[googlezero-rowhammer]: https://googleprojectzero.blogspot.co.uk/2015/03/exploiting-dram-rowhammer-bug-to-gain.html
[usenix-ffs]: https://www.usenix.org/system/files/conference/usenixsecurity16/sec16_paper_razavi.pdf
