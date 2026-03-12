Below is a structured summary of the driving motivation behind this research. Verkle has significant benefits over the alternative storage proposals. Nevertheless, it has some drawbacks too like Pedersen Hash speeds and no PQ-security.

Thus, one area of research that is active and stateless consensus is collaborating on together with EF Cryptography team is PQ-Verkle trees.
You can find more info about the initiative in the following post: ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001)).

The rest of the page contains a summary of the post and proposal.

---

## 1. Why Post-Quantum Verkle?  
- **Quantum threat to ECC:**  Verkle trees (and many ZK-EVM designs) rely on short, homomorphic EC commitments (e.g. Banderwagon/Bandersnatch).  Shor’s algorithm would break these once large quantum computers arrive.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))  
- **ZK-friendliness:**  Emulating “foreign-field” arithmetic (e.g. arithmetic over an elliptic-curve base field) inside SNARKs/STARKs is *extremely* expensive.  Each lookup or permutation argument ends up requiring complex field per-gate emulation.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))  
- **Stateless Ethereum goals:**  To push toward verifiable stateless clients with high gas limits, we need a tree commitment with  
  1. **Post-quantum security**,  
  2. **Small opening proofs**,  
  3. **Aggregatable openings** (to compress many proofs into one),  
  4. **Transparent setup**,  
  5. **Hardware-feasible performance**.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))

---

## 2. Why Not Standard Merkle/Verkle?  
- **Binary hash trees**: Depth ≈32 for a unified state → ~1 kB per leaf proof.  At ~6 000 updated leaves/block, verifying 2²⁰ proofs in a single block is untenable.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))  
    ```sage
    numerical_approx(log(6000*256,2))
    # → 20.55  ⇒ ~2^20 opening verifications
    ```  
- **Loss of “differential updatability”**:  Recursively aggregating Merkle openings (no homomorphism) forces per-leaf checks, killing disk-IO performance.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))

---

## 3. Desirable Vector-Commitment (VC) Properties  
1. **Small opening proofs** (ideally ≪1 kB per leaf)  
2. **Succinct/aggre­gatable**: support *folding* many openings into one proof  
3. **Transparent, no trapdoor**  
4. **Low-overhead verification** (linear or quasi-linear in the tree arity)  
5. **Constant-size commitments** (~32 bytes each)  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))

---

## 4. Leading PQ-VC Proposals

| Scheme & Link                          |      Setup       |      Comm.      |  Proofs   | Notes                                                                                                                                                                                                                                         |
| :------------------------------------- | :--------------: | :-------------: | :-------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PPS21**<br/>(ePrint 2021/1254)       | Trusted trapdoor |    Moderate     |   ≥O(n)   | Builds on SIS; compact vs older—but needs authority setup  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001)) |
| **PCC2024**<br/>(ePrint 2024/281)      |   Transparent    |    Moderate     |   ≥O(n)   | First practical, but too large for vector arity 256–1024  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))  |
| **Nguyen-2022**<br/>(ePrint 2022/1368) |   Transparent    | Small (~log² n) | O(log² n) |
  - Supports *stateless updatability*  
  - Very small proofs for ≤1024 vectors  
  - Linear-time verifier is acceptable for arity ≤1024  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001)) |
| **Greyhound**<br/>(ePrint 2024/1293) | Transparent | Tiny (kB) | O(√n) |  
  - Best proof sizes & fast verify  
  - *Unknown* if stateless-updatable without large trade-offs  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001)) |

> **Key diagram (Nguyen-2022 comparison)**  
> *(tables showing removal of n² terms and linear proof-size growth)*

---

## 5. Aggregation & Accumulation  
- **LaBRADOR** (lattice PIOP): can batch openings into sublinear proofs, but the implementation is *extremely* complex.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))  
- **WHIR** (hash-based PIOP): yields ≤100 kB proofs, sidesteps need for homomorphic VC—proof-aggregation shifts to the hash-proof layer.  
- **Additive-homomorphic “Multiproofs”**: ideal: aggregate all per-level VC openings into one constant-size proof, analogous to BLS signature aggregation.  *Open question* if any PQ-secure VC admits this directly.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))

---

## 6. Differential Updatability  
> *“A must-have feature.”*  
Only a few lattice-based VCs (notably Nguyen-2022) support updating individual vector slots without rewriting the whole proof.  This enables efficient stateless block-propagation.  ([A short note on Post Quantum Verkle explorations - Execution Layer Research - Ethereum Research](https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001))

---

## 7. Next Steps & Why It Matters  
1. **Deep-dive** into the Nguyen-2022 SIS-based scheme (ePrint 2022/1368):  
   - Verify proof-size vs arity trade-offs  
   - Confirm differential-updatability performance  
2. **Evaluate WHIR vs VC-native aggregation**:  
   - Measure proof-size & CPU for typical ~6 000 updates  
3. **Benchmark ZK-VM friendliness**:  
   - How costly is “foreign-field” arithmetic under each scheme?  
4. **Protocol-complexity discussion**: balancing statelessness, security, and implementer-friendliness.

> **Bottom line:**  
> A **Post-Quantum Verkle** tree built on a lattice-based, statelessly-updatable VC (especially Nguyen-2022), paired with hash-based or additive aggregation, promises to give Ethereum a truly quantum-resistant, scalable, ZK-friendly state-commitment—and is already attracting serious cryptographers.