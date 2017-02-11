The change at `version >= 106` and the later addition of the relay field both created a compatibility break with previous protocol versions. The standard should not assume that old versions of the protocol will not continue to operate indefinitely. This problem should be avoided in any future changes.

Because this message begins the handshake it must be valid for both peers **of any version**. Because the message does not provide a mechanism for nodes to determine the proper length of the message, older protocol nodes can:

1. Fail the handshake for any version message that is not valid according to its understanding.
2. Allow a message of any length, ignoring trailing bytes.
3. Have clairvoyance about future changes.

The first option will result in the old node being isolated from the network as other peers upgrade. The second option opens a DOS vector (and would have been inconsistent with the protocol). The third option is not possible. At this point older nodes that were properly implemented are broken, since the changes were not backward compatible. Patching such older nodes in order to make them compatible is the only option. In other words, this sort of incompatible change forces an upgrade on existing nodes, which should be avoided. Additionally, properly implemented nodes in the future may prevent such a change from gaining any traction on the network (due to dropped connections under the first option above).

New (or patched) clients operating **below** either of the protocol breaks described above must be hacked to allow the largest possible valid version message (at the current time) despite not having knowledge of what that is (which is what makes it a hack).

--Eric Voskuil, 2017-02-11

There is an inconsistency between these two statements:

> since version >= 70001

and

> Additionally the protocol version is increased from 70001 to 70002.

This is further confused by the fact that the original satoshi client change was inconsistently implemented. Protocol version 70001 reads, but does not write, the relay field. Protocol version 70002 reads and writes the relay field. As these clients are still operating on the network, newer clients must accommodate this behavior in order to prevent isolating the older clients.

--Eric Voskuil, 2017-02-11