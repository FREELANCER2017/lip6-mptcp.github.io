---
layout: post
title:  "Presentation of the website !"
date:   2015-10-08 14:00:27
categories: mptcp ns3 dce
---
This website is a frontend to present the work done in university Pierre & Marie Curie in the Phare team concerning the addition of multipath TCP (MPTCP) support to the network simulator [ns3][ns3].

For our studies, we developed additions to ns3, its Direct Code Execution extension (DCE), wireshark and other programs (iperf3 etc...).
Our intent is to regroup those changes [in a single github repo][mptcp-ns3] while improving them. For now some of theses changes live in other repositories [(as for DCE)][mptcp-dce].
All the code is licensed under a GPL licence, hence any modifications you deploy should be made available for everyone to see.

Our goal is to upstream all changes but it requires lots of work to document, fix style etc...
Feel free to send pull requests via the github tracker.

Keep in mind this is Work In Progress.

Matt

[ns3]:      http://www.nsnam.org
[mptcp-ns3]:   https://github.com/lip6-mptcp
[mptcp-dce]: https://github.com/teto/ns-3-dce/tree/mptcp_tests
