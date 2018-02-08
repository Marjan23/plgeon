***

Mandatory use of public derived keys is unsafe for any software which supports private key export and will result (has resulted) in funds loss. The lack of hooks for versioning is poor for future extensibility (though this could be handled at a higher level). I normally recommend against implementing this except for hardware wallets. -- Greg Maxwell 2017-03-14

***

I believe that this BIP should be on the Process (or Informational) Track rather than the Standards Track as per Greg Maxwell's comment on 2017-03-14. One thing I think that Greg missed is that this BIP does not propose a "change that affects most or all Bitcoin implementations, such as a change to the network protocol, a change in block or transaction validity rules, or any change or addition that affects the interoperability of applications using Bitcoin" as stated in [BIP-0002](https://github.com/bitcoin/bips/blob/master/bip-0002.mediawiki#BIP_types).

There are times when JBOK wallets may be preferred over HD wallets. Though, I think it is important to note that funds can be lost using JBOK software wallets, like the current BTC reference client, if they have key export functions as well. There are just more keys to export than an HD wallet.

I would also like to state that it is possible to store the master key on a hardware wallet while distributing individual account keys to their proper owners, who may or may not store it on a hardware wallet, without risking the master key. Thus it is possible to secure the master key while allowing individual accounts to use software wallets.

HD wallets, like this, can also help eliminate the blockchain bloat caused by 1-of-n multi-sig wallets by eliminating some of the need for them. The same effect can be achieved by having n+1 levels, with the keys of levels 0 through n-1 controlled by a different person. Anyone having a key for any level between 0 and n-1 can spend anything in levels n onward, so rather than 100 1-of-n multi-sig accounts being established to support n account holders, a single chain can be established with n levels each corresponding to a different account holder and 100 standard addresses established at level n.

That aside, here are the changes I would make to this BIP:

1. Move the BIP to either the Process Track or the Informational Track.
2. Clearly state that the master key needs to be protected and, ideally, should be stored on a hardware wallet.
3. Talk about the possibility of having a different owner for each account, each of which may use their preferred wallet software or hardware.
4. Swap the position of the "coin_type'" and "account'" levels so that a single account can hold any coin they want.
5. Add a 6th level before the "coin_type'" and "account'" levels for "revision_number'".
6. Allow for the possibility of sub-accounts specific to the organization using the wallet such that there are levels "account_0'" through "account_n-1'" where n is the number of account levels.
7. Add a 7th level between "revision_number'" and "account_0'" labeled "n'" to specify the number of account levels in the tree so searching is more efficient.
8. A recovery process should be clearly defined.

Based on suggestions 4-7, the path levels would for this BIP would be redefined as such:

`m / purpose' / revision_number' / n' / account_0' / account_1' / ... / account_n-1' / coin_type' / change / address_index`

Organization should clearly define their "n'" value and enforce its use. If the "n'" states that there should be 10 levels and a particular account at the "account_1'" level, only needs one sub-account, it should still complete the tree down to the "account_n-1'" level and generate the levels from "coin_type'" onward from their "account_n-1'" to meet with their organization's standards and make account recovery from the master key easier.

During the recovery process, the stop limit at each account level is 20 by default, meaning that if there are 20 consecutive "account_n-1'" accounts in an "account_n-2'" account, each with 20 empty reviving addresses for all of the organization's "coin_types'", then it is considered the last "account_n-2'" account and the next "account_n-3'" account is searched until the last "account_0'" account is found.

Overall, I think this is a good BIP, it just needs a different track and some revision. -- Colin M. Lacina 2018-02-07

***

