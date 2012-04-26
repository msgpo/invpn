invpn
=====

VPN server written in C++ (Qt)

Concept
-------

The goal of this VPN system is to provide fully decentralized VPN service
between servers in various places.

Each node will connect to at least two other nodes, and accept connections from
any node on the network. Each connected node will broadcast to all its
neighboors details such as its own MAC address, and forward broadcasts from any
of its neightboors. It will also keep track of received broadcast messages from
other nodes including from which connection that broadcast was received first, in
order to know where to route packets to that node.

All connections are established via TCP/IP sockets with SSL encryption. A common
CA is used to recognize nodes from the same group, and prevent unauthorized
access to the VPN network. Each node must have its own certificate issued by the
CA with Common Name set to the node's MAC address.

Packet types
------------

* 0: info broadcast. 1 byte version (default=1), 8 bytes id, 6 bytes MAC addr
* 1: targetted packet. 6 bytes dst, 6 bytes src, data
* 2: broadcast. 8 bytes id, 6 bytes src, data

Each packet is made of 2 bytes for the length (big endian), then 1 byte type.
The length is not counted as part of the packet when computing its length.
