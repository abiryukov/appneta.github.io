---
layout: content
title:  "Sample Captures"
categories: tcpreplay wiki
---

Tcpreplay Suite can read nearly any packet capture file. If you prefer to get up and
running quickly, we have provided some sample captures. They are specially
designed to test [IP Flow][flow] (also called NetFlow), but they are also useful for
testing performance of switches and network adapters.

Pcap files are available [here][pcaps].

## [smallFlows.pcap][small]
This is a synthetic capture which is a combination of many captures. It contains a wide
variety of applications and is designed to demonstrate many types of flows across a network.
If you want to have many flows in a small file and are not concerned about how realistic
the combination of flows is, select this capture

Size:			9.4 MB   
Packets:		14261   
Flows:			1209   
Average packet size:	646 bytes   
Duration:		5 minutes   
Number Applications:    0


## [bigFlows.pcap][big]
This is a capture of real network traffic on a busy private network's access point to the Internet.
The capture is much larger and has a smaller average packet size, than the previous capture.
It also has many more flows. If the large size
of this file isn't a problem, you may want to select it for your tests.   


Size:			368 MB   
Packets:		791615    
Flows:			40686  
Average packet size:	449 bytes   
Duration:		5 minutes   
Number Applications:    0


## Contributions
If you have other good examples of pcap files that work well with Tcpreplay please contact the
maintainer.

[flow]:     http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
[pcaps]:    https://googledrive.com/host/0Bwy1iN_ElthJb3RMb0tGSFY0V28/
[small]:    https://drive.google.com/file/d/0Bwy1iN_ElthJOFdzdXg5NkpMWVU
[big]:      https://drive.google.com/file/d/0Bwy1iN_ElthJcnhnd2l0RUJlY2c
