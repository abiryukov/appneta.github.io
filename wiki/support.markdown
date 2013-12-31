---
layout: content
title:  "Support"
categories: tcpreplay wiki
---

Having problems? Try asking for help on the 
tcpreplay-users [tcpreplay-users mail list][maillist]. But first, you are strongly encouraged 
to read the extensive documentation (man
pages, FAQ, documents in /docs and email list archives) BEFORE posting.

If you think you are experiencing a bug, 
you may consider submitting a bug report [here][issues].
It is important that you provide enough information for us to help you.

If your problem has to do with COMPILING tcpreplay include the following:

* Version of tcpreplay you are trying to compile
* Platform (Red Hat Linux 9 on x86, Solaris 7 on SPARC, OS X on PPC, etc)
* Contents of config.status
* Output from **configure** and **make**
* Any additional information you think that would be useful.

If your problem has to do with RUNNING tcpreplay or one of the sub-tools include the following:

* Version information (output of -V)
* Command line used (options and arguments)
* Platform (Red Hat Linux 9 on Intel, Solaris 7 on SPARC, etc)
* Make & model of the network card(s) and driver(s) version
* Error message (if available) and/or description of problem
* If possible, attach the pcap file used (compressed with bzip2 or gzip preferred)
* The core dump or backtrace if available
* Detailed description of your problem or what you are trying to accomplish

Note: The maintainers of tcpreplay primarily use OS X and Linux; hence, if you're reporting
an issue on another platform, it is important that you give very detailed
information as I may not be able to reproduce your issue.

Lastly, please don't email the maintainers directly with your questions.  Doing so
prevents others from potentially helping you and your question/answer from
showing up in the list archives.

[maillist]: maillist.html
[issues]:   https://github.com/appneta/tcpreplay/issues