---
layout: content
title:  "Overview"
categories: tcpreplay wiki
---

Tcpreplay is a suite of [GPLv3] licensed utilities for UNIX (and Win32 under
[Cygwin]) operating systems for editing and replaying network traffic which
was previously captured by tools like [tcpdump] and [Ethereal]/[Wireshark]. 
It allows you to classify traffic as client or server, rewrite Layer 2, 3 and 4 
packets and finally replay the traffic back onto the network and through other
devices such as switches, routers, firewalls, NIDS and IPS's. Tcpreplay supports
both single and dual NIC modes for testing both sniffing and in-line devices.

Tcpreplay is used by numerous firewall, IDS, IPS, NetFlow and other networking
vendors, enterprises, universities, labs and open source projects. If your
organization uses Tcpreplay, please let us know who you are and what you use
it for so that I can continue to add features which are useful.

Tcpreplay is designed to work with network hardware and normally does not
penetrate deeper than Layer 2. Yazan Siam with sponsorship from [Cisco] developed
*tcpliveplay* to replay TCP pcap files directly to servers. Use this utility
if you want to test the entire network stack and into the application.

As of version 4.0, Tcpreplay has been enhanced to address the complexities of
testing and tuning [IP Flow][flow]/[NetFlow] hardware. Enhancements include:

* Support for [netmap] modified network drivers for 10GigE wire-speed performance
* Increased accuracy for playback speed
* Increased accuracy of results reporting
* Flow statistics including Flows Per Second (fps)
* Flow analysis for analysis and fine tuning of flow expiry timeouts
* Hundreds of thousands of flows per second (dependent flow sizes in pcap file) 

Version 4.0 is the first version delivered by Fred Klassen and sponsored by 
[AppNeta]. Many thanks to the author of Tcpreplay, Aaron Turner who has supplied
the world with a a solid and full-featured test product thus far. The new author
strives to take Tcprelay performance to levels normally only seen in commercial
network test equipment.

## Links

* [Download and installation instructions][installation]
* [Support]
* [Tcpreplay Users Mailing List][maillist]
* #tcpreplay on [Twitter]
* [Live chat]
* [FAQ]
* [How To]
* [The history of Tcpreplay][history]
* Legacy
    * [wiki][legacywiki]
    * [Tcpreplay Blog][legacyblog]
    * [RSS Feed][rss]
    * [Podcasts]
    


[GPLv3]:    http://www.gnu.org/licenses/gpl-3.0.html
[netmap]:   http://info.iet.unipi.it/~luigi/netmap/
[flow]:     http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
[NetFlow]:  http://www.cisco.com/go/netflow
[Cygwin]:   http://www.cygwin.com/
[Wireshark]: http://www.wireshark.org
[tcpdump]:  http://www.tcpdump.org
[Ethereal]: http://www.ethreal.com
[Cisco]:    http://www.cisco.com
[AppNeta]:  http://www.appneta.com
[maillist]: https://lists.sourceforge.net/lists/listinfo/tcpreplay-users
[legacyblog]:  http://synfin.net/sock_stream/tag/tcpreplay
[legacywiki]:  http://tcpreplay.synfin.net/
[rss]:        http://synfin.net/sock_stream/tag/tcpreplay/rss
[Twitter]:    http://twitter.com
[Podcasts]:   http://tcpreplay.synfin.net/tcprecast/
[history]:        history.html
[installation]:   installation.html
[Live chat]:      chat.html
[Support]:        support.html
[FAQ]:            faq.html
[How To]:         howto.html
