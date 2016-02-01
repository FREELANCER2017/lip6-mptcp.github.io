
---
layout: post
title:  "MPTCP dissection in wireshark"
date:   2016-10-08 14:00:27
categories: mptcp wireshark
---

[Wireshark] is a network i
Over the past year, we introduced a number of improvements in MPTCP related dissection:
1. https://code.wireshark.org/review/10577
2. https://code.wireshark.org/review/11683
3. https://code.wireshark.org/review/10504
4. https://code.wireshark.org/review/10577
5. https://code.wireshark.org/review/11683
(To get a full list of loosely related changes, you can run $ git log --author="Matthieu Coudron" in your wireshark repository).

There is a last patch pending that needs more work & tests before upstreaming:
https://code.wireshark.org/review/#/c/12316/

For ease of use, we provide a github mirror [custom] incorporating all current changes (branch mptcp_final). Keep in mind it is a work in progress and don't hesitate to report possible bugs in the github tracker.

Here is a picture of some additionnal fields that are available when using our version of wireshark:
![wireshark ](./images/wireshark_news.png)

The building is the same as for vanilla wireshark. Here is an example on how to install/use it with cmake (wireshark can use autotools directly or cmake):
$ git clone https://github.com/lip6-mptcp/wireshark-mptcp.git
$ cd wireshark-mptcp
$ mkdir debug
$ cd debug

Here is my custom command, feel free to change the compiler to yours or just remove the commands:
$ CXXFLAGS="-Wno-unused-but-set-variable" cmake \
        -G"Unix Makefiles" \
        -DENABLE_GTK3=0 \
        -DENABLE_PORTAUDIO=0 \
        -DENABLE_QT5=1 \
        -DENABLE_GEOIP=0 \
        -DENABLE_KERBEROS=1 \
        -DENABLE_SBC=0 \
        -DENABLE_SMI=0 \
        -DENABLE_GNUTLS=1 \
        -DENABLE_GCRYPT=1 \
        -DCMAKE_BUILD_TYPE=Debug \
        -DDISABLE_WERROR=1 \
        -DENABLE_EXTRA_COMPILER_WARNINGS=0 \
        .. \
        -DCMAKE_C_FLAGS=$(printf %q "$CFLAGS") \
        -DCMAKE_CXX_FLAGS=$(printf %q "$CXXFLAGS") \
        -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
        -DCMAKE_C_COMPILER=clang \
        -DCMAKE_CXX_COMPILER=clang++ 

$ make wireshark

Full MPTCP dissection can be quite CPU-consuming (optimization was not our priority), thus we provide some options to enable only the needed features.
![options](./images/wireshark_options.png).

Here is my custom MPTCP profile [mptcpprofile] in case it helps (finding the field names) !

Keep in mind this is Work In Progress.

Matt

[custom]: 		https://github.com/lip6-mptcp/wireshark-mptcp
[Wireshark]:       http://www.wireshark.org
[mptcpanalyzer]:   https://github.com/lip6-mptcp/mptcpanalyzer
[mptcpprofile]:	   https://github.com/teto/home/blob/master/config/wireshark/profiles/mptcp/preferences
