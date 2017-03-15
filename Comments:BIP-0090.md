Should be harmless and eliminates some technical debt. Recommended. --Luke Dashjr, 2016-12-23

This is actually the opposite of [technical debt](https://en.wikipedia.org/wiki/Technical_debt):

> Technical debt (also known as design debt[1] or code debt) is "a concept in programming that reflects the extra development work that arises when code that is easy to implement in the short run is used instead of applying the best overall solution".

The development work was already complete and working in all functional implementations of Bitcoin. This change on the other hand is a regression to simpler-to-code yet clearly imperfect implementation - "used instead of [retaining] the best overall solution". Ironically it also creates **more** work for core Bitcoin developers at this point.

It would also be incorrect to consider it a material performance optimization (the true target of this fork). While certain implementations may fail to cache the small amount of necessary block version history, that deficiency is not inherent in the consensus rules affected by this proposal. In that sense this is a shortcut in place of, "applying the best overall solution" - in other words, it creates technical debt.

--Eric Voskuil, 2017-01-21

BIP 0090 is a straightforward proposal which objectively simplified Bitcoin Core and improved its performance. The above criticism by Voskuil is almost completely non-responsive to the proposal itself, instead choosing to invite a definition argument about "technical debt".

--Greg Maxwell, 2017-03-14 

Maxwell's claim that the criticism is non-responsive to the proposal is incorrect. This proposal is not necessary to achieve the stated performance benefit objective. This is clearly stated above and was explained in detail on bitcoin-dev, and stands unchallenged. A meager amount of cache can achieve the same advantage with little effort or complexity (which has been done). The fact that this proposal "simplified Bitcoin Core" is irrelevant. This is not a Bitcoin Core proposal, it is a Bitcoin proposal. --Eric Voskuil, 2017-03-15
