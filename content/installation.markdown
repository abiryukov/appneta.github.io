---
layout: content
title:  "Installation"
categories: tcpreplay content
---

<br />

---
Simple directions for Unix users: 
--------------------------------- 
[Download] the release tarball, extract and then...

```
./configure 
make
sudo make install
```

<br \>

---
Build netmap feature
--------------------
This feature will detect [netmap][nm]
capable network drivers on Linux and BSD 
systems. If detected, the network driver is bypassed for the execution 
duration of tcpreplay and tcpreplay-edit, and network buffers will be 
written to directly. This will allow you to achieve full line rates on 
commodity network adapters, similar to rates achieved by commercial network 
traffic generators.

**Note** that bypassing the network driver will disrupt other applications connected
through the test interface. Don't test on the same interface you ssh'ed into.

Download latest and install netmap from <http://info.iet.unipi.it/~luigi/netmap/>
If you extracted netmap into */usr/src/* or */usr/local/src* you can build normally. Otherwise you 
will have to specify the netmap source directory, for example:

```
./configure --with-netmap=/home/fklassen/git/netmap
make
sudo make install
```

[gplv3]:    http://www.gnu.org/licenses/gpl-3.0.html
[nm]:       http://info.iet.unipi.it/~luigi/netmap/
[flow]:     http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
[Download]: download.html
