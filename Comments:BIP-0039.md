# Electrum criticism of BIP39

Source: http://docs.electrum.org/en/latest/seedphrase.html

BIP39 was introduced two years after Electrum. BIP39 seeds include a checksum, in order to help users figure out typing errors. However, BIP39 suffers the same shortcomings as early Electrum seed phrases:

* A fixed wordlist is still required. Following our recommendation, BIP39 authors decided to derive keys and addresses in a way that does not depend on the wordlist. However, BIP39 still requires the wordlist in order to compute its checksum, which is plainly inconsistent, and defeats the purpose of our recommendation. This problem is exacerbated by the fact that BIP39 proposes to create one wordlist per language. This threatens the portability of BIP39 seed phrases.

* BIP39 seed phrases do not include a version number. This means that software should always know how to generate keys and addresses. BIP43 suggests that wallet software will try various existing derivation schemes within the BIP32 framework. This is extremely inefficient and rests on the assumption that future wallets will support all previously accepted derivation methods. If, in the future, a wallet developer decides not to implement a particular derivation method because it is deprecated, then the software will not be able to detect that the corresponding seed phrases are not supported, and it will return an empty wallet instead. This threatens users funds.

For these reasons, Electrum does not generate BIP39 seeds.

# Entropy requirements not well defined

Author: Liraz Siri

BIP39 requires a minimum 128-bits of entropy. Some people are suggesting this means deterministic Wallet creation procedures cannot output BIP39 because the user may provide less than 128 bits of entropy (e.g., in a passphrase). Another problem is that what constitutes true entropy in this context is not well defined.  You can verify conformity to mnemonics and checksums, but it's really hard to verify how much source entropy is in the process that generates the 128/256 bits you feed into a BIP39 compliant generation procedure. A CSPRNG is not necessarily better than a user supplied passphrase fed into a KDF, and may be worse. It depends on the amount of source entropy that goes into the CSPRNG and whether the CSPRNG is operating correctly, which is hard to verify.

