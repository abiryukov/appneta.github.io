---
layout: content
title:  "Downloads"
categories: tcpreplay content
---

**Releases**

* Latest stable release:
 - [tcpreplay-3.4.4.tar.gz][tcpreplay-3.4.4]
 - [PGP signagture][tcpreplay-3.4.4.asc]
 - [Changelog][tcpreplay-3.4.4.changelog]
 - [Download stats][stats]
 
* Latest BETA release:
 - [tcpreplay-4.0.0beta1.tar.gz][tcpreplay-4.0.0beta1] 
 - [PGP signature][tcpreplay-4.0.0beta1.asc]
 - [Announcement][4.0.0beta1_announce]   
* [Old Win32 release][win32]
 - Note that Windows is not supported by the maintainer   
* [Complete list of past releases][old_stuff]


**Source**   
Compiling source from [GitHub][repo] has more requirements that compiling a release.
Specifically you must have [AutoGen][autogen] installed. If you plan to contribute you must
have [AutoGen version 5.16.2][autogen-download], otherwise your [pull requests]
will be rejected.

* Download via [GitHub][repo]
 - `git clone https://github.com/appneta/tcpreplay`
* Or if you plan to contribute someday simply [fork][fork] the [repo][repo] and submit a [pull request][pull] when
you are ready to share your changes with us
* Or download the latest [master tarball][tarball]
* Note that *master* is always production ready, but not necessary   
the latest stable release. See [GitHub network][network]
to see the state of master

[tcpreplay-3.4.4]:           http://prdownloads.sourceforge.net/tcpreplay/tcpreplay-3.4.4.tar.gz?download
[tcpreplay-3.4.4.asc]:       http://tcpreplay.synfin.net/raw-attachment/wiki/Download/tcpreplay-3.4.4.tar.gz.asc
[tcpreplay-3.4.4.changelog]: http://tcpreplay.synfin.net/browser/tags/3.4.4/docs/CHANGELOG
[tcpreplay-4.0.0beta1]:      https://drive.google.com/file/d/0Bwy1iN_ElthJc3RwU0dMWGxBcGs/edit?usp=sharing
[tcpreplay-4.0.0beta1.asc]:  https://drive.google.com/file/d/0Bwy1iN_ElthJeXdmT05jME4yc3M/edit?usp=sharing
[4.0.0beta1_announce]:       /tcpreplay/news/2013/12/20/4-0-beta1.html
[win32]:                     http://sourceforge.net/projects/tcpreplay/files/tcpreplay-win32/
[old_stuff]:                 http://sourceforge.net/projects/tcpreplay/files/
[stats]:                     http://sourceforge.net/projects/tcpreplay/files/stats/timeline
[tarball]:                   https://github.com/appneta/tcpreplay/tarball/master
[network]:                   https://github.com/appneta/tcpreplay/network
[fork]:                      https://help.github.com/articles/fork-a-repo
[repo]:                      https://github.com/appneta/tcpreplay
[pull]:                      https://help.github.com/articles/fork-a-repo#pull-requests
[autogen]:                   http://autogen.sourceforge.net/
[autogen-download]:          http://ftp.gnu.org/gnu/autogen/rel5.16.2/
