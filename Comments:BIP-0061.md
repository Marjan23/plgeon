[BIP61 states](https://github.com/bitcoin/bips/blob/master/bip-0061.mediawiki#Compatibility):

> The reject message is backwards-compatible; older peers that do not recognize the reject message will ignore it.

This is an incorrect assumption. Peers of older versions should (and some do) validate the protocol against invalid messages. Sending a reject message when the peer has announced a version below 70002 should not be allowed by the protocol as it is not backward compatible.

It is also incorrect to assume that older protocol versions will cease to exist. It is perfectly reasonable for ancient versions of protocols to continue to operate. These protocols do not have expiration dates. New implementations can and do support configurable lower protocols levels than 70002.

If this precedent is allowed to hold the network is subject to trivial DOS attacks. The requirement to treat invalid messages as valid precludes a node from dropping the peer due to incorrect behavior. This statement should be struck (and offending implementations should be fixed so that they do not intentionally send invalid messages).

--Eric Voskuil, 2017-02-07

Clean and straightforward. Nodes should implement this. (Disagree with Voskuil's assessment, as nodes have always been expected to ignore unknown messages.) --Luke Dashjr, 2017-03-14

I also disagree with Voskuil's assessment: nodes have always ignored unknown messages. However, the reject message is of minimal utility and adds many strings to the protocol which are a common source of vulnerabilities and which wastes bandwidth with no clear gain. I recommend implementations do not implement this, or implement it only for transaction messages (which is the only case that I'm aware of anyone actually using it for something plausibly useful). --Greg Maxwell, 2017-03-14

