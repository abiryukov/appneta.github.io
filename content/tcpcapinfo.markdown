---
layout: content
title:  "tcpcapinfo"
categories: tcpreplay content
---


<br />

- [Overview](#overview)
- [Basic Usage](#basic-usage)
- [Example](#example)

<br />

---
<h2><a name="overview"></a>Overview</h2>
*tcpcapinfo was born out of a need for me to diagnose tcprewrite bugs and broken pcap files. 
Honestly, it's usefulness is probably limited only to people who code applications which 
read/write pcap files, but I include it with the Tcpreplay Suite for completeness. 
*tcpcapinfo* was first released in version 3.4.5.

---
<h2><a name="basic-usage"></a>Basic Usage</h2>

```
$ tcpcapinfo file.pcap
```

Will process file.pcap and report information about the libpcap packet/file headers, 
a packet checksum and some basic sanity checks such as if the packet is too 
large given the pcap file's snaplen header value or if the timestamp goes backwards in time. 
Note that the "packet checksum" is not the same thing as the IP checksum, 
and is intended to provide a means if two Ethernet frames are the same.

---
<h2><a name="example"></a>Example</h2>

```
$ tcpcapinfo smallFlows.pcap | head -n 30
file size   = 9444731 bytes
magic       = 0xa1b2c3d4 (tcpdump) (little-endian)
version     = 2.4
thiszone    = 0x00000000
sigfigs     = 0x00000000
snaplen     = 65535
linktype    = 0x00000001
Packet	OrigLen		Caplen		Timestamp	Csum	Note
1	 997		 997		4d3f1be6.76439	a413bd	OK
2	 440		 440		4d3f1be6.7d8ca	504113	OK
3	  66		  66		4d3f1be6.acec4	8db5c	OK
4	  54		  54		4d3f1be6.ae468	9c35b	OK
5	  66		  66		4d3f1be6.b1812	8db5c	OK
6	  54		  54		4d3f1be6.b1841	8e75c	OK
7	 998		 998		4d3f1be6.b19a3	a036c1	OK
8	  54		  54		4d3f1be6.b677e	8e75c	OK
9	  60		  60		4d3f1be6.b6bc3	7e75d	OK
10	 541		 541		4d3f1be6.b9cf8	65fffd	OK
11	  54		  54		4d3f1be6.b9d50	7e75d	OK
12	  60		  60		4d3f1be6.bb339	7e75d	OK
13	  66		  66		4d3f1be6.e2980	9ae5b	OK
14	  66		  66		4d3f1be6.e7223	bae59	OK
15	  54		  54		4d3f1be6.e727c	aba5a	OK
16	 231		 231		4d3f1be6.e7577	200944	OK
17	  60		  60		4d3f1be6.ebe8b	aba5a	OK
18	1484		1484		4d3f1be6.ec419	e92376	OK
19	 345		 345		4d3f1be6.ec752	3b9728	OK
20	  54		  54		4d3f1be6.ec76f	9ba5b	OK
21	 329		 329		4d3f1be6.ed5a1	48a71b	OK
22	 280		 280		4d3f1be6.f27e8	3ad829	OK
```
