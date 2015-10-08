---
layout: post
title:  "MPTCP simulator"
date:   2015-10-08 14:00:27
categories: mptcp ns3 dce
---
This page is a frontend to present the work done at <a href="http://www.lip6.fr">LIP6</a> concerning the addition of multipath TCP (MPTCP) support to the network simulator [ns3][ns3].

For our studies, we developed additions to ns3, its Direct Code Execution extension (DCE), wireshark and other programs (iperf3 etc...).
Our intent is to regroup those changes [in a single github repo][mptcp-ns3] while improving them. For now some of theses changes live in other repositories [(as for DCE)][mptcp-dce].
All the code is licensed under a GPL licence, hence any modifications you deploy should be made available for everyone to see.

Our goal is to upstream all changes but it requires lots of work to document, fix style etc...
Feel free to send pull requests via the github tracker.

Keep in mind this is Work In Progress.

Matt

[ns3]:      http://www.nsnam.org
[mptcp-ns3]:   https://github.com/lip6-mptcp/ns3mptcp
[mptcp-dce]: https://github.com/teto/ns-3-dce/tree/mptcp_tests
