The [motivation section](https://github.com/bitcoin/bips/blob/master/bip-0151.mediawiki#motivation) fails to justify the proposal.

> Encrypting traffic between peers is already possible with VPN, tor, stunnel, curveCP or any other encryption mechanism on a deeper OSI level, however, most mechanism are not practical for SPV or other DHCP/NAT environment and will require significant knowhow in how to setup such a secure channel.

The above fails to provide a rationale for encryption of the traffic in the first place. The P2P network is a distributed cache of public information. There is no private information in the traffic that is passed between peers. Validation exists specifically to ensure the integrity of information received from untrusted/anonymous peers. At the same time the proposal fails to describe how encrypted communication with a potential attacker provides the security benefits implied by listing the other technologies.

`Comment Jonas Schnelli`
`Eliminating passive mass surveillance can be desirable (programs like PRISM, etc.).`
`Also, as active MITM you don't know if your MITM-activities are detected or not, this itself is a huge privacy /security improvement`

> This BIP also describes a way that data manipulation (blocking commands by a intercepting TCP/IP node) would be identifiable by the communicating peers.

Without knowing whether the peer itself is the "manipulator" this is pointless.

`Comment Jonas Schnelli`
`BIP151 eliminates passive observers, BIP150 active`

> Encrypting peer traffic will make analysis and specific user targeting much more difficult than it currently is.

This speculation is unsupported.

`Comment Jonas Schnelli`
`I think it's a mathematical fact that encrypted traffic is harder to analyse then plaintext traffic. You can't just passively "listen", you need to substitute ephemeral keys in both directions and actively maintains the connection and risk being detected`

> Today it's trivial for a network provider or any other men-in-the-middle to identify a Bitcoin user and its controlled addresses/keys (and link with his Google profile, etc.).

It will remain trivial as anyone can be a peer on the network. This will not in any way make it difficult for a network provider to identify a Bitcoin user or generally obtain the contents of the user's Bitcoin communications.

> Just created and broadcasted transactions will reveal the amount and the payee to the network provider.

This will remain the case as a network provider can easily direct the user to the provider's node(s).

> The Bitcoin network does not encrypt communication between peers today. This opens up security issues (eg: traffic manipulation by others) and allows for mass surveillance / analysis of bitcoin users.

The security issue described (traffic manipulation) is not "opened up" by the lack of encryption between anonymous peers, it's inherent in the fact that it's a public network.

> Mostly this is negligible because of the nature of Bitcoins trust model,

This is of course true.

> however for SPV nodes this can have significant privacy impacts [1] and could reduce the censorship-resistance of a peer.

1. So-called "SPV nodes" are not peers on the network as they do not provide blocks (or relay transactions). The Peer-To-Peer protocol should not be driven by the needs of a Client-Server system. There exist countless ways to devise connections between a client and a trusted server that has access to a node.

2. The Bloom Filters approach has significant privacy flaws. These cannot be fixed with encryption against anonymous peers. Yet this remains, in the words of the proposal itself, the only non-negligible benefit of the proposal.

3. Censorship-resistance, refers specifically to the ability to remain anonymous while posting or getting individual transactions of interest, preventing taint. This problem *requires* an anonymizing network, such as Tor or I2P. Notably that network must *not* be applied to the P2P protocol, as this easily defeats the supposed protections. Securing the Client-Server scenario against untrusted servers requires that the client connect to a server using an anonymizing network, perform a single get or post, and disconnect. Otherwise the Client may establish secure access to a *trusted server* (not anonymous/opportunistic). Clearly this is not achieved through encryption and is not achieved through relay timing delays.

Without sufficient justification the remaining sections are moot. There is also the relationship to BIP-150, which requires such an encryption scheme in order to implement node identity. That however is not mentioned in the motivation for this proposal, and in fact is subject to its own lack of sufficient justification to outweigh the serious consequences that may very well result from making private links between trusted parties an integral feature of the P2P network.

--Eric Voskuil, 2017-01-21

This is a poor forum for debate, so I will respond just briefly to Jonas' inline comments above. Each comment uses the term "passive". The use is both [incorrect](https://en.wikipedia.org/wiki/Attack_(computing)) and irrelevant. The comments also fail to recognize that the peer may be the attacker.

* An "active attack" attempts to alter system resources or affect their operation.
* A "passive attack" attempts to learn or make use of information from the system but does not affect system resources. (E.g., wiretapping.)

Attaching a bitcoin node to the network is not an active attack. It is simply a way to monitor the traffic on the network. Attaching a large number of nodes to the network remains a passive attack, just as would be attaching a large number of monitors to wire traffic.

The distinction is however irrelevant, as previously discussed on bitcoin-dev, because the difficulty of the attack is more important than whether it is active or passive. It is actually much *easier* to attack the network by attaching nodes to it than to monitor wire traffic. Anyone can attach any number of nodes to the network. It is also irrelevant because the traffic itself is not private. The response that node *identity* is the way this is resolved lays bare the reality that BIP151 serves no purpose other than to support BIP150.

I appreciate that the development of BIP150 and BIP151 was done for the right reasons but that is not at all a justification, especially given that the consequence of node identity will be catastrophic if widely deployed.

--Eric Voskuil, 2017-02-11

The proposal states:

> This proposal is backward compatible. Non-supporting peers will ignore the encinit messages.

This statement is incorrect. Sending content that existing nodes do not expect is clearly an incompatibility. An implementation that ignores invalid content leaves itself wide open to DOS attacks. The version handshake must be complete before the protocol level can be determined. While it may be desirable for this change to precede the version handshake it cannot be described as backward compatible.

--Eric Voskuil, 2017-02-12


This proposal is generally a good one, if immature. Protecting
users against mass surveillance an a moral imperative for all protocol
designers. This proposal is a necessary step and a sufficient measure to
fully prevent passive content capture and render all active interception
detectable.

The prior criticisms are incorrect. This proposal is compatible, and 
virtually every other extension sends new messages that old nodes
do not understand. 

We know for a fact that the Bitcoin network is under active surveillance
by parties for the purpose of tracing the origins of transactions
carried by it. Although the blockchain itself is public, the
general traffic of the network is not, and Bitcoin node software
goes through considerable effort to anonymize the traffic passing through
it.

Using message authentication to prevent tampering is a necessary ingredient
to reduce the power of network level (including state) attackers to perform
partitioning attacks.

This proposal will also reduce the CPU usage of the P2P protocol on most
systems.

Users of the Bitcoin system should stand up and soundly reject parties that try to deny people access to strong encryption.

--Greg Maxwell, 2017-03-14