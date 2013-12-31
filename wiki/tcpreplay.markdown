---
layout: content
title:  "tcpreplay"
categories: tcpreplay wiki
---

- [Overview](#overview)
- [Basic usage](#basic-usage)
- [Examples](#examples)
- [Advanced Usage](#advanced-usage)
	- [Testing Routers or Switches](#testing-routers-or-switches)

<h2><a name="overview"></a>Overview</h2>

*tcpreplay* has evolved quite a bit over the years. In the 1.x days, it merely read packets and sent 
then back on the wire. In 2.x, tcpreplay was enhanced significantly to add various rewriting 
functionality but at the cost of complexity, performance and bloat. 
In 3.x, tcpreplay has returned to its roots to be a lean packet sending machine 
and the editing functions have moved to [tcprewrite][] and a powerful [tcpreplay-edit][] 
which combines the two.

In 4.x [IP FLow][flow] and [netmap][nm] features where added. Minor edit capabilities 
were added, but not at the cost of 
performance. Essentially tcpreplay is intended to be fast, and all options are designed to 
work at wire rates. Options that may affect performance such as run-time packet editing have
been moved to [tcpreplay-edit][].

<h2><a name="basic-usage"></a>Basic usage</h2>

To replay a given pcap as it was captured all you need to do is specify the
pcap file and the interface to send the traffic out interface `eth0`:

```
# tcpreplay -i eth0 sample.pcap
```

<h2><a name="examples"></a>Examples</h2>
The following examples use one of provided [sample captures][captures] on an i7 processors with
multi-port Intel 82599 10GigE adapters.

By default the pcap file is played back at the same rate that it was captured at.
If you prefer to play back at a specific speed, add the `--mbps` option.

```
# tcpreplay -i eth0 --mbps=510.5 smallFlows.pcap 
Actual: 14261 packets (9216531 bytes) sent in 0.144495 seconds.
Rated: 63784428.5 Bps, 510.27 Mbps, 98695.45 pps
Flows: 1209 flows, 8367.07 fps, 14243 flow packets, 18 non-flow
Statistics for network device: eth0
	Attempted packets:         14261
	Successful packets:        14261
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

To replay as with zero time between packets:

```
# tcpreplay -i eth0 --topspeed smallFlows.pcap 
Actual: 14261 packets (9216531 bytes) sent in 0.012960 seconds.
Rated: 711152083.3 Bps, 5689.21 Mbps, 1100385.80 pps
Flows: 1209 flows, 93287.03 fps, 14243 flow packets, 18 non-flow
Statistics for network device: eth0
	Attempted packets:         14261
	Successful packets:        14261
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

Now notice that you can substitute `-t` for `--topspeed`. You could also
get the same result with `--mbps=0`.  Repeat the sending of the pcap file 
with the `--loop` option and notice that performance increases somewhat.

```
# tcpreplay -i eth0 -t --loop=1000 smallFlows.pcap 
Actual: 14261000 packets (9216531000 bytes) sent in 10.06 seconds.
Rated: 864516674.2 Bps, 6916.13 Mbps, 1337691.18 pps
Flows: 1209 flows, 113.40 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

Now let's see a performance boost with the -K option, which will
cause packets to be read from memory instead of disk. You may want
to consider using this option whenever there is enough memory
available for the pcap file.

```
# tcpreplay -i eth0 -t -K --loop=1000 smallFlows.pcap 
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 8.02 seconds.
Rated: 1114523106.8 Bps, 8916.18 Mbps, 1724533.23 pps
Flows: 1209 flows, 146.20 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

<h2><a name="advanced-usage"></a>Advanced Usage</h2>

To obtain near wire rate you need to compile and install [netmap]nm]
network drivers. This will bypass the network driver for the duration 
of the test and allow *tcpreplay* to write to the network hardware direclty.
Note that the network stack will not be operational on the selected interface
while the test is running.

```
# tcpreplay -i eth0 -tK -l1000 --netmap smallFlows.pcap 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 7.07 seconds.
Rated: 1193506409.4 Bps, 9548.05 Mbps, 1846746.34 pps
Flows: 1209 flows, 156.56 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

To increase the *flows per second (fps)* you need to ensure that
the IP addreses are unique for every loop iteration by specifying the
`--unique-ip` option.

```
# tcpreplay -i eth0 -tK -l1000 --netmap --unique-ip smallFlows.pcap  
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 7.07 seconds.
Rated: 1193507027.6 Bps, 9548.05 Mbps, 1846747.29 pps
Flows: 1209000 flows, 156561.07 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

If you need to control the flows per second you can do so by
trying different values of `--mbps` (or `-M`) option:

```
# tcpreplay -i eth0 -K -l1000 -M9000 --netmap --unique-ip smallFlows.pcap 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 8.01 seconds.
Rated: 1124999450.7 Bps, 8999.99 Mbps, 1740743.57 pps
Flows: 1209000 flows, 147574.43 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

<h3><a name="testing-routers-or-switches"></a>Testing Routers or Switches</h3>
On your *tcpreplay* device you will need to use two network adapters attached
to input and output ports of the device under test (DUT). See [tcpprep][] help 
for examples that illustrate how to classify traffic. For example you can classify packets inside
a pcap file as either client or server. Or you could classify private vs. public.

The result of *tcprep* is a cache file which allows *tcpreplay* to send traffic through 
a device in two directions through a device, and thereby maintaining state. 

```
# tcpreplay --cachefile=sample.prep --intf1=eth0 --intf2=eth1 sample.pcap
```

Alternatively, if you have already split your traffic into two files, as in the case 
of capturing traffic using a network tap, then tcpreplay can read two files at the 
same time, one for each interface:

```
# tcpreplay --dualfile --intf1=eth0 --intf2=eth1 side-a.pcap side-b.pcap
```


[tcprewrite]:          tcprewrite.html
[tcpreplay-edit]:      tcpreplay-edit.html
[tcpprep]:             tcpprep.html
[nm]:                  http://info.iet.unipi.it/~luigi/netmap/
[captures]:            captures.html
