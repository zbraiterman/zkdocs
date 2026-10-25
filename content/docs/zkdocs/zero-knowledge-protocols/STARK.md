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
A STARK-style proof can be understood through the following conceptual sequence. Actual STARK implementations use specific trace encodings, commitments, low-degree tests, and query procedures.

{{< rawhtml >}}
 $$
 \begin{array}{c}
 \work{\varprover}{\varverifier}
 \alicework{Construct}
 \alicework{\text{Low degree test}}{T}\ 
 \alicework{\text{Commit to encoded trace} \rightarrow \text{Commitment} \leftarrow \text{Challenges Construct proof responses} \rightarrow \pi}
 \bobwork{\text{Derive random challenges}}
 \bobwork{\text{Verify proof } \pi \text{ Accept or reject}}
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
 {\varverifier} &:\quad
 \operatorname{Verify}(\vec{x},y,C_T,\pi)
 \end{aligned}
 $$
{{< /rawhtml >}}

Here, $C_T$ is a commitment to the encoded execution trace, $H$ is a cryptographic hash function, $\alpha$ represents a challenge, and $\pi$ is the proof.

This is illustrative pseudocode in mathematical notation; real STARKs typically derive multiple challenges from a transcript containing multiple commitments and protocol messages.

#### Verification requirements
The verifier accepts only if the proof satisfies the protocol's checks:
$$
\boxed{
\varverifier(\vec{x},y,\pi)=\text{accept}
}
\
$$

The intended security properties are:
* __Completeness:__ A valid computation is accepted with high probability.
* __Soundness:__ A false statement is accepted only with negligible probability.
* __Zero knowledge:__ The proof reveals no additional information about the private witness beyond what the public statement implies.

Together, these properties express the goal of a zk-STARK: convince the verifier that the claimed computation is valid without exposing the private witness or requiring full recomputation.

## Security pitfalls
* __Incorrect Fiat-Shamir Transformation:__ The Fiat-Shamir transformation replaces interactive verifier challenges with challenges derived from a cryptographic hash of the protocol transcript. If any commitment, public input, or prover message that influences verification is omitted from the transcript, an attacker may be able to manipulate the challenges and construct an invalid proof that is accepted by the verifier. All relevant transcript elements must be included in the correct order, with unambiguous encoding and domain separation.
* __Unsound Computation Constraints:__ A STARK proves that a computation satisfies the constraints encoded by its algebraic representation. If these constraints fail to enforce the intended computation, the prover may produce a valid proof for an incorrect result. For example, an omitted range check or an incorrectly specified transition constraint may allow an invalid execution trace to satisfy the proof system. The constraint system must accurately represent the intended computation.
* __Incorrect Execution Trace Validation:__ STARKs commonly use an algebraic intermediate representation, such as an Algebraic Intermediate Representation (AIR), to express valid execution traces. If the constraints do not correctly enforce the initial state, transition rules, boundary conditions, or final output, a malicious prover may construct a trace that passes verification without representing a valid execution. Each required property of the computation must be explicitly enforced.
* __Insufficient Soundness Parameters:__ STARK security depends on parameters such as the number of queries, the size of the evaluation domain, the field size, and the error probability of the low-degree testing procedure. Inadequate parameters may increase the probability that a malicious prover can convince the verifier of a false statement. Security parameters must be selected according to the protocol's security analysis and the desired soundness level.
* __Incorrect Low-Degree Testing or FRI Verification:__ STARKs commonly use a low-degree testing protocol, such as the Fast Reed-Solomon Interactive Oracle Proof of Proximity (FRI), to establish that committed evaluations are consistent with low-degree polynomials. Errors in folding operations, query indexing, Merkle authentication paths, or degree-bound enforcement may cause invalid proofs to be accepted. Implementations must verify every required consistency and authentication check.
* __Faulty Polynomial and Field Arithmetic:__ STARKs rely on arithmetic over finite fields and polynomial identities. Incorrect modular reduction, overflow, inconsistent field-element encodings, or mistakes in polynomial evaluation can cause the prover and verifier to evaluate different mathematical expressions. Arithmetic operations and field-element encodings must follow the protocol specification exactly.
* __Broken Commitment or Merkle-Tree Verification:__ STARK implementations commonly commit to execution traces and polynomial evaluations using cryptographic hash functions and Merkle trees. If a commitment is not binding, or if the verifier incorrectly checks authentication paths, an attacker may substitute values that were not part of the original commitment. The commitment scheme must be collision resistant, and every required authentication path must be validated.
* __Loss of Zero-Knowledge:__ A STARK does not automatically guarantee that every implementation hides the witness. Depending on the construction, insufficient masking, inappropriate randomness, or accidental disclosure of intermediate values may reveal private information. The zero-knowledge construction must use the required masking mechanisms and randomness, and the implementation must not expose private witness data through the proof or auxiliary outputs.
* __Weak or Reused Randomness:__ Randomness may be required for witness masking, polynomial masking, and other protocol operations. Predictable or improperly reused randomness can compromise privacy or invalidate the security assumptions of the construction. Implementations must use a cryptographically secure random number generator where required and follow the protocol's randomness-generation requirements.
* __Incorrect Proof Parsing and Verification:__ Proofs are untrusted inputs. Incorrect length checks, integer decoding, bounds checks, or handling of malformed proof elements may cause verifier crashes, denial of service, or incorrect acceptance decisions. The verifier must validate proof structure, enforce size limits, reject malformed inputs, and fail closed whenever a required check fails.
* __Implementation Side Channels:__ Even when the mathematical proof system is secure, implementation behavior may leak private information. Timing differences, memory-access patterns, error messages, or other observable behavior may reveal information about secret witness data. Implementations handling sensitive data should assess relevant side-channel risks and use appropriate mitigations.
* __Incorrect Public Statement Binding:__ The proof must be bound to the exact public inputs, claimed output, and computation being verified. If the verifier does not correctly bind these values to the proof, a valid proof may be reused in an unintended context or accepted for a different statement. Implementations must bind all relevant public inputs and context to the verification procedure and transcript.