This proposal lacks sufficient justification for the large scale attacks that it naively facilitates.

> We assume peer operators want to limit the access of different node services or increase datastream priorities to a selective subset of peers.

This is a case where the interests of "peer operators" are at odds with the interest of Bitcoin as a public network. There is no discussion in the proposal of the significant systemic risks that it introduces. Bitcoin must remain a public network for its security model to remain valid.

> Also we assume that peers want to connect to specific peers to broadcast or filter transactions (or similar actions that reveal sensitive informations)

Anonymity cannot be achieved by connection to strongly-identifiable peers. This requires an anonmizing network. See comments on BIP150.

> and therefore operators want to authenticate the remote peer and ensure that they have not connected to a MITM (man-in-the-middle) attacker.

Another word for this screening is censorship.

> Benefits of peer authentication:
>
> * Peers can detect MITM attacks when connecting to **known peers**
> * Peers can allow resource hungry **transaction filtering** only to **specific peers**
> * Peers can **allow access** to sensitive information that can lead to node fingerprinting (fee estimation)
> * Peers can allow custom message types (**private** extensions) to **authenticated peers**

This is a laundry list of forms of censorship options; selectively allowing access to *specified people*. The identity of a peer is not meaningful if it is anonymous. Identity ultimately refers to the person who controls the node.

Incidentally, it is not a necessary aspect of a Bitcoin node that it retain the "fingerprints" of past users. This is generally a design flaw resulting from ill-advised attempts at optimization. It is also not a material issue if one avoids the ill-advised use of Tor in the P2P protocol itself, and properly uses a trusted full node for queries and only posts transactions as a Client to a public Server via an anonymizing network.

> A simple authentication scheme based on elliptic cryptography will allow peers to identify each other and selectively allow access to restricted services or reject the connection if the peer identity cannot be verified.

Simple or not, there is no justification for integrating node identity into the P2P protocol. By design Bitcoin provides *consensus-based validation* as the means to ensure the validity of information obtained from untrusted peers. Introduction of trust into the protocol will inevitably lead to calls for all "major players" (mining pools, exchanges, web wallets, web APIs, merchant services, etc.) to both establish private connections to each other and eventually to exclude all connections that do not provide an authorized identity.

Such requirements will eventually arrive from regulatory agencies around the world. This BIP should be referred to as the "Know Your Customer" proposal, if not the central "Anti-Money Laundering" feature. As this happens, which is entirely predictable, Bitcoin will become a private financial network reachable for the public only through banks calling themselves Bitcoin Wallets.

--Eric Voskuil, 2017-01-21