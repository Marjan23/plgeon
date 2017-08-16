In the scripts in this proposal, pubkey hashes are used for the buyer and seller; is there an advantage over using just pubkeys? I can see only a disadvantage, i.e. increased space usage in redeeming.

~~ Adam Gibson

==========

Should directly use pubkey, instead of pubkey hash. Using pubkey hash will require more witness space for no benefit. The following script should consume the minimal space:

    [HASHOP] <digest> OP_EQUAL
    OP_IF
        <seller pubkey>            
    OP_ELSE
        <num> [TIMEOUTOP] OP_DROP <buyer pubkey>
    OP_ENDIF
    OP_CHECKSIG

~~Johnson Lau

==========
