---
layout: post
title:  "MPTCP analysis with wireshark and mptcpanalyzer"
date:   2016-01-26 14:00:27
categories: mptcp mptcpanalyzer
---

MPTCP being a relatively new protocol (working group started in 2009 but 
operational implementations are much more recent), there are very few tools available 
to help analyze the protocol. In fact, we know of only [mptcptrace] and [netperfmeter].

mptcptrace is capable of doing bulk analysis and was for instance used in the [real traffic] paper.

When developing our ns3 mptcp simulator, debugging was an important part of the work.
To debug in optimal conditions, we wanted to be able to inspects traces at the packet level, which mptcptrace can't do.
Hence we improved mptcp support in [wireshark] as described in [wireshark presentation].
Also using wireshark, we benefit from all the features already implemented such as inplace filtering,
connection tracking etc...

We then decided to leverage the csv export and couple it with [pandas] (a python library for data analysis) to 
produce some information about mptcp connections. We bundle the resulting python program under the name of
[mptcpanalyzer]. The program exhibits an interactive command line with integrated help and partial 
autocompletion to for instance:

- list the mptcp connections and their subflows
- plot per-subflow data sequence numbers for a single MPTCP connection
- etc...

We intend to add new commands and plots when necessary, which is reasonably easy with python.

You can find more information along with a tutorial in the README of [mptcpanalyzer].

Matthieu Coudron

[Wireshark]:       http://www.wireshark.org
[mptcpanalyzer]:   https://github.com/lip6-mptcp/mptcpanalyzer
[wireshark presentation]:     http://lip6-mptcp.github.io/mptcp/wireshark/2016/01/26/wireshark-presentation.html
[mptcptrace_paper]:     http://dl.acm.org/citation.cfm?id=2631453
[real traffic]:      Benjamin Hesmans, Hoang Tran-Viet, Ramin Sadre and Olivier Bonaventure. A First Look at Real Multipath TCP Traffic. Traffic Monitoring and Analysis, 2015 http://inl.info.ucl.ac.be/publications
[netperfmeter]:     https://www.uni-due.de/~be0001/netperfmeter/
[pandas]:       http://pandas.pydata.org/
