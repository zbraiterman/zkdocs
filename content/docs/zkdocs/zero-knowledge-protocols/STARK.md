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

{{< tabs "uniqueid" >}}
{{< tab "Multiplicative notation" >}}
### Interactive protocol
A STARK-style proof can be understood through the following conceptual sequence. Actual STARK implementations use specific trace encodings, commitments, low-degree tests, and query procedures.

{{< rawhtml >}}
 $$
 \begin{array}{c}
 \work{\varprover}{\varverifier}
 \alicework{Construct}{T}
 \alicework{Commit to encoded trace}
 \end{array}
 $$
{{< /rawhtml >}}

The verifier checks the proof against the public statement and the protocol's verification rules rather than re-executing the original computation.

-----

### Non-interactive protocol
Using the Fiat–Shamir transformation, the prover derives challenges from a cryptographic hash of the transcript instead of receiving them interactively from the verifier.
{{< rawhtml >}}
 $$
 \begin{aligned}
 T &\leftarrow \operatorname{Execute}(F,\vec{x},\vec{w})\\
 C_T &\leftarrow \operatorname{Commit}(T)\\
 \alpha &\leftarrow H(\vec{x},y,C_T)\\
 \pi &\leftarrow \operatorname{Prove}(T,C_T,\alpha)\\
 &\\
 \mathcal{V} &: \quad
 \operatorname{Verify}(\vec{x},y,C_T,\pi)
 \end{aligned}
 $$
{{< /rawhtml >}}
{{< /tab >}}