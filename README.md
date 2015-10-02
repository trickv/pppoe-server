# pppoe-server
Quick hack vagrant setup to develop & test VyOS IPv6 PPPoE.

This generates you a VM with two NICs on it; eth1 becomes a PPPoE server on a host-only Virtualbox network. You can then spin up a VyOS VM which connects to this same host-only network and establish a PPPoE session into it.

It doesn't do any actual routing, it just establishes the PPPoE session.

It also configures radvd to advertise some prefixes.  Unfortunately radvd doesn't detect new interfaces cleanly and the server-side interfaces are created & destroyed with each client connecting, so after you establish a PPPoE client, you'll need to run:
 sudo service radvd restart

radvd advertises [ULA](https://tools.ietf.org/html/rfc4193) /64 prefixes from [fd7e:aade:9c36::/48](https://www.sixxs.net/tools/whois/?fd7e:aade:9c36::/48)
