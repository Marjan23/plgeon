[This section](https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki#protocol-versioning) part 6:

> Thus, if a node wishes to determine exactly which version of compact blocks will be used before requesting a compact block object, it must send all of its sendcmpct version announcements, followed by a ping, and wait for the pong response to ensure it has received all sendcmpctblock version announcement messages from the remote peer...

implies that the Bitcoin P2P protocol generally requires that messages are handled and returned in the order received. This is not reasonable to assume or require, nor is it consistent with behavior of existing nodes. This is especially problematic as it pertains to unrelated protocols, such as between ping-pong and compact blocks. The only ordering assumption is that a response can only follow its request, for message types that are response-only.

--Eric Voskuil, 2017-01-21