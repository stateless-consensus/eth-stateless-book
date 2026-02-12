# Active Projects

This page provides an overview of the projects currently being worked on by the stateless Ethereum team.

## Binary Tree Implementation

Migration of Ethereum's state tree from the Merkle Patricia Trie to a binary hash tree. The binary tree (arity 2) produces significantly smaller state proofs, compatible with STARK compression. Implementation is underway in Geth and Besu, with a cross-client testnet expected shortly. EF research supports deployment for late 2027 / early 2028. A spec tweak is under discussion regarding the transition activation (one-block delay to avoid circular dependencies).


The conversion mechanism from the existing tree to the binary tree. Uses a system contract to store transition pointers, which simplifies reorgs and snap sync. Preimage distribution (needed to recompute keys in the new tree) is a major prerequisite. The transition strategy follows the [EIP-7612](../state-conversion/eip-7612.md) model.

## EIP-8037: State Creation Gas Cost Increase

Harmonizes and increases the cost of state creation operations (contract deployment, new storage slots, new accounts) to mitigate state growth under higher block gas limits. Introduces a dynamic cost-per-byte variable that scales with the gas limit, targeting an average state growth of 100 GiB/year. Also introduces multidimensional metering to separate state creation costs from execution gas, allowing larger contract deployments without hitting the single transaction gas limit.

## EIP-8038: State-Access Gas Cost Update

Increases the gas cost of state-access operations (SSTORE, SLOAD, CALL, BALANCE, EXT* opcodes) to reflect Ethereum's larger state and the resulting slowdown. State access costs haven't been updated since EIP-2929 (Berlin, 2021), while the state has grown significantly. Also fixes the underpriced EXTCODESIZE and EXTCODECOPY operations, which require two database reads but were priced the same as single-read operations.

## EIP-7907: Code Size Limits

Study of the impact of increasing the maximum contract code size, coupled with gas adjustments. Contracts larger than ~80 KB currently hit the per-transaction gas ceiling. Benchmarks measure the effect on client performance to guide the ACD decision.

## Compression-based State Expiry

An approach to state expiry through compression: old data is moved out of the active database into flat files, replaced by pointers. Corresponding internal trie nodes are removed to improve I/O.

## State Expiry Research

Exploration of several complementary expiry models:

- **Leaf-based** (EIP-7736 style): individual value expiry, resurrection via Merkle or STARK proof. Resurrection implementation is nearly complete in Geth.
- **Epoch-based**: each epoch gets a new tree, previous trees remain accessible with a resurrection bitmap.
- **"1 year of active state"**: exploration of a model where only one year of active state is kept in memory.

## BloatNet

A dedicated test network for stress-testing Ethereum's performance under state growth. Identified a critical threshold at ~650 GB beyond which memory consumption grows exponentially, validator performance degrades significantly, and state access times increase by 40%. Used to produce multi-client benchmarks and validate gas repricing proposals. More information at [bloatnet.info](https://bloatnet.info).

## Cross-Client Execution Metrics

A specification for standardized execution metrics across Ethereum clients. Enables objective performance comparison and bottleneck identification, particularly in the context of L1 scaling and the gas limit increase.

## User-Associated Storage (UAS)

A proposal to associate contract storage directly with user accounts, namespaced by contract address. Estimated savings of ~2000 gas per transaction on common paths. Requires new opcodes (USSTORE/USLLOAD). Works with the binary tree and is compatible with state expiry (a user's data expires together).

## ERC-8147: Locality-Preserving Storage Layout

A compiler-level change (no new opcodes) so that mapping storage keys for the same address land on the same leaf page in the binary trie. Improves spatial locality, cache/DB performance, and reduces witness sizes.

## Temporary Contract Storage

Semi-persistent storage managed by a system contract that clears automatically on a fixed cadence (~6 months). Aims to move ephemeral data out of permanent storage to slow long-term state growth.

## Partial Statefulness (Selective Snap Sync)

The ability for a node to sync only a segment of the state rather than the full state. Formalized through the OOPSIE concept (Opt-in Ownership of Partial State of Interest, Exclusively).
