---
layout: content
title:  "tcpliveplay"
categories: tcpreplay content
---

<br />

- [Overview](#overview)
- [Design Overview](#design-overview)
	- [Successful vs. Unsuccessful Replays?](#successful-vs-unsuccessful-replays)
- [Usage](#usage)
- [Supported OS's](#supported-os's)
- [Fresh Install Guide](#fresh-install-guide)
- [Examples of Successful Run](#examples-of-successful-run)
- [Example of Unsuccessful Run](#example-of-unsuccessful-run)
- [Credits & Thanks](#credits-&-thanks)


*Author & Publisher: Yazan Siam*   
*Email: tcpliveplay at gmail.com*    
*Updated: Dec. 28, 2013*   

---
<h2><a name="overview"></a>Overview</h2>

This tool, *tcpliveplay* formerly called ‘New Conn’, was initially started as a project
for North Carolina State University sponsored by Cisco Systems in order to
extend the functionality of the current Tcpreplay suite. I was very encouraged and
inspired by the need of protecting vulnerable devices through doing live vulnerability
testing against various devices using previously captured TCP packets.
This is because a certain network device may experience a crash due to a
specific TCP packet sequence. So I provide this tool as a solution for those
who wish to protect their network against such vulnerabilities. It is able to replay
TCP captured packets using new TCP connections against any device the user chooses.

---
<h2><a name="design-overview"></a>Design Overview</h2>

*tcpliveplay* is able to replay only TCP packets. The tool is only able to replay 
packet captures that contain one TCP connection. Based on the user input, only 
layer 2 & Layer 3 of the packets will be modified during the replay process. For example, 
if there is HTTP in the capture, it will be pushed to the remote host ‘as is’. The replay 
will only proceed if the remote host meets the expectations of the local host which are 
based on the captured packets and the last action from the station the packets are being replayed.

Before starting the replay process of the TCP packets, the tool reads in the provided pcap 
file that contains one TCP flow, and takes the SEQ & ACK #s of the entire capture. 
Based on the SEQ & ACK numbers read in, the schedule of events is setup for the replay process. 
The tool generates a random number for the first local packet, SYN packet, and then 
starts the replay process by sending this packet to the remote host. Once the local host 
receives the correct response from the remote host, it will complete the TCP 3-Way handshake and 
starts the replay process by replaying the packet immediately after the handshake (if any). \
The local host will only proceed in the replay process if the SEQ & ACK numbers meet t
he expectations of the tool based on the last action of the local host and the original 
captured packets provided to the tool.

<h3><a name="successful-vs-unsuccessful-replays"></a>Successful vs. Unsuccessful Replays?</h3>

Note that the packet replays will not always complete to the end due to possible change 
in behavior of the remote host. The tool can only expect so much of the remote host ahead 
of time. So if the remote host behaves differently than the expectation of the tool, the 
tool will re-attempt replaying several times. Then if the replay does not proceed, the tool 
will error out to the user with possible reasons of why the replay did not complete. 
In this case, re-attempting to replay the entire packet capture from the beginning is your best 
option because it may lead to completing the replay successfully. Note that the longer 
the packet captures are, the more likely there will be a possibility of changed behavior 
from the remote host.

---
<h2><a name="usage"></a>Usage</h2>

Use the following specific syntax to replay a TCP capture:

```
# tcpliveplay <device> <file.pcap> <Destination IP > <Destination MAC> <Source Port>
```

*Device*: The device the packets will be sent out on, such as eth0 or eth1.

*file.pcap*: The “*.pcap” packet capture you desire to replay. Note that all non-TCP 
packets will be filtered out and ignored. Only replay captures that contain one TCP flow.

*Destination IP*: The destination IP string of the remote host you wish to replay 
the captures against.

*Destination MAC*: The destination MAC address of NIC directly connected to your replay station.

*Source Port*: The TCP source port. If the user does not desire a specific port, then may 
instead type “random” which will determine a random number at runtime and use that for the 
source port. The generated numbers will be in the private ports range of 49152 to 65535.

Due to the nature of the replay, you must suppress the kernel RST flags because the 
replay is injecting packets into the replay station’s NIC. Issue the following:

```
# sudo iptables -A OUTPUT -p tcp --tcp-flags RST RST -s <your ip> -d <dst ip> --dport <dst port, example 80 or 23 etc.> -j DROP
```

Example of suppress command:

```
# sudo iptables -A OUTPUT -p tcp --tcp-flags RST RST -s 10.0.2.15 -d 192.168.1.10 --dport 80 -j DROP
```

Here are examples of running tcpliveplay:

```
# tcpliveplay eth0 sample1.pcap 192.168.1.5 52:51:01:12:38:02 random
# tcpliveplay eth0 sample2.pcap 192.168.1.5 52:51:01:12:38:02 52178
```

Types of Packet Captures

This tool can only replay TCP packet captures that contain one TCP flow. Future 
improvements will allow users to replay captures that contain multiple TCP connections 
at the same time.

---
<h2><a name="supported-os's"></a>Supported OS's</h2>

The current state of the tool is only supported on Linux environments. 
The tool will soon be improved to support the other platforms which the
Tcpreplay suite supports.

---
<h2><a name="fresh-install-guide"></a>Fresh Install Guide</h2>

If you experience problems installing the Tcpreplay suite with this tool, 
then do the following. On a fresh Linux system, here are some points
that may help you install it successfully:

1. `sudo apt-get remove libpcap0.8 tcpdump`
2. `sudo apt-get install libtool libc6 bison flex`
3. Downloaded libpcap-1.2.1 and tcpdump-4.2.1 from [http://www.tcpdump.org/][tcpdump]
4. "untarred" them in /usr/local
    * `sudo ./configure --prefix=/usr && sudo make && sudo make install`
    * `tar -C /usr/local -zxvf libpcap-1.2.1.tar.gz`
    * `tar -C /usr/local -zxvf tcpdump-4.2.1.tar.gz`
5.Navigate to each of the above extracted directories and do
    * `sudo ./configure`
    * `sudo make && sudo make install`
6. Remove the default autogen: `sudo apt-get remove autogen`
7. Install [autogen 5.16.2][autogen]
8. Navigate to the directory do:
    * `./configure`
    * `sudo make && sudo make install`
9. Then install the following packages:
10 `sudo apt-get install guile-1.8-libs libc6 libopts25 libopts25-dev libxml2 guile-1.8 guile-1.8-dev`
11. Issue: `mv /usr/lib/libopts.so.25 /usr/lib/~libopts.so.25`
12. Issue: `ln -s /usr/local/lib/libopts.so.25 /usr/lib/libopts.so.25`
13 Checkout the code: 'git clone git@github.com:appneta/tcpreplay.git'
14. Navigate to the directory then do:
    * `./autogen.sh`
    * `./configure`
    * make && sudo make install

<br \>
---
<h2><a name="examples-of-successful-run"></a>Examples of Successful Run</h2>

```
yhsiam:~$ sudo tcpliveplay eth0 sample1.pcap 192.168.1.5 52:51:01:12:38:02 random
[sudo] password for yhsiam: 
new source port:: 61487
Random Local SEQ: 859480898
Packets Scheduled 11
Sending Local Packet...............	[1]
Receiving Packets from remote host...
Received Remote Packet...............	[2]
Remote Pakcet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[3]
Sending Local Packet...............	[4]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[5]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[6]
Remote Packet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[9]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[10]
Remote Packet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[11]
----------------------------------------------------------------------------
- CONGRATS!!! You have successfully Replayed your pcap file ' sample1.pcap'  
----------------------------------------------------------------------------

----------------TCP Live Play Summary----------------
- Packets Scheduled to be Sent & Received: 	    11   
- Actual Packets Sent & Received:                   11   
- Total Local Packet Re-Transmissions due to packet       
- loss and/or differing payload size than expected: 5   
- Thank you for Playing, Play again!                      
----------------------------------------------------------
```

<br \>
---
<h2><a name="example-of-unsuccessful-run"></a>Example of Unsuccessful Run</h2>

```
yhsiam:~$ sudo tcpliveplay eth0 sample2.pcap 192.168.1.5 52:51:01:12:38:02 52139
new source port:: 52139
Random Local SEQ: 2095660524
Packets Scheduled 15
Sending Local Packet...............	[1]
Receiving Packets from remote host...
Received Remote Packet...............	[2]
Remote Pakcet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[3]
Sending Local Packet...............	[4]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[5]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[6]
Payload size of received packet does not meet expectations

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ WARNING: Remote host is not meeting packet size expectations.               +
+ for packet 6. Application layer data differs from capture being replayed.  +
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Requesting retransmission.
 Proceeding...
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[9]
Payload size of received packet does not meet expectations

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ WARNING: Remote host is not meeting packet size expectations.               +
+ for packet 9. Application layer data differs from capture being replayed.  +
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Requesting retransmission.
 Proceeding...
Sending Local Packet...............	[7]

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ ERROR: Re-sent packet [7] 3 times, but remote host is not  +
+ responding as expected. 3 resend attempts are a maximum.     +
+ Closing replay...                                            +
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-------------------------------------------------------------------------
- Unfortunately an error has occurred  halting the replay of  
- the pcap file 'sample2.pcap'. Please see error above for details...   
-------------------------------------------------------------------------

----------------TCP Live Play Summary----------------
- Packets Scheduled to be Sent & Received: 	    15   
- Actual Packets Sent & Received:                   6   
- Total Local Packet Re-Transmissions due to packet       
- loss and/or differing payload size than expected: 8   
- Thank you for Playing, Play again!                      
----------------------------------------------------------
```

<br \>
---
<h2><a name="credits-&-thanks"></a>Credits & Thanks</h2>

Thanks to Aaron Turner for providing extensive support throughout d
eveloping this tool. Aaron, you have been very helpful and very supportive. 
Thanks to Cisco Systems for sponsoring the project and providing expertise in 
the subject area, Mr. Panos Kampanakis. Thanks to Dr. Yannis Viniotis of the 
Electrical & Computer Engineering department at North Carolina State University 
for advising on this project and sponsoring this project with Cisco Systems.

[tcpdump]: http://www.tcpdump.org/#latest-release
[autogen]: http://sourceforge.net/projects/autogen/files/AutoGen/AutoGen-5.16.2/
