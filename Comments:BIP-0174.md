# Comments for the Partially Signed Transaction Format

* Partial signature entries use the map key `{0x02}|{public key}`.  However, a single public key may be involved in signing multiple inputs, and the signature will differ in each one.  Since the map keys are to be unique, this is a problem.  I suggest the following map key instead: `{0x02}|{input index}|{pubkey index}`

* HD derivation entries waste space by repeating the public key.  I recommend using the following map key instead: `{0x03}|{input index}|{pubkey index}`, which also matches previous point.

## In response to Dev Random from achow101

> However, a single public key may be involved in signing multiple inputs, and the signature will differ in each one.  Since the map keys are to be unique, this is a problem.

Each input has its own partial signatures map; it isn't global.

> HD derivation entries waste space by repeating the public key.  I recommend using the following map key instead: `{0x03}|{input index}|{pubkey index}`, which also matches previous point.

The hd keypaths (unlike the partial signatures map) is global. Public keys are not duplicated in the map.

## Further response from devrandom

I understand the response to the first question.

As to the second question - the public key is already present in the redeem script, so it does not seem necessary to duplicate it in the map key.  Instead, could refer to the public key index within the redeem script.
