---
layout: post
title:  "Configuring RubyDNS for bitflipping"
description: "Part two of four"
date:   2016-10-02 16:35:58 +0100
categories: DNS Configuring RubyDNS Bitsquatting Bitflupping Cybersquatting Typosquatting
---
### Set up RubyDNS
Start with cloning RubyDNS locally
```shell
axl@axl:~/$ git clone https://github.com/ioquatix/rubydns.git
axl@axl:~/$ cd rubydns
axl@axl:~/rubydns/$ gem install rubydns
axl@axl:~/rubydns/$ bundle
axl@axl:~/rubydns/$ cd examples
axl@axl:~/rubydns/examples/$ ruby ./axl-lab.rb start
Starting FlakeyDNS daemon...
Daemon status: running pid=17878
```

```shell
sudo tcpdump -i lo udp port 5300 -w tcpdump1
sudo tcpdump -X -r tcpdump1
```

```shell
axl@axl:~/$ sudo tcpdump -i lo udp port 5300 -X
tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
19:55:28.358478 IP localhost.36255 > localhost.5300: UDP, length 36
	0x0000:  4500 0040 cb53 0000 4011 b157 7f00 0001  E..@.S..@..W....
	0x0010:  7f00 0001 8d9f 14b4 002c fe3f 219a 0120  .........,.?!...
	0x0020:  0001 0000 0000 0001 0361 786c 036c 6162  .........axl.lab
	0x0030:  0000 0100 0100 0029 1000 0000 0000 0000  .......)........
19:55:28.360218 IP localhost.5300 > localhost.36255: UDP, length 41
	0x0000:  4500 0045 cb54 4000 4011 7151 7f00 0001  E..E.T@.@.qQ....
	0x0010:  7f00 0001 14b4 8d9f 0031 fe44 219a 8500  .........1.D!...
	0x0020:  0001 0001 0000 0000 0361 786c 036c 6162  .........axl.lab
	0x0030:  0000 0100 01c0 0c00 0100 0100 0151 8000  .............Q..
	0x0040:  040a 6178 6c                             ..axl
```

### References
* 
* [Artem Dinaburg at DEF CON 19 - Bit-squatting: DNS Hijacking Without Exploitation][dinaburg-defcon19]

[dinaburg-defcon19]: https://www.youtube.com/watch?v=lZ8s1JwtNas&feature=youtu.be&t=1250
