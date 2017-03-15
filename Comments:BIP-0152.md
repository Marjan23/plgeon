[This section](https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki#protocol-versioning) part 6:

> Thus, if a node wishes to determine exactly which version of compact blocks will be used before requesting a compact block object, it must send all of its sendcmpct version announcements, followed by a ping, and wait for the pong response to ensure it has received all sendcmpctblock version announcement messages from the remote peer...

implies that the Bitcoin P2P protocol generally requires that messages are handled and responded to in the order received. This is not reasonable to assume or require, nor is it consistent with behavior of existing nodes. This is especially problematic as it pertains to unrelated protocols, such as between ping-pong and compact blocks. The only ordering assumption is that a response can only follow its request, for message types that are response-only.

--Eric Voskuil, 2017-01-21


The protocol has strong requirements for ordering already, and has always had such a requirement.  Moreover, no compact block messages will be sent until the a sendcmpact message has been processed; so there would not be ambiguity except in odd circumstances (described in the BIP).  This proposal is very well constructed, was extensively reviewed, and deployed and successfully used on an overwhelming super-majority of nodes on the network.

--Greg Maxwell, 2017-03-14
 
I am generally in favor of this proposal. I would not consider my comment above an objection. My comment is that the enlistment of one protocol into the service of another (ping into compact blocks) complicates clean asynchronous implementation. It is true that the various Bitcoin protocols have ordering requirements. It is also true that sendcmpact activates the protocol. I did not dispute either of these ideas.

However this proposal introduces an ordering requirement on ping/pong such that all sendcmpctblock messages must be sent before responding to a ping that arrived after preceding sendcmpct messages. There were not previously any constraints on ping/pong from other protocols, apart from the usual requirement that the handshake complete before activating other protocols. So now ping/pong must be implemented as part of the compact blocks protocol. Otherwise one must be assuming sequential processing of all messages. It is this assumption that I find problematic.

--Eric Voskuil, 2017-03-15