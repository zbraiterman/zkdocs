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
$\varprover$ convinces $\varverifier$ that there exists a private witness w such that F(x,w)=y, without revealing w or requiring $\varverifier$ to re-execute the computation.
{{< /hint >}}