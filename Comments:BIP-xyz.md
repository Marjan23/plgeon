I already reported on #lightning-dev but worth documenting here in case anybody else is playing around with it, this blank `prevout` at 
https://github.com/cdecker/bitcoin/blob/noinput/src/script/interpreter.cpp#L1217  
is used when `SIGHASH_NOINPUT` is the sighash type, but the input index that's being set is actually `0xFFFFFFFF` instead of `0x00000000` as the bip suggests, can be seen here :  
https://github.com/cdecker/bitcoin/blob/noinput/src/primitives/transaction.h#L24  
Something like the following will have to be added for the implementation to be compatible with the bip
```diff
diff --git a/src/script/interpreter.cpp b/src/script/interpreter.cpp
index 3df1fc0c9..43e398581 100644
--- a/src/script/interpreter.cpp
+++ b/src/script/interpreter.cpp
@@ -1230,6 +1230,8 @@ uint256 SignatureHash(const CScript& scriptCode, const CTransaction& txTo, unsig
         if (!noinput) {
             prevout = txTo.vin[nIn].prevout;
             script = scriptCode;
+        } else {
+            prevout.n = (uint32_t)0;
         }

         if ((nHashType & 0x1f) != SIGHASH_SINGLE && (nHashType & 0x1f) != SIGHASH_NONE) {
```