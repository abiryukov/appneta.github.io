---
layout: content
title:  "Overview"
categories: tcpreplay content
---

Tcpreplay is a suite of [GPLv3][gplv3] licensed tools for UNIX (and Win32 under  Cygwin) operating systems which gives 
you the ability to use previously captured traffic in  libpcap format to test a variety of network devices. It allows
you to classify traffic as client or server, rewrite Layer 2, 3 and 4 headers and finally replay the traffic back onto
the network and through other devices such as switches, routers, firewalls, NIDS and IPS's. Tcpreplay supports both single 
and dual NIC modes for testing both sniffing and in-line devices.

Tcpreplay is used by numerous firewall, IDS, IPS, IP Flow/NetFlow and other networking vendors, enterprises, universities,
labs and open source projects. If your organization uses Tcpreplay, please let me know who you are and what you use
it for so that I can continue to add features which are useful.

Version 4.0.0 is the first version delivered by Fred Klassen of AppNeta Inc. Many thanks to the author of Tcpreplay,
Aaron Turner who has supplied the world with a a solid and full-featured test product thus far. The new author
strives to take Tcprelay performance to levels normally only seen in commercial network test equipment.

[gplv3]:    http://www.gnu.org/licenses/gpl-3.0.html
[nm]:       http://info.iet.unipi.it/~luigi/netmap/
[flow]:     http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
