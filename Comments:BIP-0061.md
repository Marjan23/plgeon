[BIP61 states](https://github.com/bitcoin/bips/blob/master/bip-0061.mediawiki#Compatibility):

> The reject message is backwards-compatible; older peers that do not recognize the reject message will ignore it.

This is an incorrect assumption. Peers of older versions should (and some do) validate the protocol against invalid messages. Sending a reject message when the peer has announced a version below 70002 should not be allowed by the protocol as it is not backward compatible.

It is also incorrect to assume that older protocol versions will cease to exist. It is perfectly reasonable for ancient versions of protocols to continue to operate. These protocols do not have expiration dates. New implementations can and do support configurable lower protocols levels than 70002.

If this precedent is allowed to hold the network is subject to trivial DOS attacks. The requirement to treat invalid messages as valid precludes a node from dropping the peer due to incorrect behavior. This statement should be struck (and offending implementations should be fixed so that they do not intentionally send invalid messages).

--Eric Voskuil, 2017-02-07