# Stunc

Stunc, also known as the Studio Universal Naming Convention, is a variation of the standard UNC proposed by developers at Script-Ware V2 in an effort to increase exploit compatibility in scripts.

While many great things came out of this new standard, a lot of others have been lacking in recent times, such as the refusal by some developers to innovate and develop the standard further.

At RbxStu V3 we wish to try and change the mindset by proposing small, yet manageable changes to UNC, in the hopes of partially detaching or innovating on what is already available. Please remember, however, that suggestions are ALWAYS open in our project's Discord server. The spec is in constant development with the little time that the developers can allocate, and all of its assets are present on this web page.

The Stunc spec also contains its own experimental test, inspired heavily by the SUNC test, an alternative to the generic UNC test, which boasts a great improvement over the original test script, as it has started to be faked, which has caused exploiters to put efforts into making hard-to-fake scripts, such as SUNC. However, Stunc will most likely not suffer such an issue, as the API would be exclusive to RbxStu V3, as there is (as of 30/12/2024) no other Studio executor publicly released as it is.

This documentation will provide:

* Remarks on Function
* Aspects such as if it is pure/has no side effects
* If it errors
* Alternative APIs
* Low-Level Overview (Low-Level Aspects)
* High-Level Overview (High-Level Aspects)

Some structures may have slight differences from the standard UNC ones; the Stunc WebSocket, as it stands currently, is not implemented into RbxStu V3. However, its key difference would be marked by the existence of a `:Connect` connection on the constructor, which may lead to some messages being lost in the initial connection, which is most undesirable.
