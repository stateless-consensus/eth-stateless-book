# Trees

- [Trees](#trees)
  - [Overview](#overview)
  - [Current tree](#current-tree)
  - [Relevant design aspects](#relevant-design-aspects)
    - [Arity](#arity)
    - [Merkelization cryptographic hash function](#merkelization-cryptographic-hash-function)
    - [Account’s code](#accounts-code)
  - [Proposed trees strategies](#proposed-trees-strategies)

## Overview

Changing the state tree(s) is one of the core protocol changes we must make to achieve a stateless Ethereum future. The main goal is to design a new tree for more efficient state proofs.

## Current tree

The current data structure used to store Ethereum state is a Merkle Patricia Trie (MPT). [This article](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/) explains more about this tree.

## Relevant design aspects

Let’s explore the most important angles on a new tree design, addressed in Verkle Trees and Binary Tree proposals.

### Arity

The current MPT tree has an arity of 16. The original goal for this decision was to reduce disk lookups, as it means the tree becomes shallower compared to a lower-arity tree. The other side of the coin is that state proofs are way bigger. If interested in the details, read the following [rationale section in the Binary Tree EIP](https://eips.ethereum.org/EIPS/eip-7864#arity-2).

### Merkelization cryptographic hash function

The cryptographic hash function used for the tree merkelization can significantly impact state-proof verification. Verifying state proofs involves calculating hashes in a specific way to compare against the expected tree root.

There are two angles on performance:

- **Out of circuit** refers to how fast it is to calculate the hash function result in a regular CPU.
- **In circuit** refers to how fast this can be calculated in SNARK circuit under a particular proving system.

Both types of performance are relevant since the protocol requires calculating hash functions for different tasks both out and in circuits. The current tree uses [keccak](https://keccak.team/keccak_specs_summary.html), which has good out-of-circuit performance but is hard to perform decently on most bleeding-proving systems.

### Account’s code

The current MPT doesn’t store the account’s code bytecode directly in the tree; it only stores a commitment (i.e., the code-hash as `keccak(code_bytecode)`).

While this decision is helpful in avoiding potentially bloating the state tree with all the accounts’ code, it has an unfortunate drawback: since the tree stores the result of hashing the whole, we still need to provide the full code as part of the proof if we want to prove a small slice of it.

This is the reason for the worst-case scenario of proving the state for an L1 block. You can craft a block that forces the prover to include a contract code of maximum size. A better tree design should allow for more efficient proofing of account code slices.

Proving parts of an account's code is critical to efficiently allow stateless clients or block state proofs.  During a transaction execution, only a small fraction of an account’s code is typically executed. Think, for example, of an ERC-20: the sender will execute the `transfer` method but never do on-chain calls to other methods like `balanceOf`.

## Proposed trees strategies

The new tree proposals attack those problems by proposing:

- A more efficient encoding of data inside the tree.
- Including the account’s code inside the tree, allowing size-efficient partial code proving.
- A more convenient merkelization strategy to generate and verify proofs more efficiently.

Verkle and Binary trees share the same strategy for solving the first two points — we’ll dive deeper into them on the *Data encoding* page. Each proposes a different strategy for the last bullet, explained in their respective Verkle Trees and Binary Tree pages.
