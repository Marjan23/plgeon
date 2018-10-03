Regarding the future of the Lightning Network I like that BIP. However I could not find a link to an implementation. Where can I find it?

(above comment from @renepickhardt )

***

I notice that the nSequence for the input under consideration is kept inside the signed data. Interesting to consider the implications for Eltoo:

* If nSequence is signed, then the `10 OP_CSV` in the Eltoo paper is unnecessary since both of the settlement signatures will be already enforcing a specific maturation delay using BIP68.
* If nSequence is unsigned, then the `10 OP_CSV` is needed. But, perhaps leaving nSequence unsigned opens the door for more flexibility in other smart contract schemes.

Cheers - @markblundeberg