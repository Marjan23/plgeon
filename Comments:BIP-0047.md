Poorly designed. I recommend against implementing this version. --Luke Dashjr, 2016-12-23

I support the goal. But this proposal is not well designed, and the way that it has been implemented in practice is by clients sending their scanning codes to third party servers; linking all of their payments-- a design flaw that was called out early in this (and related proposals lives). --Greg Maxwell, 2017-03-14

No comment as to design. However, I think that there is a typo in the Identity section. A sentence reads 
"For example, the payment code created represented by (m / 47' / 0' / 0') is part of the account represented by (m / 44' / 0' / 0')."
I think that the account should not have a hardened final key pair. It should read "(m / 44' / 0' / 0)" instead.
-- Paul Imthurn, 2018-8-15