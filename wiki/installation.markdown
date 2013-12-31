---
layout: content
title:  "Download and Installation"
categories: tcpreplay wiki
---

- [Downloads](#downloads)
    - [Download Releases](#download-releases)
    - [Download Source](#download-source)
- [Installation](#installation)
	- [Simple directions for Unix users](#simple-directions-for-unix-users)
	- [Build netmap feature](#build-netmap-feature)
- [Advanced Options](#advanced-options)
- [Special Instructions for Windows](#special-instructions-for-windows)
- [Need Help?](#need-help)


<h2><a name="downloads"></a>Downloads</h2>

<h3><a name="download-releases"></a>Download Releases</h3>

* Latest stable release:
 - [tcpreplay-3.4.4.tar.gz][tcpreplay-3.4.4]
 - [PGP signagture][tcpreplay-3.4.4.asc]
 - [Changelog][tcpreplay-3.4.4.changelog]
 - [Download stats][stats]
 
* Latest BETA release:
 - [tcpreplay-4.0.0beta2][tcpreplay-4.0.0beta2] 
 - [Beta 2 announcement][4.0.0beta2_announce]   
 - [Beta 1 announcement][4.0.0beta1_announce]   
* [Old Win32 release][win32]
 - Note that Windows is not supported by the maintainer   
* [Complete list of past releases][old_stuff]

<h3><a name="download-source"></a>Download Source</h3>
Compiling source from [GitHub][repo] has more requirements that compiling a release.
Specifically you must have [AutoGen][autogen] installed. If you plan to contribute you must
have [AutoGen version 5.16.2][autogen-download], otherwise your [pull requests]
will be rejected.

If you want to help develop Tcpreplay visit our [Developer Wiki][dev-wiki].

* Download via [GitHub][repo]
 - `git clone https://github.com/appneta/tcpreplay`
* Or if you plan to contribute someday simply [fork][fork] the [repo][repo] and submit a [pull request][pullreq] when
you are ready to share your changes with us
* Or download the latest [master tarball][tarball]
* Note that *master* is always production ready, but not necessary   
the latest stable release. See [GitHub network][network]
to see the state of master


<h2><a name="installation"></a>Installation</h2>

<h3><a name="simple-directions-for-unix-users"></a>Simple directions for Unix users</h3>

```
./configure 
make
sudo make install
```

<h3><a name="build-netmap-feature"></a>Build netmap feature</h3>
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

You can also find netmap source at http://code.google.com/p/netmap/
<h2><a name="advanced-options"></a>Advanced Options</h2>

There are quite a few configure time options for tcpreplay which allow you to control a lot
of things. Some of the more interesting ones are:

* *--enable-debug* -- useful for debugging bugs and crashes.
* *--enable-64bits* -- use 64 bit counters to handle large pcap files & looping
* *--enable-libnet* -- link to libnet. Note that libnet support has been deprecated due to
various bugs which have not been fixed in over a year.
* *--with-libnet* -- specify root path to libnet (something like --with-libnet=/usr/local)
* *--with-libpcap* -- specify root path to libpcap
* *--with-netmap* -- specify root path to netmap
* *--with-tcpdump* -- specify path to tcpdump executable
* *--enable-tcpreplay-edit* -- compile tcpreplay with packet editing support

You can also manually select a particular method to inject packets:

* *--enable-force-pf* -- force tcpreplay to use Linux's PF_PACKET to send packets
* *--enable-force-bpf* -- force tcpreplay to use Free/Net?/OpenBSD or OS X's BPF interface
to send packets
* *--enable-force-libnet* -- force tcpreplay to use Libnet to send packets
* *--enable-force-inject* -- force tcpreplay to use Libpcap's pcap_inject() API to send packets
-* *-enable-force-sendpacket* -- force tcpreplay to use Libpcap's pcap_sendpacket() API
to send packets

If you're having compatibility issues with a system installed GNU Autogen, 
you may want to consider these options:

* *--disable-local-libopts* -- Don't use the libopts tearoff supplied with tcpreplay 
(default is enabled)
* *--disable-libopts-install* -- don't install the libopts library files

<h2><a name="special-instructions-for-windows"></a>Special Instructions for Windows</h2>

Consider Windows support for Tcpreplay is experimental- beta quality if you will.
We strongly recommend you read the page about [how to get support for Tcpreplay][Support].

With that said, you'll need [Cygwin] to compile/run tcpreplay. You'll also need to install 
[Winpcap] the port of libpcap for Windows. For whatever reason, it seems important 
that you install the Winpcap files in the Cygwin root directory (/Wpdpack).

Be sure to install both the [driver and DLL][dll] files AND [developer pack][devpack]. 
Then when you run 
./configure, you'll need to specify the location for Winpcap using the `--with-libpcap` 
flag, but use all lowercase: ./configure `--with-libpcap=/wpdpack`.

After that, for the most part things should just work. There are some caveats; a few 
features and `make test` don't work, but for the most part they seem to be pretty minor.

For more detailed instructions, see the [Win32Readme.txt].

Note: We've been informed that the guile Cygwin package is broken.
This horribly breaks parts of GNU Autogen- specifically the parts which
allow you to build Tcpreplay via GitHub. Hence, I strongly recommend grabbing 
a tarball release.

<h2><a name="need-help"></a>Need Help?</h2>

Having problems? Try asking for help on the 
tcpreplay-users [mailing list][maillist] or check out the [Support] section.


[maillist]: maillist.html
[Support]:  support.html
[nm]:       http://info.iet.unipi.it/~luigi/netmap/
[Cygwin]:   http://www.cygwin.com/
[flow]:     http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
[Winpcap]:  http://www.winpcap.org
[dll]:      http://www.winpcap.org/install/default.htm
[devpack]:  http://www.winpcap.org/devel.htm
[tcpreplay-3.4.4]:           http://prdownloads.sourceforge.net/tcpreplay/tcpreplay-3.4.4.tar.gz?download
[tcpreplay-3.4.4.asc]:       http://tcpreplay.synfin.net/raw-attachment/wiki/Download/tcpreplay-3.4.4.tar.gz.asc
[tcpreplay-3.4.4.changelog]: http://tcpreplay.synfin.net/browser/tags/3.4.4/docs/CHANGELOG
[tcpreplay-4.0.0beta2]:      https://drive.google.com/folderview?id=0Bwy1iN_ElthJWi14Mnh5YUJTUWs&usp=docslist_api#
[tcpreplay-4.0.0beta2.asc]:  https://drive.google.com/file/d/0Bwy1iN_ElthJeXdmT05jME4yc3M/edit?usp=sharing
[4.0.0beta1_announce]:       {{ site.url }}/tcpreplay/news/2013/12/20/4-0-beta1.html
[4.0.0beta2_announce]:       {{ site.url }}/tcpreplay/news/2013/12/22/4-0-beta2.html
[win32]:                     http://sourceforge.net/projects/tcpreplay/files/tcpreplay-win32/
[old_stuff]:                 http://sourceforge.net/projects/tcpreplay/files/
[stats]:                     http://sourceforge.net/projects/tcpreplay/files/stats/timeline
[tarball]:                   https://github.com/appneta/tcpreplay/tarball/master
[network]:                   https://github.com/appneta/tcpreplay/network
[fork]:                      https://help.github.com/articles/fork-a-repo
[repo]:                      https://github.com/appneta/tcpreplay
[pullreq]:                   https://help.github.com/articles/fork-a-repo#pull-requests
[autogen]:                   http://autogen.sourceforge.net/
[autogen-download]:          http://ftp.gnu.org/gnu/autogen/rel5.16.2/
[dev-wiki]:                  https://github.com/appneta/tcpreplay/wiki
[Win32Readme.txt]:           https://github.com/appneta/tcpreplay/blob/master/docs/Win32Readme.txt
