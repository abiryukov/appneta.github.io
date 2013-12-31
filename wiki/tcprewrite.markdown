---
layout: content
title:  "tcprewrite"
categories: tcpreplay content
---

- [Overview](#overview)
- [Basic Usage](#basic-usage)
	- [Direction & Selection](#direction-&-selection)
- [Rewriting Layer 2](#rewriting-layer-2)
	- [DLT Plugins](#dlt-plugins)
	- [DLT_EN10MB (Ethernet)](#dlt_en10mb-ethernet)
		- [Rewriting Source & Destination MAC addresses](#rewriting-source-&-destination-mac-addresses)
		- [802.1q VLAN's](#8021q-vlan's)
	- [DLT_CHDLC (Cisco HDLC)](#dlt_chdlc-cisco-hdlc)
	- [DLT_USER0 (User Defined)](#dlt_user0-user-defined)
- [Rewriting Layer 3](#rewriting-layer-3)
	- [Forcing Traffic Between Two Hosts](#forcing-traffic-between-two-hosts)
	- [Randomizing IP Addresses](#randomizing-ip-addresses)
	- [Changing Networks via Pseudo-NAT, Source/Destination IP Map](#changing-networks-via-pseudo-nat-sourcedestination-ip-map)
	- [Editing IPv4 TOS/DiffServ/ECN](#editing-ipv4-tosdiffservecn)
	- [Editing IPv6 Traffic Class](#editing-ipv6-traffic-class)
	- [Editing IPv6 Flow Label](#editing-ipv6-flow-label)
- [Rewriting Layer 4](#rewriting-layer-4)
	- [Re-Mapping Ports](#re-mapping-ports)
	- [Forcing Checksum Calculation](#forcing-checksum-calculation)
- [Rewriting Layers 5-7](#rewriting-layers-5-7)
- [Dealing with MTU problems](#dealing-with-mtu-problems)
- [Fragroute](#fragroute)
	- [Overview](#overview2)
	- [Usage](#usage)

<h2><a name="overview"></a>Overview</h2>
In version 3.0, all of the packet editing functionality in [tcpreplay] was moved
to [tcprewrite]. In 3.4.1 this editing functionality was re-introduced in *tcpreplay*
with the creation of [tcpreplay-edit]. Hence, all the options listed below are valid
for both *tcprewrite* and *tcpreplay-edit*.

<h2><a name="basic-usage"></a>Basic Usage</h2>
Running *tcprewrite* requires you to provide it an input pcap file and the name 
of the output pcap file (which will be overwritten).

```
$ tcprewrite --infile=input.pcap --outfile=output.pcap
```

Additional arguments for actually editing packets are described below.

<h3><a name="direction-&-selection"></a>Direction & Selection</h3>
Before we get to packet editing, it is important to remember that some of these rewrite 
options allow you to edit packets differently depending on the direction 
of the packet. Packet direction is determined by consulting a *tcpprep cache file*, 
which allows you to define direction based on a variety critera.

Using *tcpprep cache files*, you can also mark packets as to be skipped during processing 
by *tcprewrite*. Use of this feature allows you to select which packets are edited and which
packets are not. Since cache files are seperate from the actual pcap, you can use multiple
cache files with different processing rules for multiple passes of *tcprewrite*.

To specify a *tcpprep cache file* to use during processing, use the `--cachefile` option.

<h2><a name="rewriting-layer-2"></a>Rewriting Layer 2</h2>
*tcprewrite* supports a lot of Layer 2 rewriting options to help you modify packets so
that traffic can flow through switches, firewalls, routers, IPS's and many other
forwarding devices.

<h3><a name="dlt-plugins"></a>DLT Plugins</h3>
As of 3.0, tcprewrite uses plugins to support different DLT/Layer 2 types.
This not only makes the code easier to maintain, but also helps make things clearer
for users regarding what is and isn't supported. Each plugin may support reading
and/or writing packets. By default, the plugin used to read packets is also used
for output, but you can override the output plugin using the `--dlt` option. Changing the
DLT plugin allows you to convert the packets from one DLT/Layer 2 type to another type.
This allows you, for example, to capture traffic on say an Ethernet interface and replay
over Cisco HDLC or capture on a BSD Loopback interface and replay over Ethernet.

Plugins supported in output mode:

* Ethernet (enet)
* Cisco HDLC (hdlc)
* User defined Layer 2 (user)

Plugins supported in input mode:

* Ethernet
* Cisco HDLC
* Linux SLL
* BSD Loopback
* BSD Null
* Raw IP
* 802.11
* Juniper Ethernet (version >= 4.0)

Hence, if you have a pcap in one of the supported input DLT types, you can convert it
to one of the supported output DLT type by using the `--dlt=<output>` option. Depending
on the input DLT you may need to provide additional DLT plugin flags.

<h3><a name="dlt_en10mb-ethernet"></a>DLT_EN10MB (Ethernet)</h3>
The Ethernet plugin allows you to control the source and destination MAC addresses.
Additionaly, you can add, remove and edit 802.1q VLAN tag headers.

<h4><a name="rewriting-source-&-destination-mac-addresses"></a>Rewriting Source & Destination MAC addresses</h4>
The most common layer 2 rewriting need is to change the source and destination 
MAC addresses of packets so that they will be processed by the correct device.
By using the `--enet-dmac` and `--enet-smac` options you can specify what the new
destination and source MAC addresses should be respectively.

The following would cause all traffic to have a destination MAC
of 00:55:22:AF:C6:37 and a source MAC of 00:44:66:FC:29:AF:

```
$ tcprewrite --enet-dmac=00:55:22:AF:C6:37 --enet-smac=00:44:66:FC:29:AF --infile=input.pcap --outfile=output.pcap
```

Now, that's probably not very useful unless all your traffic is unidirectional. 
So what if you have bi-directional traffic that you want to send through a router
who's MAC addresses are 00:55:22:AF:C6:37 and 00:44:66:FC:29:AF? We'll assume the client is
00:22:55:AC:DE:AC and the server is 00:66:AA:D1:32:C2. 
First you would need a *tcpprep cache file* which splits the traffic. Once you have that, 
you would run* tcprewrite* like this:

```
$ tcprewrite --enet-dmac=00:44:66:FC:29:AF,00:55:22:AF:C6:37 --enet-smac=00:66:AA:D1:32:C2,00:22:55:AC:DE:AC --cachefile=input.cache --infile=input.pcap --outfile=output.pcap
```

The important thing above is to remember that the first MAC addresses listed for 
the dmac/smac flag is for server traffic and the second addresses are for client traffic.

One very useful flag to keep in mind is `--skipbroadcast` which causes tcprewrite to skip 
rewriting MAC addresses which are broadcast (FF:FF:FF:FF:FF:FF) or 
multicast (first octet is odd). 
Rewriting broadcast/multicast MAC address break things like ARP and DHCP.

<h4><a name="8021q-vlan's"></a>802.1q VLAN's</h4>

*tcprewrite* also allows you to add or remove 802.1q VLAN tag information from
ethernet frames. Removing the 802.1q tag information is as simple as specifying `--vlan=del`:

```
$ tcprewrite --enet-vlan=del --infile=input.pcap --outfile=output.pcap
```

You can also take non-tagged frames and make them tagged by using `--enet-vlan=add` and one or 
more of the following `--enet-vlan-tag`, `--enet-vlan-cfi`, `--enet-vlan-pri`

```
$ tcprewrite --enet-vlan=add --enet-vlan-tag=40 --enet-vlan-cfi=1 --enet-vlan-pri=4 --infile=input.pcap --outfile=output.pcap
```

will set the VLAN tag to be 40, the CFI value to 1 and a VLAN priority of 4.

<h3><a name="dlt_chdlc-cisco-hdlc"></a>DLT_CHDLC (Cisco HDLC)</h3>

Cisco HDLC has two fields in the Layer 2 header: address and control. 
Both can be set using this plugin:

* `--hdlc-address`
* `--hdlc-control`

<h3><a name="dlt_user0-user-defined"></a>DLT_USER0 (User Defined)</h3>

The user defined DLT option allows you to create any DLT/Layer2 header 
of your choosing by using the following two options:

* `--user-dlt` - Set pcap DLT type
* `--user-dlink` - Set packet layer 2 header

<h2><a name="rewriting-layer-3"></a>Rewriting Layer 3</h2>
As of version 3.4.2, *tcprewrite* supports both IPv4 and IPv6 addresses. There are a number
of methods for rewriting IP addresses depending on your needs. When enabling a 
layer 3 rewrite rule, *tcprewrite* will automagically re-calculate checksums for you, 
so there is no need to pass `--fixcsum`. When specifying IPv6 addresses,
wrap the address in hard brackets like so: [2001::dead:beef] or for networks: [2001::/16]

Not only do the following options edit the IP header, but in the case of IPv4,
also modified ARP requests/replies to match as well.


<h3><a name="forcing-traffic-between-two-hosts"></a>Forcing Traffic Between Two Hosts</h3>
Sometimes you have a pcap with a bunch of hosts and you want rewrite all the traffic to be 
between two hosts or "endpoints". You can choose the IP addresses 
(like 10.10.1.1 and 10.10.1.2) for these two hosts by using the `--endpoints` rule:

```
$ tcprewrite --endpoints=10.10.1.1:10.10.1.2 --cachefile=input.cache --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

Note that `--endpoints` requires a cache file and use of `--skipbroadcast` is highly recommended.


<h3><a name="randomizing-ip-addresses"></a>Randomizing IP Addresses</h3>
If you have a pcap that you want to give someone else without revealing 
your IP addresses, then this may be what you're looking for. 
Note that this feature only handles IP headers and ARP messages; it does not
modify application data which may contain your original IP address as well.

When IP addresses are randomized, it is done in a deterministic manner,
based on the seed value you provide, so that sessions between
two hosts are maintained. Using different seed values results
in different values for the IP addresses for the same input pcap.

```
$ tcprewrite --seed=423 --infile=input.pcap --outfile=output.pcap
```

<h3><a name="changing-networks-via-pseudo-nat-sourcedestination-ip-map"></a>Changing Networks via Pseudo-NAT, Source/Destination IP Map</h3>
Pseudo-NAT works very much like network address translation. 
It allows you to map IP addresses in one subnet to IP addresses in another subnet.
Each source and destination subnet is expressed in CIDR notation, and needn't be the
same size. You can specify multiple CIDR pairs and use the `--pnat` flag twice if you
use a cache file. The format is: `<match_cidr>:<rewrite_cidr>,...`

```
$ tcprewrite --pnat=10.0.0.0/8:172.16.0.0/12,192.168.0.0/16:172.16.0.0/12 --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

would rewrite any IP in either 10.0.0.0/8 or 192.168.0.0/16 to be in the 172.16.0.0/12 
subnet. You could also rewrite IP's differently depending on the direction of the packet:

```
$ tcprewrite --pnat=10.0.0.0/8:192.168.0.0/24 --pnat=10.0.0.0/8:192.168.1.0/24 --cachefile=input.cache --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

Would cause traffic in 10.0.0.0/8 to be remapped to different subnets depending 
on the classification of the node as client or server. The result is that both source 
and destination IP's will be remapped properly to maintain the session.

Alternatively to the `--pnat` option you can use `--srcipmap` and/or `--dstipmap` 
to apply different rules to the source and destination IP addresses in packets. 
`--srcipmap` and `--dstipmap` work just like `--pnat` and use the same 
`<match_cidr>:<rewrite_cidr>,...` format.

<h3><a name="editing-ipv4-tosdiffservecn"></a>Editing IPv4 TOS/DiffServ/ECN</h3>
To change the TOS (now known as DiffServ/ECN) use the --tos flag to specify the new value.

```
$ tcprewrite --tos=50 --infile=input.pcap --outfile=output.pcap
```

Would set the TOS byte in every IPv4 header to 50.

<h3><a name="editing-ipv6-traffic-class"></a>Editing IPv6 Traffic Class</h3>

To change the Traffic Class field use the `--tclass` flag to specify the new value.

```
$ tcprewrite --tclass=33 --infile=input.pcap --outfile=output.pcap
```

Would set the Traffic Class field in every IPv6 header to 33.

<h3><a name="editing-ipv6-flow-label"></a>Editing IPv6 Flow Label</h3>

To change the Flow Label field use the `--flowlabel` flag to specify the new value.

```
$ tcprewrite --flowlabel=67234 --infile=input.pcap --outfile=output.pcap
```

Would set the Flow Label field in every IPv6 header to 67234.

<h2><a name="rewriting-layer-4"></a>Rewriting Layer 4</h2>
*tcprewrite* also supports some limited TCP/UDP editing. Whenever you edit the layer 4
data of a packet, tcprewrite will automatically recalculate the appropriate checksums.

<h3><a name="re-mapping-ports"></a>Re-Mapping Ports</h3>
Using tcprewrite, you can remap a TCP or UDP session from one port to another.
One example may be to change all the HTTP traffic to run over port 8080 instead of 80. 
To remap a port, use the `--portmap` flag.

```
$ tcprewrite --portmap=80:8080,22:8022 --infile=input.pcap --outfile=output.pcap
```

would re-map TCP/UDP traffic running on 80 to be 8080 and traffic on port 22 to port 8022.

<h3><a name="forcing-checksum-calculation"></a>Forcing Checksum Calculation</h3>
Many network cards support TCP/UDP/IP checksum offloading, so 
if you capture traffic which was generated by the same system, the checksums will be incorrect.
This can obviously cause problems later on when you try replaying the traffic.
By using the `--fixcsum` flag, you can force *tcprewrite* to fix the checksums.
Note, *tcprewrite* will automatically fix checksums when editing packets.

```
$ tcprewrite --fixcsum --infile=input.pcap --outfile=output.pcap
```

<h2><a name="rewriting-layers-5-7"></a>Rewriting Layers 5-7</h2>
Often pcap's are truncated so some of the application data in the packet is missing.
Depending on the device type that will be processing the traffic, the application 
data may or may not be important, but having a full packet may be. Routers and 
firewalls for example don't usually fully process application data.

*tcprewrite* supports three methods to "fix" the missing data. You can either pad out the
packet with 0x00 or alter the packet headers to indicate that the packet length is only as
large as what was captured. In both cases, the packet data is most likely invalid,
but at least the packet is valid. The third option is to remove the packet completely.
Note that by removing the packet from the output pcap file, hence you should not re-use 
a *tcpprep cache file* with the resulting file since it will no longer match the pcap file.

Pad the packets with 0x00:

```
$ tcprewrite --fixlen=pad --infile=input.pcap --outfile=output.pcap
```

Rewrite the packet header length fields to match the captured packet length:

```
$ tcprewrite --fixlen=trunc --infile=input.pcap --outfile=output.pcap
```

Delete the packet from the pcap file:

```
$ tcprewrite --fixlen=del --infile=input.pcap --outfile=output.pcap
```

When padding packets, their maximum size will be limited to the MTU value 
(default is 1500 bytes) which can be over-ridden using the `--mtu` option.

<h2><a name="dealing-with-mtu-problems"></a>Dealing with MTU problems</h2>
Sometimes the maximum size of a frame you can send on an interface (MTU) is smaller 
then some packets you need to send. Normally, *tcpreplay* will skip these packets 
completely, but you have a few other options:

1, Truncate packets to the MTU length (default 1500 bytes):

```
$ tcprewrite --mtu-trunc --infile=input.pcap --outfile=output.pcap
```

2, Truncate packets to a user defined MTU (1000 bytes):

```
$ tcprewrite --mtu=1000 --mtu-trunc --infile=input.pcap --outfile=output.pcap
```

3. Use IP fragmentation to break up large packets into smaller ones. As of v3.3.0 you can use the fragroute engine to fragment IP packets into smaller pieces to fit within the MTU. The way to do this is to create a text file frag.cfg with the contents
```
ip_frag 1400
```

and run tcprewrite like so:

```
$ tcprewrite --fragroute=frag.cfg --infile=input.pcap --outfile=output.pcap
```
This will cause tcprwrite to fragment any packet into 1400 byte chunks. Since IP fragmentation is done at the IP layer, we use a value smaller then the MTU (in this case assuming 1500 for ethernet) to make sure we have enough room for the ethernet and IPv4 headers. Of course, this won't help any non-IP frames, so you may have some packets which can't be sent in some situations.

<h2><a name="fragroute"></a>Fragroute</h2>

<h3><a name="overview2"></a>Overview</h3>
As of Tcpreplay 3.3.0, tcprewrite integrates  Dug Song's fragroute engine. 
Due to library constraints fragroute may or may not enabled in your binary. 
Running `tcprewrite -V` will tell you. Due to limitations of the fragroute code, 
currently it only supports non-VLAN tagged ethernet frames (DLT_EN10MB).

*tcprewrite* uses the exact same configuration file syntax as fragroute, 
but doesn't support a few of the commands:

* delay
* echo
* print

Newer versions of Tcpreplay (3.4.2 and above) added support for IPv6 fragroute with
the following options:

* ip6_opt [route <segments> <ip6-addr> ...] | [raw <type> <byte stream>]
* ip6_qos <traffic-class> <flow-label>

<h3><a name="usage"></a>Usage</h3>

Basic usage to fragment all packets in a pcap file:

```
$ tcprewrite --fragroute=frag.conf --infile=input.pcap --outfile=output.pcap
```

If you have a *tcpprep cache file* and with to only fragroute packets on direction 
(client-to-server or server-to-client) then use the `--fragdir` option like so:

```
$ tcprewrite --fragroute=frag.conf --fragdir=c2s --cachefile=input.cache --infile=input.pcap --outfile=output.pcap
```

The fragroute engine is applied last, so any other tcprewrite options will be applied first.

[tcpreplay-edit]:      tcpreplay-edit.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html

