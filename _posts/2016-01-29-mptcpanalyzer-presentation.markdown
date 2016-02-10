---
layout: post
title:  "MPTCP analysis with wireshark and mptcpanalyzer"
date:   2016-01-26 14:00:27
categories: mptcp mptcpanalyzer
---

MPTCP being a relatively new protocol (working group started in 2009 but 
operational implementations are much more recent), there are very few tools available 
to help analyze the protocol. In fact, we know of only one: [mptcptrace].

mptcptrace is capable of doing bulk analysis and was for instance used 
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

Matt

[Wireshark]:       http://www.wireshark.org
[mptcpanalyzer]:   https://github.com/lip6-mptcp/mptcpanalyzer
