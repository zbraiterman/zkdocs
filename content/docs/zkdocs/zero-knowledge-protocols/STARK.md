---
weight: 8
bookFlatSection: true
title: "Zero-Knowledge Scalable Transparent Argument of Knowledge"
summary: "A cryptographic proof system that lets one party prove a computation was done correctly without revealying the underlying private data."
needsVariableResetButton: true
references: ["stark"]
---
# zk-STARK
A zk-STARK (Zero-Knowledge Scalable Transparent Argument of Knowledge) is a cryptographic proof system that lets a prover convince a verifier that a computation was executed correctly without revealing private data or re-executing the code.

{{< hint info >}}
**Goal:**
$\varprover$ convinces $\varverifier$ that $\exists, \vec{w}: C(\vec{x}, \vec{w}) = 1$.
{{< /hint >}}

Here, the symbols mean:
* __$\vec{x}$:__ the public input, known to both the prover and verifier.
* __$\vec{w}$:__ the private witness, known to the prover but not disclosed to the verifier.
* __$C$:__ the computation or constraint system being verified.
* __$C(\vec{x}, \vec{w}) = 1$:__ the public input and private witness satisfy the required correctness conditions.

### Interactive protocol
The STARK interactive identification protocol

{{< rawhtml >}}
 $$



{{< /rawhtml >}}