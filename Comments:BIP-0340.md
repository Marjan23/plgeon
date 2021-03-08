### Major Request (Urgent): This BIP Should Be Canned

The primary reason for introducing Schnorr's is to get around the weaknesses in the random variable construction for the sake of the proof which secp256k1 must generate in order to provide a unique signature for a message that's later authenticated. 

The supposed benefits of this implementation are: 

1. Greater obfuscation of transaction source since the aggregate signatures required to spend are not revealed, but rather they act as a 'threshold' (similar to Shamir's Secrets), whereby many parts (keys) are used to construct just one signature. This ultimately obfuscates the fact that there was more than one signature involved. Although, one could argue that a normal wallet could be used with a similarly outfitted scheme using BLS Signatures (with 'decentralized key generation'). ```One key(representing the group), vs. x; given x = m in the m-of-n scheme```. 

2. Following from the idea in #1, potentially smaller transaction sizes. Although, the recently revised whitepaper on Schnorr's by Blockstream states that a 3-round MuSig will be opted for vs. 2-rounds; not exactly clear on what that means for the size of the transaction (especially given the fact that Bitcoin has no "state" apart from the cached stacks during the stack execution of transactions) 

3. An imperviousness to malleability (assumed; as detailed by the Core developers)

### ECDSA (secp256k1) is Wearing Out its Welcome on Bitcoin and Elsewhere

This is troublesome for a countless # of reasons, but adding Schnorr in the manner this BIP proposes, will only incur greater technical debt on the protocol by addressing one of the symptoms (malleability), but not the root cause (non-deterministic nonces + a failure to provide a cohesive, ecosystem-wide standard for the instantiation of address signing). 

The solution, thankfully, is to replace the random oracle construction withe a deterministic one. 

The best thing for Bitcoin to do (before anything else), would be to either swap out the secp256k1 construction altogether for an Edwards' Curve instantiation or make secp256k1 RFC6979 compliant (that IETF outlines the proper derivation). 

### Broader Cryptography Community Appears to Agree With This Sentiment 

Over the last few years various pillars of the internet as well as within the greater IT space have begun to phase out the NIST curves (which Bitcoin currently uses), in favor of the Edwards' Curve implementation. Of course, this is due in a large part to the increased global distrust of cryptographic primitives deriving from the United States mainland since the revelation that a U.S. intelligence agency had placed a purposeful backdoor into secp256r1 in the form of a 'trapdoor' function. 

However, the above aside. None of what is being written here is motivated by any desire to evict the NSA's from the project (especially since SHA256 is an NSA construction as well). 

**Evidence of Cryptographic Authorities Giving the Nod to Edwards' Curves**

The world's foremost cryptographer, Daniel J. Bernstein, has advocated numerous times for the use of Edwards' Curves (ed25519) in favor of Elliptic Curve algorithms. Even though Bernstein is the originator of the Edwards' Curve, it is worth noting that it is not burdened with many  of the same issues that Elliptic Curve cryptography has, and will continue to face due to the differences in the way that the x, y (private / public) key pairs are generated. 

Recently, the NIST posted a public RFC (request-for-comments) soliciting opinions on their tentative induction of "Ed25519 and Ed448, for use with EdDSA" for their augmented FIPS 186-5 cryptographic standard. [1]

For any other observers that may be a little confused here: 

- The 'NIST' stands for a U.S. governmental body known as the "National Institute of Standards and Technology". For reference, this is the same body that helped user in cryptographic standards such as SHA-2 (merkle-damgerd construction), which Bitcoin currently uses as well as AES (Rindjael, specifically), which is by far the most dominant version of AES out there on the market currently ("Serpent" was the runner-up; fyi) 

-  The 'FIPS' stands for the "Federal Information Processing Standards". This register is generally taken pretty seriously, since most information-sensitive industry in the private sector must adhere to these standards (with all public sector employees being forced to do so as required). The FIPS standards are of immeasurable importance since they dictate the way that mission-critical information must be cryptographically secured, encrypted or otherwise protected. 

#### Main issue Here is With the 'Random' Oracle Mandate for ECDSA Signatures 

Putting Schnorr to the side for a second, Schnorr does not **solve the major issue here with ECDSA**, which is the **random oracle** construction that this design mandates for each unique signature generated with the curve. 

The general formula for producing an ecdsa signature is `${s = k^{-1} * (h + rx)\pmod n}$`

With the variables above mapped as the following: 

- 'x' is the private key coordinate point for the (private / public key pairing derived from the secp256k1 operation) 

- 'k' is a random value between {1 ... n-1}

- 'r' is a random point on the curve that is selected by taking the random variable we identified above (k) and multiplying it by the generator point G (this point 'G' is already well-known) ; thus ${r = kG}$ 

- ${r=kG}$ determine the multiplicative scalar value for which 'x' will be multiplied  to find another point on the curve for the generated proof ('s' variable); this is represented by the ${rx}$, which is then XOR'd with the hash of the message (hash pre-determined ; of course in this context it is SHA256)

The primary issue here in relying explicitly upon the strength of the 'randomness' (so to speak) of the generated variable ('k'), is that it essentially force the protocol to hinge on having a high-quality means to create / generate "randomness". 

#### Research Strongly Backs These Ideas as Well 

In 2019, researchers Joachim Breitner and Nadia Heninger, published an enlightening piece about specific weaknesses of ECDSA signatures produced by cryptocurrencies. The paper, titled, 'Biased Nonce Sense: Lattice Attacks Against Weak ECDSA Signatures in Cryptocurrencies', was published in the peer-reviewed International Association of Cryptologic Research. [2]

The paper's Abstract opens with the following: 

> "In this paper, we compute hundreds of Bitcoin private keys and dozens of Ethereum, Ripple, SSH , and HTTPS private keys by carrying out cryptanalytic attacks against digital signatures contained in public blockchains and Internet-wide scans." 

From there, the Abstract points out the primary culprit that allowed this level of cryptanalysis possible, stating: 

> "The ECDSA signature algorithm requires the generation of a per-message secret nonce. If this nonce is not generated uniformly at random, an attacker can potentially exploit this bias to compute the long-term signing key."

The researchers credit "implementation vulnerabilities" as being the reason for the "biased nonce". 

**Most Startling Revelation Made By the Researchers** 

Their results were not hypothetical. Nor did they rely on hypothetical collisions. 

Specifically, the research states, "While the hidden number problem is a popular tool in the cryptanalytic literature for recovering private keys based on side channel attacks, to our knowledge we are the first to apply these techniques to already-generated keys in the wild, and the first to observe that these techniques may apply to signatures in cryptocurrencies." 

Which leads to the following unnerving statement, **"In total, we computed around 300 Bitcoin keys with these techniques. As of this writing, 818975 satoshis, or around $54, and 30.40 XRP, or about $14, remain in Bitcoin and Ripple accounts whose keys we were able to compute, suggesting that these flaws do not yet appear to be known, or else the funds would have already been stolen."**

The paper goes on to state that the nonce vulnerabilities fall into several classes that suggest "several independent implementation vulnerabilities". 

What's even worse is that the researchers also claim that they have, "**[Found] evidence that othert attackers are already emptying the accounts of cryptocurrency users whose keys are revealed through known vulnerabilities** (both repeated nonces and private keys posted online)", that they "anticipate that users will be affected once knowledge of this flaw becomes public."

Curiously, these researchers claim that they have also attempted to "disclose flaws to the small number of parties" they were able to identify but that "in most cases" they were unable to identify anyone whom they could "**responsibly disclose to**". 

Despite the depressing reality the research above paints for us - all is not lost. 

The researchers cap off these observations by stating, unequivocally, that "**All of the attacks we discuss in this paper can be prevented by using deterministic ECDSA nonce generation**". 

### LadderLeak Research Paper Released in 2020 Claiming to Have Found an Extremely Efficient Way to Recover Private Keys

Toward the back half of 2020, researchers published a paper describing an attack they termed, 'LadderLeak', in which they claimed they were able to "break ECDSA" with "less than one bit of nonce leakage". Specifically, they stated, "in principle, the vulnerability affects various curve parameters in the above implementations, including NIST P-192, P-224, P-256, P-384, P-521, B-283, K-284, K-409, B-571, sect163r1, secp192k1, secp256k1", meaning that the elliptic curve that Bitcoin uses was among those susceptible to such an attack. 

Here is the link to said paper = https://dl.acm.org/doi/10.1145/3372297.3417268

1. https://csrc.nist.gov/publications/detail/fips/186/5/draft

2. https://eprint.iacr.org/2019/023.pdf


----

The comments above appear to be misinformed. Nearly all ECDSA implementations today, including all the ones used in Bitcoin software that I know about, already use derandomized RFC6979 nonce generation for secp256k1. BIP340 too specifies such a nonce generation algorithm - one that is inspired by Ed25519's in fact. It permits adding randomness as research has shown this improves resistance to certain fault & side-channel attacks, but the randomness is not critical for security (it is purely additive). Lastly, the entire nonce generation concern is an implementation aspect that's orthogonal for the signature scheme - it can be done well, or badly, either with ECDSA or Schnorr/BIP340. BIP340 chooses to specify it, in the hope that implementations adopt it as a best practice, but circumstances may call for alternative nonce generation algorithms too. -- Pieter Wuille, 2021 Mar 07.