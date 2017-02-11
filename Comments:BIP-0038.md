Designed for single-key "paper wallets", which are not a good idea in general. --Luke Dashjr, 2016-12-23

I assume this is not meant as a general recommendation against "paper wallets", but rather paper wallets that consist of a single key?  If so, is there an equivalent BIP (or discussion) on passphrase-protected HD paper wallets? --Jonathan Cross, 2017-01-17

The design may have been intended for paper wallets, although there is nothing inherently bad about either a paper wallet or encryption of a single key. I would agree that a "single-key" *wallet* is a bad idea from a privacy perspective, but there are other perfectly "good" scenarios for encryption of a single secret. The are however **significant** security problems with one aspect of BIP38. The issue is documented in detail [here](https://github.com/libbitcoin/libbitcoin/wiki/BIP38-Security-Considerations). --Eric Voskuil, 2017-01-21

Thank you Eric.  It seems the security issues only relate to the [Confirmation Code](https://github.com/bitcoin/bips/blob/master/bip-0038.mediawiki#confirmation-code) section, correct? Are there security issues with owner-created, single-use, BIP-38 encrypted key pair when create and printed on a secure platform?  --Jonathan Cross, 2017-02-04

That is correct. In implementing all aspects of BIP38 (and BIP39) in the libbitcoin library, and providing a command line interface via bx, we had a chance to thoroughly review the scenarios. The confirmation code scenario is inherently flawed and the entire section should be struck. The other scenarios appear to be secure to the extent one trusts the platform and the cryptographic assumptions. The former is up to the user and I'm not qualified to asses the latter.

In response to your initial query, backing up an HD wallet consists of securing the master private key, which this BIP accomplishes. It is useful in the case where one must secure any *existing* private key. If one intends to create a new HD wallet BIP39 may be a better option. It accomplishes the objective of securing the master public key (which it generates) but cannot secure an existing key.

--Eric Voskuil, 2017-02-11