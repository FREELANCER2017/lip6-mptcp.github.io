---
layout: post
title:  "Hybrid MPTCP simulation with ns3/DCE"
date:   2016-01-26 14:00:27
categories: mptcp dce ns3
---

First you may wonder what I mean by "hybrid simulation" ?

ns3 is discrete network simulator and by default can only run simulations based on models, 
i.e. abstraction that simplify to focus on the key aspects of technologies. 
It makes experimenting and reproductibility easier but at the cost of ignoring some real world
artefacts. It also implies that models need to be developed beforehand which can be costly and 
not necessarely interesting outside the algorithmic aspects of a protocol.

[DCE] is a project that aims at 

[Wireshark] is a network analyzer.
Over the past year, we introduced a number of improvements in MPTCP related dissection:

1. [Registers an 'mptcp' protocol](https://code.wireshark.org/review/10577)
2. [Introduced interval trees to handle DSN/SSN search](https://code.wireshark.org/review/#/c/11714/)

To get a full list of loosely related changes, you can run in your wireshark repository:
{% highlight bash %}
$ git log --author="Matthieu Coudron" 
{% endhighlight %}

There is a [last patch](https://code.wireshark.org/review/#/c/12316/) pending that needs more work & tests before upstreaming.

For ease of use, we provide a custom [github mirror](https://github.com/lip6-mptcp/wireshark-mptcp) incorporating all current changes (checkout the branch 'mptcp_final'). 

Here is a picture of some additionnal fields that are available when using our version of wireshark:
![wireshark]({{ site.baseurl }}/images/wireshark_new.png)

The building is the same as for vanilla wireshark. Here is an example on how to install/use it with cmake (wireshark can use autotools directly or cmake):

{% highlight bash %}
$ git clone https://github.com/lip6-mptcp/wireshark-mptcp.git
$ cd wireshark-mptcp
$ mkdir debug
$ cd debug
{% endhighlight %}
Here is my custom command, feel free to change the compiler to yours or just remove the commands:
{% highlight bash %}
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
{% endhighlight %}
Full MPTCP dissection can be quite CPU-consuming (optimization was not our priority), thus we provide some options to enable only the needed features in the menu "Edit -> Preferences -> Protocols -> MPTCP". Just tick the boxes but keep in mind it is a work in progress and don't hesitate to report (or fix) bugs in the github tracker.

Here is [my custom MPTCP profile](https://github.com/teto/home/blob/master/config/wireshark/profiles/mptcp/preferences) in case it helps (finding the field names) !




New Linux mptcp version (0.88) with DCE.

As Linux mptcp version 0.88 has released (http://multipath-tcp.org/pmwiki.php?n=Main.Release88), the following is the update for DCE integration of the version. This version is based on Linux 3.11.0 branch of net-next-sim (DCE).

1. get mptcp code
 % git clone -b mptcp_v0.88 git://github.com/multipath-tcp/mptcp

2. add ns-3-dce (Direct Code Execution) code and merge to mptcp code
 % cd mptcp
 % git remote add dce git://github.com/direct-code-execution/net-next-sim.git 
 % git fetch dce
 % git merge dce/sim-ns3-3.11.0-branch

3. patch a bit manually
% cat >> arch/sim/defconfig <<END
CONFIG_MPTCP=y
CONFIG_MPTCP_PM_ADVANCED=y
CONFIG_MPTCP_FULLMESH=y
CONFIG_MPTCP_NDIFFPORTS=y
CONFIG_DEFAULT_FULLMESH=y
CONFIG_DEFAULT_MPTCP_PM="fullmesh"
CONFIG_TCP_CONG_COUPLED=y
CONFIG_TCP_CONG_OLIA=y

END

% make defconfig ARCH=sim

3.1 or menuconfig to enable these options
% make menuconfig ARCH=sim

4. build kernel (as a shared library)
 % make library ARCH=sim

If everything is going well, you can try to use it over ns-3

1. build ns-3 related tools
 % make testbin -C arch/sim/test

2. run mptcp simulation !
 % cd arch/sim/test/buildtop/source/ns-3-dce
 % ./waf --run dce-iperf-mptcp

you will see generated pcap files by this execution.

build steps for DCE integration is also available as a script;

https://gist.github.com/thehajime/8077571/raw/d663819992cf9822b150bd8e8c14474bb363a9d9/mptcp0.88-build.sh﻿

Matt

[Wireshark]:       http://www.wireshark.org
[mptcpanalyzer]:   https://github.com/lip6-mptcp/mptcpanalyzer
[tuto_mptcp0.86]: https://plus.google.com/u/0/+HajimeTazaki/posts/aYuboVFcQeF
[tuto_mptcp0.88]: https://plus.google.com/+HajimeTazaki/posts/1QUmR3n3vNA
