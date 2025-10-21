<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD049 -->

# SIC calls history

- [Call #44: October 20, 2025](#call-44-october-20-2025)
- [Call #43: October 06, 2025](#call-43-october-06-2025)
- [Call #42: September 22, 2025](#call-42-september-22-2025)
- [Call #41: Aug 25, 2025](#call-41-august-25-2025)
- [Call #40: Aug 11, 2025](#call-40-august-11-2025)
- [Call #39: July 28, 2025](#call-39-july-28-2025)
- [Call #38: July 14, 2025](#call-38-july-14-2025)
- [Call #37: June 02, 2025](#call-37-june-02-2025)
- [Call #36: May 19, 2025](#call-36-may-19-2025)
- [Call #35: May 5, 2025](#call-35-may-5-2025)
- [Call #34: April 21, 2025](#call-34-april-21-2025)
- [Call #33: April 7, 2025](#call-33-april-7-2025)
- [Call #32: March 24, 2025](#call-32-march-24-2025)
- [Call #31: February 24, 2025](#call-31-february-24-2025)
- [Call #30: February 10, 2025](#call-30-february-10-2025)
- [Call #28: December 2, 2024](#call-28-december-2-2024)
- [Call #27: November 4, 2024](#call-27-november-4-2024)
- [Call #26: October 21, 2024](#call-26-october-21-2024)
- [Call #25: September 23, 2024](#call-25-september-23-2024)
- [Call #24: September 9, 2024](#call-24-september-9-2024)
- [Call #23: August 26, 2024](#call-23-august-26-2024)
- [Call #22: July 29, 2024](#call-22-july-29-2024)
- [Call #21: July 15, 2024](#call-21-july-15-2024)

## Call #44: October 20, 2025

[Agenda](https://github.com/ethereum/pm/issues/1770)
[Video recording](TBD)

### Team updates

- [@gballet](https://x.com/gballet) ([@go_ethereum](https://x.com/go_ethereum)) and [@kt2am1990](https://x.com/kt2am1990) ([@HyperledgerBesu](https://x.com/HyperledgerBesu)) reported partial test success; some vectors fail due to a suspected RLP encoding discrepancy between Geth and Besu.  
- [@kt2am1990](https://x.com/kt2am1990) ([@HyperledgerBesu](https://x.com/HyperledgerBesu)) is investigating problematic data dumps from Guillaume and will collaborate with Thomas Zamojski to accelerate resolution. Goal is to resolve Geth–Besu interoperability for binary-tree implementations.  
- Also reported that [@jasoriatanishq](https://x.com/jasoriatanishq) ([@NethermindEth](https://x.com/NethermindEth)) had deprioritized binary-tree work pending these test results..
- Mario Vega is leading the rebase of WELD/EELS/Yields, targeting completion within ~two weeks. This rebase is critical to integrate binary-tree support into the updated repo structure. Carlos will assist with plugging binary trees post-rebase, with support from Mario's team. Completion enables broader stateless test vectors and validation.
- [@gabrocheleau](https://x.com/GabRocheleau) shared that ([EthereumJS](https://twitter.com/EFJavaScript))'s verkle support is deprecated and has been removed from the repo. Binary-tree support is maintained for testing. Gabriel is also drafting a state-expiry survey for wallet/app/provider teams on state management, requirements, costs and potential incentives. Draft to be shared soon. 
- [@ngweihan_eth](https://x.com/ngweihan_eth) shared that he is researching and discussing in-protocol vs out-of-protocol state expiry: https://ethresear.ch/t/state-expiry-in-protocol-vs-out-of-protocol/23258. Also exploring contract-level state expiry (expiring storage of unused contracts). Estimated reduction: 50–60 GB (~20% of total storage). Approach favors client-side reclamation without immediate protocol changes; Wei Han plans to formalize an EIP and seek discussion.

### Compression-based state expiry

- Guillaume presented his initial findings and research on state compression: explorations target ~20% state size reduction initially, with pathways up to ~80% via more granular expiry to improve DB performance and storage efficiency. 
- Compression-based expiry: move old account/slot data out of DB into flat files. Replace expired values with pointers; drop internal trie nodes to improve I/O and reduce bloat. Writes to archived data reinsert into main state; thresholds proposed to balance recomputation cost. Serves as a gradual path toward potential in-protocol expiry.
- Performance notes: even a 20% whole-account expiry yields notable DB speedups in client benchmarks; 80% leaf-level targets larger savings but requires more work.
- Economic/fee alignment: if expiry becomes in-protocol, higher gas for writing to expired/archived storage (e.g., discussed alongside upcoming EIPs like 8037/8038) could incentivize efficient state usage. Builders may deprioritize transactions resurrecting expired state unless tipped appropriately.

## Call #43: October 06, 2025

[Agenda](https://github.com/ethereum/pm/issues/1747)
[Video recording](https://www.youtube.com/watch?v=W4K10M2eMHs)

### Team updates
- [@gballet](https://x.com/gballet) ([@go_ethereum](https://x.com/go_ethereum)) is assisting [@CPerezz19](https://x.com/CPerezz19) in reactivating execution spec tests to be binary-tree compatible and proposed a new gas-accounting PR (EIP-8032) aimed at stemming state growth.
- [@CPerezz19](https://x.com/CPerezz19) is working with [@gballet](https://x.com/gballet) on binary-tree test fixes, has identified and fixed bugs in the binary-tree implementation, and continues BloatNet work while pushing benchmarks to EELS for integration with Kamil's tool from [@NethermindEth](https://x.com/NethermindEth).
- Thomas Zamojski ([@HyperledgerBesu](https://x.com/HyperledgerBesu)) shared that [@kt2am1990](https://x.com/kt2am1990) is working on the transition and awaiting binary-tree test returns. Thomas has been focused on gas optimization and will return to statelessness, picking up the transition topic from Karim.
- [Gabriel Rocheleau](https://x.com/gabrocheleau) ([EthereumJS](https://twitter.com/EFJavaScript)) shared that EthereumJS could consider supporting stateless clients for future testnet usages, but absence of execution witnesses & proofs on binary tree testnets makes this impossible for the time being.

### Updates on binary trie t8n self-fixtures consumption progress. ([@CPerezz19](https://x.com/CPerezz19))
- State diffs are the primary blocker; more than half of the 500+ tests pass when excluding state diffs.
- Temporary removal of state-diff tests to simplify rebases and speed PR merges.
- Fixture refactor to align vertical and binary trees; transition to EELS with a Python reimplementation to generate fixtures natively.
- Test representation moving from separate allocs to a unified key–value list; adopt SHA256 for simpler key computation.

### Discussion on code chunks inclusion in the tree ([@CPerezz19](https://x.com/CPerezz19))
- Proposal to store code chunks separately and deduplicate across accounts, removing the preferential treatments of the first 128 chunks in the account leaf. 
- Treat code and storage uniformly to simplify the binary-tree structure; decision postponed pending further research.

### BloatNet updates ([@CPerezz19](https://x.com/CPerezz19))
- Unblocked on merging PRs to EELS; submitted fixes on stop-contract handling and benchmarking.
- Multi-opcode tests in place; key single-opcode tests focus on L1 scaling (SSTORE on the largest ERC-20s; SLOAD into empty/likely-empty slots).
- Goal is to integrate with Kamil’s benchmarking tool from [@NethermindEth](https://x.com/NethermindEth); cross-team call scheduled. Stop contracts not specifiable across tests remains a major issue.
- Syncing tests: collaboration with partners; clients like Reth and Erigon are actively debugging syncing issues. Several benchmark runs executed; target is five clients in one pass to avoid heavy snapshot re-runs.

### Findings on post-EIP6780 selfdestructs ([@ngweihan_eth](https://x.com/ngweihan_eth))
- Around 2.7M unique self-destructed accounts (~1% of accounts; ~0.03% of storage slots); activity peaked shortly after Dencun and has since declined.
- Small current footprint noted by [@ngweihan_eth](https://x.com/ngweihan_eth); [@gballet](https://x.com/gballet) flagged cumulative risks if zero-slot accounts persist.
- Next steps: gather more precise frequency data to assess long-term effects and inform potential pruning/removal proposals.

## Call #42: September 22, 2025

[Agenda](https://github.com/ethereum/pm/issues/1718)
[Video recording](https://www.youtube.com/watch?v=9Rt3xL8OT2U)

### Devnet Progress & Performance:

Karim Taam released a new Docker image for the upcoming devnet and has improved transition implementation by pre-downloading preimages to a database, rather than a single RPC call per preimage resolution. Testing is currently underway.

### Blobnet & Benchmarking Updates

Carlos Perez shared progress on Bloatnet syncing, including fixes for peering issues, and announced the readiness of the multi-opcode benchmarking PR, which will streamline future benchmarking efforts.

### Ethereum State Analysis Insights

Wei Han Ng presented a [detailed analysis of Ethereum mainnet state access patterns](https://ethereum-magicians.org/t/not-all-state-is-equal/25508). Key findings include that EOAs have longer activity spans than contracts, with 55% of contracts active for only one block and 80% of accounts active for less than a year. The analysis also showed a high concentration of contract deployments, with the top 100 bytecodes and factories accounting for significant percentages of all contracts, indicating widespread bytecode reuse.

Based on the data, Wei Han Ng proposed several ideas:
 * Re-evaluating state expiry periods, suggesting that keeping data for over a year might be sufficient.
 * Implementing a temporary storage model where slots automatically clear after a set period.
 * Making contract deployment cheaper for reused bytecodes through mechanisms like a same-block duplicate discount or a global code registry.
 * Introducing progressive per-address storage pricing to disincentivize excessive slot spamming, though acknowledging potential drawbacks for successful contracts.

Guillaume Ballet discussed the implications of removing storage slot deletions (where writing to zero no longer deletes a slot) on state bloat, noting that current data suggests a significant impact that would necessitate repricing.

### Devnet Transition & Pre-image Distribution

 * A new key computation format for the next testnet using SHA-256 hash was also discussed to simplify client implementation by eliminating the need for pre-image distribution. This was rejected, as is makes things more complex after all.
 * The next devnet will not include a transition to facilitate Geth's participation due to slow merging. Future devnets with transitions will require a clear specification for pre-image downloading.

### State Analyzer

Plans were announced for a state analyzer panel on the EthPandaOps Lab website, providing a state dashboard with various metrics that will be regularly updated to track patterns.

## Call #41: August 25, 2025

[Agenda](https://github.com/ethereum/pm/issues/1684)

[Recording video](https://www.youtube.com/watch?v=GVFwMhiUueg)

### 1. Team updates

- [@kt2am1990](https://x.com/kt2am1990) ([@HyperledgerBesu](https://x.com/HyperledgerBesu)) successfully completed the tests on Hoodi, allowing for transitions without crashes or memory issues.

- [@jasoriatanishq](https://x.com/jasoriatanishq) ([@NethermindEth](https://x.com/NethermindEth)) is starting to work on Binary Tree implementation.

- [@gballet](https://x.com/gballet) ([@go_ethereum](https://x.com/go_ethereum)) is working on merging more code into Geth, aiming to perform more tests and replays for the testnet

### 2. EIP4762 Updates

- [@gballet](https://x.com/gballet) presented an update on EIP4762, explaining a proposed change to how system calls interact with witnesses. If a call is a system call, nothing should be recorded into the witness, simplifying the behavior.

### 3. BloatNet Updates

- [@CPerezz19](https://x.com/CPerezz19) shared that syncing at 2x mainnet size for state were conducted. There were some issues primarily due to peering. Only Besu and Geth were able to sync as of now. Future goals include incorporating more syncing metrics and launching compute-based benchmarks.

### 4. Blake3 vs. SHA256 in Geth

- [@gballet](https://x.com/gballet) discussed a pushback from Gary in the Geth team regarding merging Blake3 in Geth as there isn’t a reliable release in Golang. The teams have concluded that SHA256 would be used for the initial testnets until a formal decision is made at the EIP level.

### 5. State Expiry Pruning Benchmarks

- [@ngweihan_eth](https://x.com/ngweihan_eth) presented benchmarking results for “naive pruning” in Geth, focusing on the snapshot layer to evaluate the effectiveness of state expiry. The results showed significant reductions in snapshot size (81.3%) and decent performance gains in state read and DB compaction time.

## Call #40: August 11, 2025

[Agenda](https://github.com/ethereum/pm/issues/1672)

[Recording video](https://www.youtube.com/watch?v=CrVLcOEfPhQ)

### 1. Team updates

- [@gballet](https://x.com/gballet) [@gabrocheleau](https://x.com/GabRocheleau) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)): several stateless PRs merged in Geth; next up is transition logic and binary-tree integration. 
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)). Async update done by Guillaume: Bloatnet now ~2× mainnet & started a testing doc and are keeping a [registry of bloatnet cases](https://hackmd.io/9icZeLN7R0Sk5mIjKlZAHQ?view). 
- [@kt2am1990](https://x.com/kt2am1990) ([Besu](https://x.com/HyperledgerBesu)): in-memory binary tree integrated; node can compute binary-tree state roots (non-validating) and follow a local fork; simple Geth vectors validate; shadowfork transition experiments ongoing; perf work reduced transition overhead. Besu has validated simple Geth vectors and will consume more as they land.
- [@jasoriatanishq](https://x.com/jasoriatanishq) ([Nethermind](https://x.com/NethermindEth)): wrapped Verkle transition work; addressing perf/refactors; binary-tree work to begin after leave (in 2 weeks), timeline TBD.
- [@ngweihan_eth](https://x.com/ngweihan_eth) ([@StatelessEth](https://x.com/StatelessEth)): code-chunking analysis shows ~30% average byte access for 32-byte chunks; code-access opcodes boost locality. Findings: ~half of EOAs active for only 4–5 days; most storage slots touched once within a block. [Analysis](https://hackmd.io/RfHgXDnBSW2CZkWhANjeHw?view)

### 2. New testnet

- [Besu](https://x.com/HyperledgerBesu) will lead the binary-tree testnet; aim is before month-end. A Docker image enabling testnet bring-up should be shareable in ~1 week (includes recent EIP hooks and gas adjustments).
- [Geth](https://x.com/go_ethereum) is delayed by a state-layer rewrite; unlikely to lead this month but will follow once ready.
- [Nethermind](https://x.com/NethermindEth) timeline to be confirmed on the next call.
- Cross-client note: align handling of system-address calls for witness behavior; verbal agreement exists, spec/doc edits pending, and Besu will wire this once finalized.

### 3. EIP-2926 (Chunk-Based Code Merkleization)

- [EIP-2926](https://eips.ethereum.org/EIPS/eip-2926) is being resurrected as a smaller, low-risk step toward fuller statelessness.
- Benefits: removes the effective code-size ceiling, enables chunk-level witnesses/caching, and mitigates prover-killer patterns (e.g., pathological `EXT*CODE*` access).
- Plan: target an early rollout (e.g., Glamsterdam); perform a tree transition for code (~10 GB scale, ~1 day) to demonstrate feasibility, and ship a first, smaller package of the stateless stack.
- Next: present the proposal, circulate the draft and gather client feedback. 

### 4. Stateless Summit

- [@gabrocheleau](https://x.com/GabRocheleau) preparing a survey to tooling teams, RPC providers, wallets, and client teams to gauge interest, proposed contributions, and topics; also to collect speaker nominations.
- Devconnect Buenos Aires may skew app-focused; if core-protocol attendance looks light, we’ll consider alternate timing (e.g., ETHCC Cannes in March).
- Action: finalize and distribute the survey; consolidate responses and propose an agenda window.

### 5. Testing documentation

- [@gballet](https://x.com/gballet) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)): Binary-tree testing doc started: input/output examples and root-hash verification. Expand beyond basic leaf insertions to account+storage scenarios.
- [Test registry](https://notes.ethereum.org/@gballet/binary_tree_test_vectors) maintaining a running catalog of cases.

## Call #39: July 28, 2025

[Agenda](https://github.com/ethereum/pm/issues/1627)

[Recording video](N/A)

Call was skipped because of low attendance. Below are the async updates.

### 1. Team updates

- [@gballet](https://x.com/gballet)[@gabrocheleau](https://x.com/GabRocheleau) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)) are still working on filling test fixtures, running into issues with the Binary tree filling that remain to be addressed.
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)) created metric proposals for Sync & Compaction (https://hackmd.io/@CPerezz/SkzDZmngT) and created scenarios & questionnaire (https://hackmd.io/@CPerezz/ryoMhzaLel, https://hackmd.io/@CPerezz/HkZlj_jIgg) - Planning to review with teams (ETA Aug 1st 2025). 


### 2. Upcoming testnet

- Postponed to next meeting.

### 3. Bloatnet update

- Ran into potential issues with Erigon and Besu. Erigon has identified the write amplification bug and are working on a fix. 
- Besu suffered from bad peering, but this seems to have been solved. 
- After a 1k block reorg caused by a wrong version of Besu, we are bloating again.
- Potential upcoming Bloatnet name change.

## Call #38: July 14, 2025

[Agenda](https://github.com/ethereum/pm/issues/1566)

[Recording video](https://youtu.be/xxxxxxxxxxx)

### 1. Team updates

- [@gballet](https://x.com/gballet) [@gabrocheleau](https://x.com/GabRocheleau) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)) is working on filling test fixtures for Binary Trees, starting with binary-tree genesis and will then proceed with conversion testing.
- [@kt2am1990](https://x.com/kt2am1990) ([Besu](https://x.com/HyperledgerBesu)) is close to finalizing a working in-memory binary tree, while Geth faces delays due to a complete state layer rewrite. Should have an implementation ready to consume tests within a few days of work. 
- [@ngweihan_eth](https://x.com/ngweihan_eth) ([@StatelessEth](https://x.com/StatelessEth)) is building a State Indexer project and presented code chunking analysis. 
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)) is working on witness updating and bloatnet progression.

### 2. EIP-4762: Witness addition rules

- Felix Lange raised concerns about the definition of system contracts in EIP-4762. The group agreed to revise the EIP to base witness rules on system calls (from specific addresses) instead of system contracts, reducing maintenance burden. Felix will update the EIP accordingly.

### 3. New testnet

- [Besu](https://x.com/HyperledgerBesu) team expects readiness for a binary tree testnet in about a month, while Geth is delayed due to a state rewrite. Agreement to proceed with a Besu & Nethermind-only testnet, breaking with the usual Geth-first approach. Timeline for Nethermind is uncertain, to be confirmed in the next call.

### 4. Bloatnet update

- Bloatnet has reached 1.5x the mainnet state after resolving RPC issues. All clients completed a first round of syncing tests. This round might have uncovered a pruning bug in [Erigon](https://x.com/erigoneth), to be confirmed. Next phase will target 2x mainnet state, focusing on compaction and worst-case scenario testing.

### 5. Code chunking analysis presentation and EIP proposal

- [@ngweihan_eth](https://x.com/ngweihan_eth) ([@StatelessEth](https://x.com/StatelessEth)) presented analysis of 32-byte code chunking, showing a 30% average byte access ratio. Code access instructions significantly improve access ratios. The group discussed experimenting with different chunk sizes (e.g., 16 bytes) for efficiency. Wei Han will update the analysis and publish a dashboard. 

## Call #37: June 02, 2025

[Agenda](https://github.com/ethereum/pm/issues/1555)

[Recording video](https://youtu.be/iclp0s_f-68)

### 1. Team updates

- [@ignaciohagopian](https://x.com/ignaciohagopian) ([@StatelessEth](https://x.com/StatelessEth)): has kept working on adding more zkEVM coverage in `execution-spec-tests`. He mentioned that these tests are not only trying to trigger worst-case scenarios but also build a foundation to reevaluate potential gas cost changes for zkEVMs.
- [@gballet](https://x.com/gballet) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)): has finished the Binary Tree implementation with spec tests passing, and has been working on filling existing `execution-spec-tests` with this new tree so other clients can have a larger set of tests to compare implementation correctness. It has also continued working on the Python-ification of EIP-4762 using received feedback.
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)): has been working on spamoor use-cases for state bloating, and preparation work for BloatNet, which was presented later in the call.
- [@kt2am1990](https://x.com/kt2am1990) ([Besu](https://x.com/HyperledgerBesu)): has been refactoring the Besu’s codebase to support shadow forks in general, not only for Hoodi. Also, he has been working on adding support for block creation, which surfaced some performance problems related to Verkle Trees. Since this tree was designed as abandoned, it has decided to ignore them and prepare for the next shadow fork to use Binary Trees.

### 2. Leaf-level state expiry (EIP-7736) effectiveness

[@ngweihan_eth](https://x.com/ngweihan_eth) presented his work on the effectiveness of Leaf-Level state expiry. The presentation slides provide an excellent summary of findings, which can be found [here](https://docs.google.com/presentation/d/14O9yIDAKMeOW-EAEeAvdyfyy12jwheqT51xWmsYp6jI/edit?slide=id.g2fc552e0e46_1_692#slide=id.g2fc552e0e46_1_692).

After the presentation, there were some discussions:

- [@gballet](https://x.com/gballet) asked if there is any intuition on how raising the gas limit would affect the total active state of the network, or its relationship with worst-case scenarios. [@ngweihan_eth](https://x.com/ngweihan_eth) mentioned that theoretically, the worst case is that all gas is used to touch as much state as possible within the state expiry epoch, so the active state is very big. On a more practical note, it is helpful to keep an eye on how raising the gas limit might change the data access patterns for many use-cases, for example, ERC20 contracts. Also, the state expiry period could be adjusted to 6 months instead of the usually assumed 1-year period.
- [@CPerezz19](https://x.com/CPerezz19) asked if it is possible to distinguish what kind of data is being expired, such as account vs storage data. [@ngweihan_eth](https://x.com/ngweihan_eth) mentioned this isn’t possible. This is because unified tree designs don’t have extra information to provide semantics on leaf-level data. He noted that adding a marker could also help fix some revival transaction validation challenges, or consider not expiring account data, but only contract storage. More thought should be put into this since there might be some tradeoffs.
- [@ignaciohagopian](https://x.com/ignaciohagopian) mentioned that in his previous research, he found that heavy mainnet usage of Solidity hashmaps in a unified tree was pretty unfriendly with state expiry. Hashmaps spread data over the tree space, creating many stems with only one value. [@ngweihan_eth](https://x.com/ngweihan_eth) shared that he has also been thinking about this and found that many of these single values should expire, so at least at the value level, we can still make a big number of deletions. Still, it would be useful to dig deeper into checking how many of the unexpired stems only contain one value.

### 3. BloatNet initiative website

[@CPerezz19](https://x.com/CPerezz19) presented the release of the [BloatNet website](https://cperezz.github.io/bloatnet-website/), which contains full details about the initiative's motivation and goals. Additionally, it contains the metrics to be gathered and the potential attack vectors to try bloating the Ethereum state size as much as possible.  After the first version of this devnet runs, this website will contain all the findings that can help motivate any protocol changes to prevent excessive state growth and understand the implications of gas limit increases on network robustness.

He mentioned that feedback has already been received from Geth, Besu, and Erigon, which is helpful since they’re the main network actors who will benefit from the findings. Any other core developer or community member is welcome to collaborate — the three main sections of the website (Metrics, Attack Vectors, and State Bloat) contain forms you can fill out to suggest more metrics, bloating strategies, or any useful idea.

## Call #36: May 19, 2025

[Agenda](https://github.com/ethereum/pm/issues/1538)

[Recording video](https://www.youtube.com/watch?v=bNa7SaCgnIc)

### 1. Team updates

- [@ignaciohagopian](https://x.com/ignaciohagopian) ([@StatelessEth](https://x.com/StatelessEth)): has worked on the zkEVM benchmark suite to assess the performance on zkVMs for L1 block proving. He will present this work in a future SIC to explain it further.
- [@gballet](https://x.com/gballet) ([@StatelessEth](https://x.com/StatelessEth)/[@go_ethereum](https://x.com/go_ethereum)): has been working on merging more pending PRs into geth, rebasing the current default stateless branch on top of Pectra, phant compilation to RISCV32,  remaining spec differences with Geth Binary Trees implementation, and an EIP-4762 spec rewrite that was presented later in the call.
- [@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: has made progress in passing the conversion tree execution spec tests and helped review the rewrite of EIP-4762 draft PR.
- [@kt2am1990](https://x.com/kt2am1990) (Besu): has worked in a Hoodi state tree conversion that was later presented in more detail in the call. Thomas from his team will also start working on a Binary Tree implementation for Besu.
- [@lfmpinto](https://x.com/lfmpinto) (Besu): worked on enhancing tx/block tracing to include the witness state at each opcode execution event — this is the [related PR](https://github.com/hyperledger/besu/pull/8336).
- [@jasoriatanishq](https://x.com/jasoriatanishq) (Nethermind): has been progressing in passing all existing tests in the rebased version on top of Pectra. He plans to reproduce Karim's work in the Hoodi testnet and work on a Binary Tree implementation.
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)): has been working on including [more scenarios](https://hackmd.io/D12VBHdMSU6y_vcqWcJV_g) into the [spamoor](https://github.com/ethpandaops/spamoor) tool and coordinating with the Nethermind team on how to prepare the launch of BloatNet. He has also been contacting wallets and dapps to understand their thoughts on strong statelessness and share new ideas to see if users providing witnesses can be incentivized at the protocol level.

### 2. Hoodi state tree conversion

[@kt2am1990](https://x.com/kt2am1990) presented his work migrating the Hoodi state tree in the live testnet using the current [Overlay Tree](https://eips.ethereum.org/EIPS/eip-7612) + [block-by-block state conversion](https://eips.ethereum.org/EIPS/eip-7748) EIPs. 

The presentation can be found [here](https://docs.google.com/presentation/d/14-a5z4aBQP0r2sNjtpl-jXzMaJSsUy6NuAxGqP1QJM4/edit) and can provide a nice summary of the talk, but as some general remarks:

- It used a Nimbus client as the CL to connect to Hoodi and two Besu nodes for the task.
- Two Besu nodes were used as a quick way to have preimage resolving for the tree conversion.
- The timestamp where the conversion starts is hardcoded today, but other ways of providing this value can be evaluated in the future when more clients join a similar setup.
- The total conversion progress took 13 hours, according to Hoodi’s slot time duration.
- Previous run attempts allowed for detecting problems, including performance, chain reorgs, and state rollbacks. After the fixes, a successful run was possible.
- Future steps could cover performing the same work in bigger networks like Sepolia or Mainnet.

After the presentation, there were follow-up discussions:

- [@gballet](https://x.com/gballet) agreed it would be helpful to have more clients join the same effort so the resulting state roots can be further verified. He also mentioned it would be helpful to see if it would be possible to try other code paths, such as block building.
- [@gballet](https://x.com/gballet) asked if there were available metrics regarding the pre- and post-state size, which could be interesting. [@kt2am1990](https://x.com/kt2am1990) mentioned that he will take a look and share offline.
- [@gballet](https://x.com/gballet) asked if there was more insight into the conversion speed, and [@kt2am1990](https://x.com/kt2am1990) mentioned the extra RPC influenced these calls for preimage resolving. The node will have the preimages in future versions, and these extra RPC calls can be removed.
- [@ignaciohagopian](https://x.com/ignaciohagopian) mentioned that reorg-like tests don’t exist yet since it is pending that the testing framework can support them, but it is planned to do it eventually.
- A variant strategy for checking state roots between EL clients is to export an existing chain, agree on the conversion timestamp, and let clients run the migration “locally” and compare results. This could be easier than shadow-forking a live testnet.

More EL clients will be jumping into running these scenarios to gain more confidence about state root calculations (and thus, the correctness of the EIP implementation).

### 3. EIP-4762 revamp

[@gballet](https://x.com/gballet) presented a rewrite of EIP-4762 with a more Pythonic style, aligned with the more modern styles of protocol changes specs. This rewrite is the first draft that [@GabRocheleau](https://x.com/GabRocheleau) and [@lfmpinto](https://x.com/lfmpinto) reviewed.

The EIP rewrite helped identify implementation bugs in Geth, such as accessing bytecode outside the defined bytecode for a contract (i.e., a `PC` greater than the contract size without accessing valid bytecodes in that code-chunk). Other clients will double-check how they have implemented this particular border case to see if there is already a consensus bug issue.

Apart from rewriting the original EIP, this new version also aims to cover the atomicity changes discussed in previous SIC calls and EIP-7702 considerations pending in the current specs. The draft must still resolve how warm costs are charged correctly, checking `CALLCODE` witness rules, and other quirks. Still, there will be further iterations after getting feedback from reviews.

After the presentation, there were further discussions:

- [@GabRocheleau](https://x.com/GabRocheleau) asked about the plans for Binary Trees and the current EIP. [@gballet](https://x.com/gballet) said that at this point, it seems clear that the current plan is going with Binary Trees, but we will still use Verkle for short-term efforts on Hoodi state tree conversion. Whenever the next stateless devnet happens, it will use Binary Trees (probably around Q2/Q3). EIP-4762 will be used as is for Binary Trees since it should be compatible with the EIP assumptions about the underlying tree. There might be pushbacks and potential changes, but it should make sense overall.
- [@jasoriatanishq](https://x.com/jasoriatanishq) asked how fast we’ll start using Binary Trees in the upcoming work, since in the next steps, EL clients will try reproducing [@kt2am1990](https://x.com/kt2am1990) work on Hoodi. [@gballet](https://x.com/gballet) confirmed that this is the plan (i.e., keep testing the conversion with Verkle) and Binary Trees will be used first in a less complex context, such as a fresh devnet.

### 4. Forking rules for state tree conversion

[@GabRocheleau](https://x.com/GabRocheleau) raised a point on how the forks would work for the state transition, particularly regarding how EL clients decide on expectations on witness in the block and/or the Overlay Tree functioning. 

[@gballet](https://x.com/gballet) mentioned that Geth works by only expecting a single fork, and keeping track of the state conversion logic initialization and finalization events to decide on those particular points. [@GabRocheleau](https://x.com/GabRocheleau) found this clarification helpful and suggested that it be explained better in the specs. [@gballet](https://x.com/gballet) agreed we can do that.

## Call #35: May 5, 2025

[Agenda](https://github.com/ethereum/pm/issues/1497)

[Recording video](https://www.youtube.com/watch?v=Oo8PcGqmLQ4&feature=youtu.be)

### 1. Team updates

- [@ignaciohagopian](https://x.com/ignaciohagopian) ([@StatelessEth](https://x.com/StatelessEth)): published an [article](https://www.notion.so/DEPRECATED-Merkelizing-Bytecode-Options-Tradeoffs-1d6d9895554180fd9b58f807cf05315e?pvs=21) revisiting bytecode chunking solution space and started helping on zkVM benchmarks.
- [@gballet](https://x.com/gballet) ([@StatelessEth](https://x.com/StatelessEth)/Geth): worked on many upstream PRs into Geth, began rebasing the stateless branch on top of Pectra, and is preparing for a rewrite of EIP-4762 with a more Python-like style, which will also include EIP-7702 updates.
- [@CPerezz19](https://x.com/CPerezz19) ([@StatelessEth](https://x.com/StatelessEth)): is [preparing](https://hackmd.io/@CPerezz/ryATkZIelx) to launch a devnet focused on aggressive state-growth; scripted spam scenarios that maximise “gas-per-byte” expansion and started prototyping a mainnet replay strategy to reach 2-4× state size.
- [@jasoriatanishq](https://x.com/jasoriatanishq) (Nethermind): finished the Pectra rebase and will then work on integrating the tree conversion. He also plans to work on the implementation of the Binary Tree EIP.
- [@kt2am1990](https://x.com/kt2am1990) (Besu): rebased Verkle on Pectra and tested the conversion on Hoodi. While doing so, he identified some bugs and performance issues that are being fixed. Thomas keeps working on stateless verification.
- [@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: working on passing all the tree conversion test vectors, generalizing the conversion to any target tree, and doing other architectural adjustments.

### 2. *BloatNet* state-growth testnet

_Note: the call recording refers to this devnet as FatNet but it was later renamed to BloatNet._

[@CPerezz19](https://x.com/CPerezz19) explained the goal of *BloatNet*: replicate Nethermind’s idea with *PerfNet* but focus on state size growth and its implications for EL clients. He shared a [collaborative document](https://hackmd.io/@CPerezz/ryATkZIelx) to identify valuable metrics to gather.

[@_sophiagold_](https://x.com/_sophiagold_) suggested including snap-sync metrics, and [@CPerezz19](https://x.com/CPerezz19) mentioned it is useful but requires chatting with Pari to figure out how to do this correctly, since it might have some networking assumptions.

[@gballet](https://x.com/gballet) mentioned that EL clients already have a cap on the number of connected nodes, so we might be able to simulate this realistically. Additionally, he mentioned that some transaction load (and proper access patterns) should be happening on the chain to test the healing phase correctly.

### 3. EIP-7748 – special-account conversion rule

A follow-up discussion clarified some confusion in the last SIC regarding custom rules in EIP-7748 regarding not converting some data.

[@gballet](https://x.com/gballet) confirmed that the accounts previously described in EIP-7748 don’t exist on mainnet. The new proposed action targets accounts described in [EIP-7610](https://eips.ethereum.org/EIPS/eip-7610): accounts with zero nonce, empty code, non-zero balance, and non-empty storage. There are accounts satisfying this condition on mainnet, and it will be helpful if we can clean them up during the conversion. The intention is to keep the invariant that if an account has non-empty storage, the nonce is greater than zero.

It was pointed out that this rule doesn’t conflict with EIP-7702, since any activated delegation implies an increase of the nonce, thus stopping the `nonce==0` condition of the rule.

EIP-7748 was already updated with this new rule, so EL clients are expected to update their implementation. This also means that execution-spec-tests should cover this new rule.

### 4. EIP-7702 – delegation removal semantics

[@ignaciohagopian](https://x.com/ignaciohagopian) wanted to discuss more precisely a topic tangentially touched on in the previous SIC call: which is the proper action in the new tree when an EIP-7702 delegation is removed?

He proposes that, apart from updating the code-size and code-hash to zero and empty, we must also clear the delegation indicator. He argues that, although a removed delegation can be indirectly detected by checking the code size or code hash, it would leave the tree in an inconsistent state. If someone only looks at the first code chunk, they might see a delegation without effect since it is also forced to check the code-size/code-hash.

After discussing the increased gas cost, [@gballet](https://x.com/gballet) and [@kt2am1990](https://x.com/kt2am1990) agreed that clearing the delegation indicator is correct. Nobody else opposed the decision. This decision will be materialized in the upcoming EIP-7702 updates in all stateless-related EIPs.

### 5. EIP-4762 – atomic witness charging

Some SIC calls ago, [@gballet](https://x.com/gballet) proposed a change for EIP-4762 where additions to the witness corresponding to a single operation have atomic gas charging, which means they don’t generate partial witnesses.

He [implemented this](https://github.com/gballet/go-ethereum/pull/541/files) for Geth and feels that this is a change worth confirming. [@jasoriatanishq](https://x.com/jasoriatanishq) was initially doubtful about this change, but now has a neutral opinion. [@gballet](https://x.com/gballet) also mentioned that most tests pass, but only 99 fail. This is likely expected since it is a spec change. [@ignaciohagopian](https://x.com/ignaciohagopian) confirmed that some tests are expected to fail, and he can help identify which ones should, so we know which is a good stopping point for tests to pass.

Additionally, [@ignaciohagopian](https://x.com/ignaciohagopian) said this proposal should be written in the EIP — the plan is to include it as part of the more general EIP-4762 rewrite that will happen soon.

### 6. Access-lists interaction with 4762

[@ignaciohagopian](https://x.com/ignaciohagopian) noticed that the current EIP-4762 spec doesn’t mention any effect on currently available access lists. In particular, he mentioned an example where a currently warmed-up storage slot via access lists would have a more expensive gas cost after EIP-4762. This means it will probably break contracts that rely on access lists for proper functioning.

[@gballet](https://x.com/gballet) did some history exploration on the previous versions of the EIP and found that an old version [mentioned](https://github.com/ethereum/EIPs/blob/bff015462ed74da68759dbc0aebce5ffe2d53bf3/EIPS/eip-4762.md#replacement-for-access-lists) access lists but was only related to their serialization format and not gas cost impacts.

[@ignaciohagopian](https://x.com/ignaciohagopian) proposed two options. The naive one is processing the access list and including the accounts/storage-slots as part of the witness, which can cause witness bloat if those aren’t accessed. The other variant is EL clients must always consider access lists before doing any branch/chunk cold charging, which avoids the witness bloat if elements in the access list are never accessed.

[@gballet](https://x.com/gballet) said that something around those lines could work, and suggested doing an EIP-4762 PR concrete proposal to clarify this topic and discuss it again in a future SIC to make a decision.

## Call #34: April 21, 2025

[Agenda](https://github.com/ethereum/pm/issues/1452)

[Recording video](https://www.youtube.com/watch?v=lhw3KBBQkLo)

### 1. Team updates

[@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth): worked on a newly published [Tree Shape page](https://stateless.fyi/development/mainnet-analysis/tree-shape.html) in the book, progressed on a new tool to analyze 4762 mainnet impacts ([gathering metrics](https://jsign.github.io/eth-state-access-charts/) while doing that), and is finishing some research doc on bytecode chunking and a book page for the 32-byte code chunker.

[@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: I made progress on the tree conversion code and plan to start running existing execution-spec-tests to check the correctness of the implementation.

[@gballet](https://x.com/gballet) from geth/[@StatelessEth](https://x.com/StatelessEth): continued working on merging tree conversion logic into geth master. He also published a [Holesky shadowfork](https://stateless.fyi/development/advanced/holesky-shadowfork.html) page in the stateless book, which explains how to easily run geth in devnet mode using Holesky state.

[@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind: has been wrapping the tree conversion implementation, now passing all tests. He is also working on rebasing the working branch on Pectra.

### 2. EIP-7612: follow up on code conversion when touching an account

The discussion focused on EIP-7612's behavior in specific scenarios, namely:

- When the storage slot of a contract is written to.
- Whether code should be converted to the new tree when account data (e.g., balance) is written.

In the first issue, [@gballet](https://x.com/gballet) initially had concerns about the difficulty of implementing the change for geth. However, upon further investigation, he concluded that the change was not as problematic as initially thought. Given the support from other EL client core developers, the decision was made to convert only the written storage slot and not the account data.

In the second issue, the group had previously decided that careful calculations regarding the worst-case write load were necessary if code conversion was also implemented. [@gballet](https://x.com/gballet) [presented the calculations](https://notes.ethereum.org/@gballet/rJqTrRXyex#/), and it was confirmed that the change was unsafe and therefore could not be made. This conclusion aligns with EIP-7612, so no changes to the specification are required.

### 3. EIP-7612: EIP-7702 code-mutation implications

During discussions, [@ignaciohagopian](https://x.com/ignaciohagopian) mentioned how EIP-7702 introduces a new scenario where an account's code might need to be converted as part of EIP-7612 before tree conversion is activated.

When EIP-7612 is active and a transaction with a 7702-delegation is executed, an an EOA's code is considered modified, i.e., the bytecode is modified. Since the Merkle Patricia Trie is considered read-only, the delegation bytecode should be written in the new tree. Participants agreed that this is the correct behavior for EIP-7612.

[@gballet](https://x.com/gballet) also raised the scenario where an EOA with an existing delegation decides to remove it. Currently, the new tree API doesn't support deletions. The conclusion reached was that the corresponding code-chunk of the delegation would be written with 0x00 bytecodes. The removal of the delegation would be signaled by `codeSize` and `codeHash` having values zero and empty-hash, respectively (although the code-chunk would still exist in the tree since leaf deletions aren’t supported).

These conclusions will be included as part of EIP-7702 impact on stateless EIPs.

### 4. EIP-7748: empty accounts conversion rule

[@ignaciohagopian](https://x.com/ignaciohagopian) proposed simplifying EIP-7748 by removing the current rule of not migrating empty accounts with storage data. The original intention of this rule in the EIP is to finish cleaning up existing accounts during the tree conversion. However, considering that [EIP-7523](https://eips.ethereum.org/EIPS/eip-7523) claims no existing instances are present in mainnet, this rule could potentially be removed, lowering the complexity of the tree conversion logic..

[@gballet](https://x.com/gballet) noted that while [EIP-7610](https://eips.ethereum.org/EIPS/eip-7610) claims there are accounts with zero nonce, zero code length, and non-empty storage, it isn’t clear if these cases include any account with zero balance.

The decision was made to verify with a synced full node whether any empty accounts with non-empty storage still exist in the mainnet, after which removing the rule from EIP-7748 could be considered.

### 5. EIP-7748: tree conversion logic position in block execution pipeline

During tree conversion test writing, [@ignaciohagopian](https://x.com/ignaciohagopian) noticed that geth's tree conversion logic was triggered after block transaction execution, contrary to EIP-7748, which describes it occurring beforehand. He explained this shouldn't be an issue theoretically, as any tree conversion writes can be overridden by block execution writes, and tree conversion writes are independent of transaction execution effects.

An open question was posed to participants about potential relevance of both orderings. No compelling reason favouring one order over the other was identified — the exact positioning is not considered relevant at the specification level, allowing EL clients to determine the placement as an implementation detail.

### 6. EIP-4762: CALL new account cost and FILL_COSTs

During the meeting, [@ignaciohagopian](https://x.com/ignaciohagopian) presented a potential mispricing issue with `FILL_COST`s in EIP-4762. The presentation, supported by slides, explained that after packing account fields into `BASIC_DATA`, the `FILL_COST` might be too low. This allows creating a new account with a value-bearing `CALL` costing approximately half the gas compared to the current mainnet cost of 25,000. Additionally, creating a new storage slot under EIP-4762 would also be approximately half the cost compared to mainnet.

The meeting concluded without a decision on how to adjust the current `FILL_COST` gas cost of 6,200. Further discussions are necessary to understand the potential implications of repricing on other aspects of writing new state.

## Call #33: April 7, 2025

[Agenda](https://github.com/ethereum/pm/issues/1418)

[Recording video](https://www.youtube.com/watch?v=EwsUDLSj_1w)

### 1. Team updates

[@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth): worked on new execution-spec-tests conversion tests, a document shared with [@techbro_ccoli](https://twitter.com/techbro_ccoli) regarding tech debt in the testing framework, and also shared a [document about EIP-4762 worst-case blocks](https://hackmd.io/@jsign/4762-worst-case).

[@gballet](https://x.com/gballet) from Geth/[@StatelessEth](https://x.com/StatelessEth): has been working on a Binary Tree implementation for geth, where the next steps are running the official test vectors to check correctness. He also has been working on merging the tree conversion logic into mainnet Geth.

[@CPerezz19](https://x.com/CPerezz19) from [@StatelessEth](https://x.com/StatelessEth): has been working on a document to be shared soon on the relationship between stateless and FOCIL, investigating potential incompatibilities/challenges and symbiosis between them. Some findings might be discussed in future SIC calls.

[@kt2am1990](https://x.com/kt2am1990) for @HyperledgerBesu: has been working on the tree conversion, fixing some problems found while running the tests shared by [@ignaciohagopian](https://x.com/ignaciohagopian). All tests are passing now.

[@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind: has also been working on tree conversion, with most tests passing. It is also working on rebasing Nethermind Verkle implementation on top of Pectra.

[@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: he couldn’t make much progress on the tree conversion logic, but he’s planning to resume that work in the upcoming weeks.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) from STEEL: has finished the long overdue pending rebase of execution-spec-tests branch for stateless and is planning to start working on the items mentioned in [@ignaciohagopian](https://x.com/ignaciohagopian) testing-framework tech-debt document.

### 2. “A Protocol Design View on Statelessness” presentation

[@_julianma](https://x.com/_julianma) from the EF RIG team presented a recent [Ethresearch post](https://ethresear.ch/t/a-protocol-design-view-on-statelessness/22060) about a general view of statelessness from a protocol design perspective. Both this research post and presentations are highly recommended. The slides are available [here](https://docs.google.com/presentation/d/1dC68cxy6vfnqEjDcBlUb7EycM0_l5wqJL5RYjnIVo3U/edit?usp=sharing).

Summary of the Q&A after the presentation:

- [@gballet](https://x.com/gballet) questioned whether dapps or wallets would be willing to host the state. [@_julianma](https://x.com/_julianma) expressed uncertainty about applications taking that responsibility as the optimal solution. [@gballet](https://x.com/gballet) mentioned it would be a good line of work to continue exploring further by contacting dapps and wallet providers.
- [@MorphNetrunner](https://x.com/MorphNetrunner) from the Portal Network team asked if the Portal network was considered for witness creation. [@gballet](https://x.com/gballet) mentioned that Piper might prefer to wait before promising the network could provide this capability.
- [@_sophiagold_](https://x.com/_sophiagold_) asked if the mentioned out-of-protocol state-expiry rules can help with the state growth problem or any of the drawbacks of known state-expiry solutions, such as address space expansion. [@_julianma](https://x.com/_julianma) said that probably not, but maybe through this optimization problem, framing new avenues could be discovered.
- [@ignaciohagopian](https://x.com/ignaciohagopian) shared a point from [@soispoke](https://x.com/soispoke) (not present in the call) about how transaction witnesses could be resolved in private mempools by actors other than the transaction sender. The challenge remains for high CR guarantees using the public mempool.

### 3. EIP-7612: Contract’s code conversion

The current EIP-7612 proposes that an existing contract code will never be converted to the overlay tree outside the tree sweep. [@kt2am1990](https://x.com/kt2am1990) mentioned this could complicate Besu implementation, thus raising the point if we can reconsider this choice.

[@gballet](https://x.com/gballet) raised the point that this can complicate the implementation but also raised the concern about witness blowup if the existing contract code is to be converted, which can be inconvenient.

[@jasoriatanishq](https://x.com/jasoriatanishq) mentioned that before, we had considered not including execution witnesses in blocks before the full tree conversion is done, which implies execution clients might need to full sync from the snap sync syncing point up to that moment. [@gballet](https://x.com/gballet) mentioned that including partial execution witnesses might be preferred by the geth team to assist in having a better syncing strategy than full sync. Since there’s no consensus around this topic, we might touch on this again in future SIC calls.

Apart from this potential concern, [@ignaciohagopian](https://x.com/ignaciohagopian) noticed a similar drawback which is independent of that decision: execution clients still need to do Overlay Tree writes even without partial execution witnesses, so a malicious block builder can build a block that (indirectly) converts as many 24KiB contracts as possible resulting in a very high load for clients since they need to do a lot of insertions. [@gballet](https://x.com/gballet) shared some quick math on this topic, saying this might not be a concern, but [@ignaciohagopian](https://x.com/ignaciohagopian) mentioned it doesn’t seem to match his intuition.

After checking his assumption offline, it was confirmed by [@gballet](https://x.com/gballet) that the extra leaves would amount to  ~1.8M for a 36M block, which is a good reason not to do that. To be continued in the next SIC

### 4. EIP-7612: Account Header

In the last call, we previously discussed whether, during the Overlay Tree phase before the tree conversion, a transaction only writing to a storage slot triggers the conversion of the account. Considering how geth works, this is the case since the change in storage root is understood as an account basic data change that triggers the mentioned conversion.

In the previous call, there was some slight consensus to keep this behavior, but now that more EL clients have run the tests, this topic has been re-discussed.

[@jasoriatanishq](https://x.com/jasoriatanishq) mentioned he prefers only to move the storage slot without the account data. [@kt2am1990](https://x.com/kt2am1990) mentioned migrating the account isn’t a big problem compared to the previously discussed code conversion rule. [@gballet](https://x.com/gballet) dived into the technicalities of doing it in geth, which could be challenging, and he will be in contact with [@jasoriatanishq](https://x.com/jasoriatanishq) to understand better how this is done in Nethermind.

This topic is closely related to the previous contract code discussion, so we will continue discussing both in the next SIC call, considering any other off-topic talks during the next two weeks.

### 5. Next call agenda

The call ran out of time to discuss the last two agenda items, which will be rolled over to the next SIC call.

## Call #32: March 24, 2025

[Agenda](https://github.com/ethereum/pm/issues/1369)

[Recording video](https://www.youtube.com/watch?v=w9dTNwNq2uU)

### 1. Team updates

[@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth): has been working on EIP-7748 execution spec tests in the context of the previous decision on focusing stateless efforts in gaining confidence on EL clients implementations for the tree conversion.

[@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind: is finalizing the tree conversion implementation in Nethermind and planning to run the execution spec tests next week.

[@kt2am1990](https://x.com/kt2am1990) for @HyperledgerBesu: made progress on general stateless implementation tasks. In particular, he has a preliminary version of the tree conversion, which passed preliminary execution spec tests and uncovered some bugs.

[@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: he started the tree conversion implementation and will probably be able to execute the provided tests in one or two weeks.

### 2. Tree conversion (EIP-7748) testing planning document

[@ignaciohagopian](https://x.com/ignaciohagopian) presented [a new document](https://hackmd.io/@jsign/tree-conversion-tests) describing the strategy and test cases for tree conversion testing.

At the top of the document, a set of bullets quickly describe the main reasons we need to plan test cases to cover in the tests and different stages of testing, including execution spec tests, running devnets with synthetic or testnet/mainnet data, and finally shadow forks of testnets/mainnet.

He continued to present at a high level the different buckets of designed tests, which include testing:

- Non-partial account conversion
- Partial account conversion
- Code-chunk stride accounting
- Special accounts
- Conversion ending
- Modified accounts
- Blocks txs execution with writes overlapping conversion units
- Access partially converted contracts
- Reorgs

The document uses emojis to signal which tests are already created, pending, or require testing-framework changes to be supported. For more details, refer to the [document](https://hackmd.io/@jsign/tree-conversion-tests).

The new fixtures will be shared in the matrix group for EL clients. EL clients should remember that failing tests can have a decent probability of getting bugs in EIP-4762, so it makes sense to share as early as possible if that’s the case for further inspection. After at least one client passes all the tests, failing tests probably signal a bug in your EL client.

Regarding potential geth bugs, [@gballet](https://x.com/gballet) shared that geth has a known bug affecting the Holesky shadow fork run but probably not the testing filling mentioned. The bug is known and can be worked around, but if anyone is trying to build a devnet or shadow fork using the current version, please wait until the bug is fixed.

### 3. Indirect account header conversion when writing storage-slots

[@ignaciohagopian](https://x.com/ignaciohagopian) raised a subtle point on how geth works by describing the following scenario:

- The overlay tree EIP is activated.
- A tx writes to a storage slot in a contract.

Under this scenario, as described in EIP-7612, the write done in the storage slot should happen in the new tree (i.e., overlay tree). But there’s an extra write also done in the new tree: the contract account header.

At first sight, this might be surprising since no actual write happens in the account header, but it is coherent. Currently, under **only** Merkle Patricia Trie's (MPT) assumptions, a write in any storage slot means the _storage root_ of the account _would_ be updated. This marks the account header as dirty, which has also been converted to the new tree. The subtlety is that the account header didn’t have any real data update since the storage root isn’t part of the new tree design, and it also doesn’t make entire sense since the MPT is frozen.

The bottom line question is if this makes sense for all EL clients or if we might want to change the behavior, since another reasonable option is not doing this indirect conversion.

The consensus from [@gballet](https://x.com/gballet), [@g11tech](https://x.com/g11tech), and [@kt2am1990](https://x.com/kt2am1990) seems to be that keeping the current behavior is OK. [@ignaciohagopian](https://x.com/ignaciohagopian) mentioned if EL clients are unsure of how their clients behave, this will be apparent when they run the execution-spec-tests since they’ll disagree with geth, at which point we can re-discuss the topic if needed.

### 4. Testing framework: Preimages

Some days ago, when [@kt2am1990](https://x.com/kt2am1990) was testing some of the first available tests, it was considered that the fixtures could contain a separate field with the MPT preimages required for EIP-4762 logic.

[@ignaciohagopian](https://x.com/ignaciohagopian) explains that geth works today by automatically recording the MPT preimages when it ingests the provided fixtures pre-state. He believes we shouldn’t blow up the testing framework with extra fields if EL clients can derive it from existing data.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) from the testing team made it even clearer that you can interpret the preimages from the pre-state, which should be enough compared to a more explicit field of preimages. In summary, we won’t add the preimages as a new field in fixtures, and clients should continue to record them as they ingest the provided fixture pre-state.

### 5. State Expiry discussion

[@g11tech](https://x.com/g11tech) raised a topic outside the agenda that is still related to the tree conversion topics. Can we reuse the code of the transition to implement state expiry?

[@sophia](https://x.com/_sophiagold_) provided more historical context and confirmed that trying to achieve expiry by using one tree per epoch was very complex since it requires introducing address space extensions and has high UX friction. She provided the following links, which dive deeper into this topic: a potentially [underexplored idea](https://ethereum-magicians.org/t/types-of-resurrection-metadata-in-state-expiry/6607) that could be a viable solution and a [summary from Vitalik from some time ago about state expiry](https://hackmd.io/@vbuterin/state_expiry_paths).

[@ignaciohagopian](https://x.com/ignaciohagopian) mentioned that we should try allocating this topic to a future SIC agenda so we have time to continue the discussion if needed.

### 6. Post-quantum Verkle Trees variants

[@CPerezz19](https://x.com/CPerezz19) [wrote a document](https://hackmd.io/qeEJsgjVSDeMjiR8d3-UvQ?both) that summarizes the main benefits of the current Verkle Tree design and how it compares with other presented alternatives. In particular, it explores potential variants that address the post-quantum weakness of the current design.

He has been talking with many experts in the field and found out there’s active interest in the topic. This might signal an underexplored topic, meaning there might be viable PQ variants for a Verkle Tree design.

The next step is to get more cryptography experts involved in the presented document to have feedback and potentially do some benchmarks to confirm if any of these variants could be a feasible contender to be proposed.

Future SIC calls will continue to discuss this topic since we need to give time for more people to chime into Carlos's work and provide feedback.

### 7. EXTCODECOPY behavior when targeting system contracts

[@gballet](https://x.com/gballet) raised whether doing an `EXTCODECOPY` of a system contract should include system contract code chunks in the witness.

This creates tension with the current EIP spec, which mentions not adding system contract bytecode with few exceptions. The argument behind the current spec is that stateless clients have all system contract code baked into their implementation, so adding this information in the witness is wasteful. This definition would force geth (and potentially other clients) into extra complexity in detecting which cases executed code should be included in the execution witnesses, which can lead to bugs (compared with clients doing direct storage access for system contract implementations).

Both [@g11tech](https://x.com/g11tech) and [@kt2am1990](https://x.com/kt2am1990) agreed that adding the system contract bytecode to the witness for these cases would be fine. The remaining task is opening a PR and changing the spec to clarify this newly decided behavior.

## Call #31: February 24, 2025

[Agenda](https://github.com/ethereum/pm/issues/1322)

[Recording here](https://www.youtube.com/watch?v=Z_8pKQJwqwQ)

### 1. Team updates

[@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth): worked on pre-image file generation and writing the Ethereum stateless book, both of which were shared later in the call.

[@gballet](https://x.com/gballet) for [@go_ethereum](https://x.com/go_ethereum): stateless book reviewing and digging into how a proving system for binary trees would look like.

[@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind: started rebase on Pectra for the next devnet.

[@kt2am1990](https://x.com/kt2am1990) and Thomas for @HyperledgerBesu: made progress on Besu working in stateless mode around state reconstruction and proof verification. Also, starting to work on the state conversion EIP.

[@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: merged the Binary Tree implementation with some tests from the spec and some extra ones. Now working on writing better documentation and the client state manager for this tree.

### 2. Preimage file generation

[@ignaciohagopian](https://x.com/ignaciohagopian) presented the work published in [Ethresearch some days ago](https://ethresear.ch/t/state-tree-preimages-file-generation/21651).

The presentation can be viewed in the video recording between timestamps [04:46](https://youtu.be/Z_8pKQJwqwQ?t=286) and [31:32](https://youtu.be/Z_8pKQJwqwQ?t=1892) (~27 min).

The slides can be accessed [here](https://docs.google.com/presentation/d/1fQkoiQu7geS6un0PpD_y0W9ZJfOmzgE7GnGPRDCPchI/edit?usp=sharing).

Points touched after the presentation:

- Gottfried suggested we make each entry in the preimage file 32 bytes instead of [20-bytes || 32-bytes]. This could simplify cursor management in clients for reorgs since moving back only depends on the sweep stride and not specific block details. The file compression could automatically hide the extra overhead.
- [@gballet](https://x.com/gballet) said that for geth, things might not be simplified much. Gottfried highlighted that the proposal can lead to a more straightforward implementation despite being doable.
- Gottfried raised another point: If we don’t go with the CDN path and have to chunk the preimage information, we’d have to think about verifying each chunk independently, which can introduce more complexity. [@gballet](https://x.com/gballet) mentioned we can solve this with a two-pointer header. Depending on the distribution mechanism we decide on, we might have to dig deeper into this, or we can ignore it.

### 3. Ethereum Stateless Book announcement

[@gballet](https://x.com/gballet) did a quick overview of the recently published [Ethereum Stateless Book](https://stateless.fyi/). Both [@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) worked on this first version.

The book provides a complete picture of many angles about going stateless, such as:

- Motivations
- Target trees
- BLOCKHASH state
- Gas remodeling
- State conversion
- Use cases
- Resources
- Development

Ignacio mentioned that the book is [open source](https://github.com/stateless-consensus/eth-stateless-book), and anyone is invited to collaborate.

### 4. Multi-client State Conversion

[@gballet](https://x.com/gballet) shared that there are current discussions with the research team about the right path forward regarding statelessness to avoid potentially repeating what happened with Verkle Trees.

He highlighted that getting more confidence in how fast clients can be ready for the state conversion can be decisive. He thinks it’s worth inviting more EL clients to work on it so we can show that the state conversion has a low risk.

Reaching a milestone where multiple clients go through a complete conversion of state without consensus bugs might be a more important goal than a new devnet with features that we’ve discussed in previous SIC calls.

[@GabRocheleau](https://x.com/GabRocheleau) asked if we should work on the conversion EIP with Verkle or Binary, and there seems to be an agreement to do it with Verkle. The target tree isn’t important for the goal since the conversion is tree-agnostic. However, doing it with Verkle is easier for clients since most of them already have the implementation ready compared to Binary Trees.

[@ignaciohagopian](https://x.com/ignaciohagopian) mentioned that he would be working on `execution-spec-tests` for the conversion so clients can test their implementations faster than launching devnets. There will be collaboration on this with Spencer from the testing team.

[@gballet](https://x.com/gballet) raised a point about a potential confusion between the data migration order in the [EIP](https://eips.ethereum.org/EIPS/eip-7748) and the preimage file, but the discussion will continue offline. This point may be resumed in further SIC calls, but currently, no action is needed.

## Call #30: February 10, 2025

[Agenda](https://github.com/ethereum/pm/issues/1263)

[Recording here](https://www.youtube.com/watch?v=0dMb2tecuwI)

### 1. Team updates

[@GabRocheleau](https://x.com/GabRocheleau) for @EFJavaScript: The Ethereum JS team has developed an early implementation of binary trees to serve as a testing ground for future developments.

[@kt2am1990](https://x.com/kt2am1990) and Thomas for @HyperledgerBesu: The Besu team has been working on some code refactorings to allow stateless execution of blocks, in particular, only relying on chunkified code chunks for EVM execution. Additionally, the implementation of block-proof verification is being finished.

[@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind: No updates at the moment.

[@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth): drafted the Binary Tree EIP, benchmarked the out-of-circuit performance of hash function candidates, and started working on some documentation projects that will be released soon.

[@gballet](https://x.com/gballet) for [@go_ethereum](https://x.com/go_ethereum): have been upstreaming stateless related changes to geth and collaborating on Binary Tree spec efforts.

### 2. Binary Tree EIP

[@ignaciohagopian](https://x.com/ignaciohagopian) presented an overview of the [Binary Tree EIP](https://eips.ethereum.org/EIPS/eip-7864).

The underlying motivations for a Binary Tree EIP are:

- Concerns about quantum computers becoming a real threat due to breakthroughs last year.
  - Verkle Trees introduce non-quantum-safe components into the protocol, which is in contrast to new efforts, e.g., new research on signature aggregation for CL, and others.
  - Depending on the estimation for quantum computers becoming real, the return on investment of Verkle Trees might not be worth it.
- SNARK proving systems performance improving rapidly, making it more viable to use them for proving pre-state for blocks.
- Jumping directly into a potential final tree: deploying Verkle Trees means we’ll have to do a tree conversion again for quantum. Doing a Binary Tree has higher changes than Verkle in becoming the final tree potentially avoiding further tree conversions.

Regarding the design:

- It pulls many ideas from Verkle Trees EIP
  - Data encoding aggregates accounts data in stems.
  - The same applies to storage slots and code chunks.
  - Code chunkification is the same as Verkle but with minor tweaks.
  - The implementation is much more straightforward than Verkle since it only relies on hash functions, so we don’t have to introduce new elliptic curves or similar structures.
- [Python spec implementation](https://github.com/jsign/binary-tree-spec).
- The hash function for Merkelization:
  - The current proposal uses Blake3 as a first proposal since other hash functions, such as Poseidon2, aren’t considered safe today. EF research is [actively](https://www.poseidon-initiative.info/) working to assess its security more formally.
  - Poseidon2 is probably the best candidate since it’s an arithmetic hash function that is very efficient for proving systems.
  - Given a full implementation of a Binary Tree with Blake3, if we decide to switch to Poseidon2, it would be a simple change, so this dilemma of whether or not to use Poseidon2 shouldn’t be a blocker to starting to implement it.
  - The current proving performance for hash functions which aren't Poseidon2 is still one order of magnitude far from what we need. Poseidon2 proof generation is already fast enough.

Question regarding the current viability of Verkle Trees vs. Binary Trees:

- [@ignaciohagopian](https://x.com/ignaciohagopian) claims Verkle Trees have a low chance of being used.
- Depending on how the L1 scaling roadmap is defined, other changes, such as delayed execution payload, might even favor binary trees.
- It’s pretty hard to know this until it’s discussed in ACD, but call participants seem to agree that Binary Trees now have higher chances of being the right state tree switch.

Regarding future stateless devnets:

- The next devnet will still be Verkle since we only plan to implement EIP-4762 missing features. But further devnets probably will be Binary.
- Binary Tree devnets can start with Blake3 as the hashing function without proof. This can buy time until we decide on the security of Poseidon2 and proving systems. Since later switching the hash function shouldn’t have a significant impact, we aren’t blocked by this.

### 3. Atomicity in gas charging

[@gballet](https://x.com/gballet) explained a proposal from the geth team regarding how some operations in EIP-4762 should have other _atomicity_ semantics.
The motivation from the geth team is simplifying the implementation in their codebase.

The current way EIP-4762 works is that some operations that expect to add more than one leaf to the access events aren’t atomic:

<img src="https://hackmd.io/_uploads/B1D2cTPFkx.png" width="200"/>

Received feedback:

- [@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind claims this change might make their implementation more complex.
- [@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth) raises concerns about whether this change would make the spec more complex since now _atomicity_ in witness additions is something we must explain very well to avoid consensus bugs. Today isn’t required since any addition is expected to be atomic, and there’s no “group level” atomicity concept.

[@gballet](https://x.com/gballet) acknowledged these opinions but thinks the spec is unclear, and might plan to implement the proposed change in geth to confirm if this concludes it might simplify geth implementation or if this was just a wrong intuition. We expect to continue to discuss this proposal in further SIC calls.

### 4. Account version discussion

This is a topic raised by [@gballet](https://x.com/gballet).

[@ignaciohagopian](https://x.com/ignaciohagopian) starts by giving a summary of the motivation for this discussion:

- The current tree proposal has a `VERSION` field.
- There’re motivations to encode into the account stem, information to distinguish if the account is using EIP-7702 delegations and/or is an EOF account.
- The dilemma is between using:
  - The `VERSION` field in the account stem to signal if it’s a usual EoA, EIP-7702 or EOF account, or
  - Keep using `VERSION = 0` and encode this information in the reserved bytes in `BASIC_DATA`.

[@gballet](https://x.com/gballet) goes through further details about potential future use cases for `VERSION` and signals that it seems pretty clear that we shouldn’t use the `VERSION` field. [@jasoriatanishq](https://x.com/jasoriatanishq) and [@ignaciohagopian](https://x.com/ignaciohagopian) agree on the conclusion.

[@ignaciohagopian](https://x.com/ignaciohagopian) gives a final summary:

- `VERSION` describes tree-level semantics, i.e., how to interpret the 256 values in the stem.
- Account level identification is independent from the tree, which justifies not using `VERSION` for this but `BASIC_DATA` reserved fields.

### 5. New stateless team public page and X account

[@gballet](https://x.com/gballet) shares two updates:

- [stateless.fyi](http://stateless.fyi), a new website is a refreshed public explanation about Ethereum's stateless benefits.
- A team [X account](https://x.com/StatelessEth) was created, which people can follow to get updates about Ethereum's stateless efforts.

## Call #28: December 2, 2024

[Agenda](https://github.com/ethereum/pm/issues/1203)

[Recording here](https://www.youtube.com/watch?v=5bxvSLvc9LA)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): started working on EIP draft for binary tries. Also working on new execution spec tests from bugs that were found in latest devnet.

[@g11tech](https://x.com/g11tech) for [@EFJavaScript](https://twitter.com/EFJavaScript): been playing with devnet-7, and resolving some recently found issues.

[@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): Working on implementing the blockhash gas cost modification. Also implementing the proof verification. This will unblock some other stuff like Verkle sync (snap sync equivalent), and validating proof coming from the other client. Also working on optimizing stem generation by modifying how we call the Rust Verkle library.

### **2. Execution spec tests**

Update from Ignacio, sharing [v0.0.8](https://github.com/ethereum/execution-spec-tests/releases/tag/verkle%40v0.0.8).

After we previously launched devnet-7, we found a consensus bug with Nethermind. Debugged with Tanishq and found an edge case. Created a new test case to cover this. Idea going forward is that every bug we find in any devnet should result in a new execution spec test to cover it. Any new client that joins the next devnet will also be covered by these cases.

### **3. Preimage distribution and Portal Network**

[Piper](https://twitter.com/pipermerriam) from Portal Network joined to share some thoughts on preimage distribution with Portal.

Regarding preimage distribution for the Verkle migration: Piper mentioned that Portal can solve this, but probably isn’t the best solution. Portal is best for clients who want to grab a subset of the data on demand. With the preimage problem for Verkle, all of the clients would need to grab the full set of preimages. But there could be a “file-based approach” that could make sense in this case.

The file-based approach: S3 buckets being able to generate the file, and potentially having pre coded S3 buckets in clients that are already distributed. Has a predistributed trust model. Distributing big files like this from S3 buckets is a pretty straightforward approach. Alternative to S3 buckets, could use torrents.

Guillaume mentioned that Lukasz from Nethermind had previously indicated a preference for using an in-protocol p2p approach. But added that it seems clear that the CDN approach is the simplest and should probably go with that.

Next steps: make a spec on a potential format for the file.

### **4. State expiry**

Hadrien from OpenZeppelin joined to share some thoughts on state expiry.

One related question that was discussed in Devcon: do we want to resurrect based on reads or based on writes?

Hadrien: most of the storage accesses are as you expect related to reading and writing. And so one question is whether a read would extend the lifetime of an extension or not. There are a few slots that are being read, but never written to. For example in the case of ERC-721, when tokens are transferred the slot will often contain a zero because nobody is allowed to take the token. And anytime there is a transfer of that token, in the current implementation it will write another zero to reset, even if old value is a zero, because its cheaper than trying to read. Depending on how state expiry works, there may be different cost model depending on whether there is a zero because it was never written to the state, compared to if a zero gets explicitly written.

This is where the approach of state expiry shown in EIP-7736 comes in. Han joined to share some updates on this front.

Han is currently working on implementing 7736, and the changes needed on the Verkle part are done. Still working on the geth part. Once we get all the components complete and integrated can hopefully have a devnet to start testing out the various scenarios.

### **5. Stateless Transactions EIP**

Gajinder shared some of the latest thinking around changes needed to best support stateless clients.

One of the challenges is that stateless clients which don’t maintain any execution state would not be able to do any kind of local block building. This proposal is a partial remedy to this problem.

The basic idea is that with every transaction the transaction submitter will also have to submit an execution witness, and that execution witness will have a state diff + proof of the state diff. The execution witness would also bundle the parent state root, which is what a builder would look at to see whether it can include this tx in the particular block that it’s building.

Gajinder is currently drafting the EIP. Will have something to share soon.

## Call #27: November 4, 2024

[Agenda](https://github.com/ethereum/pm/issues/1196)

[Recording here](https://www.youtube.com/watch?v=1PYu4_Ac1Po)

[no notes this week]

## Call #26: October 21, 2024

[Agenda](https://github.com/ethereum/pm/issues/1186)

[Recording here](https://youtu.be/MJA1e95cfww)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): added around 30 new tests in the execution test spec repo, mostly covering edge cases regarding out of gas execution transactions. (This is relevant for Verkle because if you run out of gas, you might generate a partial witness). Guilluame has been working on speeding up witness generation, as well as the rebase on top of Cancun to get updated metrics.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): testing and debugging latest hive tests. Also have been progressing on the Nethermind implementation of the transition.

[@lu-pinto](https://github.com/lu-pinto) and [@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): Working on gas costs. Currently 2 tests are failing (regarding self-destruct). Also all of the `BLOCKHASH` tests are failing because the logic for pulling out the `BLOCKHASH` from the system contract is not yet implemented, but that’s up next. Have also completed some optimizations on the rust-verkle crypto library. And now have a working version of the flatDB based on stem. Next step here is to generate the preimage to be able to run this flatDB with mainnet blocks and compare performance. Lastly, continuing to work on integrating the [Constantine](https://github.com/mratsim/constantine) crypto library into Besu.

### **2. Circle STARKs seminar**

[Matan](https://sites.google.com/view/matanprasmashomepage) joined to share some info on an upcoming [SNARK-focused seminar](https://forms.gle/n3BtvJXLGrfbUPkV8) he will be leading, which will be open for anyone currently working on stateless. The seminar will be focused on bridging the gaps for upcoming items on the Verge roadmap, in particular exploring the topics of SNARKing Verkle, Circle STARKs, and STARKed binary hash trees (as a potential alternative to Verkle). No prerequisites or math background required for the course. Idea is accessible to lay audience. The seminar will likely be weekly and run for TBD number of weeks.

[Apply to join here!](https://forms.gle/n3BtvJXLGrfbUPkV8)

### **3. Verkle Metrics**

Guillaume shared an [updated document](https://www.notion.so/Verkle-measurements-123d9895554180e6ac17eddf76c692b6?pvs=21) which provides a helpful overview of latest Verkle metrics. This is data collected by replaying ~200k historical blocks (around the time of the Shanghai fork). While they don’t provide a perfect prediction for how things will look in the future, it does help give a solid approximation of what to expect.

Few highlights below, and check out the doc for the full report.

#### **Witness size**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_74d2e508ca3499afb6064b4af670e4cf.png)

#### **Database**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_71000583740af6e688511c8562c48227.png)

#### **Tree key hashing: comparing Pedersen vs sha256**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_ce9bc497d137bad837dbf5f8e1661569.png)

### **4. Spec Updates**

Last up on this week’s call, a few quick points related to gas cost spec:

- `CREATE` gas cost: currently 32,000. But for Verkle, we’ve considered reducing this.
  - Decision: keep as-is for now.
- How much to charge when doing account creation (e.g. when doing a call and the address does not exist yet). And similar question for `SELFDESTRUCT`
  - Decision: open a convo with research team for further discussion

## Call #25: September 23, 2024

[Agenda](https://github.com/ethereum/pm/issues/1149)

[Recording here](https://youtu.be/pfORU9ngjzI)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): just about ready for testnet relaunch. Most PRs merged. Also working on benchmarking. Some updates to the test fields, and fixed some tests around witness checks. Can also run the tests in stateless mode soon, where the witness acts as the prestate source of truth.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): no major updates. Working with Ignacio on tests. Will try to run the tests in stateless mode this week 🔥

[@kt2am1990](https://twitter.com/kt2am1990) and [@lu-pinto](https://github.com/lu-pinto) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): last week, working on the flat DB based on stem key. Completed some benchmarking to see impact on SLOAD. Found the perf was quite bad. Working with Kev on some optimizations. Also working on integrating [Constantine](https://github.com/mratsim/constantine) library into Besu, in order to compare perf. Made some good progress on getting the test fixtures to run. Will get back to finalziing gas costs after.

### **2. EIP-7702 in Verkle**

We quickly went through a [PR from Guillaume](https://github.com/ethereum/EIPs/pull/8896) to update the stateless gas costs EIP in order to support changes coming in [7702](https://eips.ethereum.org/EIPS/eip-7702). (Current plan is for 7702 to be included in Pectra, & 7702 contains a new type of tx: `authorization_list`. Info that is used to update some accounts)

TLDR is this updates the gas costs EIP so if you call these functions (`EXTCODESIZE`, `EXTCODEHASH` etc), then you also need to touch the `CODEHASH_LEAF_KEY`. Ping Guillaume with any comments/questions.

### **3. Partial Witness Charging**

Ignacio walked through a few scenarios where a bit more granularity may be needed around gas costs in Verkle. [See PR here.](https://github.com/gballet/go-ethereum/pull/495)

Scenario 1: if you don’t have enough gas to pay for whatever witness charging you have to do, then you don’t actually need to include that in the witness. (e.g. if you do a jump and don’t have enough available to pay, then you wouldn’t include that cold chunk in the witness)

Scenario 2: there are several places in execution where you have to include more than one leaf in the witness. In these cases, Geth was previously always including both leaves. Even if you didn’t have enough gas, it was already added to the witness.

For both scenarios we want to allow for better granularity / partial witness charging.

TLDR: only add things to the witness if you have the available gas for it

### **4. [Testnet-7 Check-in](https://kaustinen-testnet.ethpandaops.io/)**

Ending things with a quick check on testnet readiness. Pari said DevOps should have time to assist later this week. Recommended doing it locally first in case bugs are found, and then once it works locally switch to a public testnet 🥳

## Call #24: September 9, 2024

[Agenda](https://github.com/ethereum/pm/issues/1149)

[Recording here](https://youtu.be/TTqikpo4R7g)

### **1. Team updates**

Gary for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): continuing work on stem-based flat database, and have a working implementation. Will simplify sync for Besu. Also making progress on gas cost changes and witness updates (EIP-4762). And planning on getting back to work on the transition stuff soon.

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): last week, discussed sync with the Geth team, and would like to figure out how performant it is to compute all of the leaves as they are needed (reading SLOAD + building a snapshot based on those leaves). Interested to understand Besu’s performance in this regard. Also completed a bunch of work on the testing framework. Gas costs are ready to go. Should be ready for new testnet in next few days.

[@GabRocheleau](https://twitter.com/GabRocheleau) for [@EFJavaScript](https://twitter.com/EFJavaScript): continued work to prepare for the upcoming testnet. Running the actual tests this week. Also made some upgrades to the [WASM cryptography library](https://github.com/ethereumjs/verkle-cryptography-wasm) so can begin creating proofs, and allows for stateful verkle state manager. Previously could only run blocks statelessly.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): mostly been working on getting the Hive test running. Found and fixed a few bugs. One change in the spec around gas costs, and continuing work on the transition. Working on some cryptography improvements as well.

Somnath for [@ErigonEth](https://twitter.com/erigoneth): integrating the gas cost changes and the witness calculation changes. Noticed some issues with the state management, but hopeful can resolve it in next week. Erigon has already started migrating everything to [Erigon 3](https://erigon.tech/the-rise-of-erigon3-and-our-new-release-cycle/), but current verkle work is based on Erigon 2. Will try to join devnet-7 soon after it launches.

[@g11tech](https://x.com/g11tech) for [@lodestar_eth](https://x.com/lodestar_eth): rebasing on latest from lodestar, which should provide a more stable base for testnet because of improved performance.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: we have the witness assertions now within the framework when filling tests. Can write a test, and assert that a balance is what you expect. More of a sanity check. Now have similar for witness-specific values. Next step is to optimize how the tests are filled, and improve the transition tool runtime.

### **2. Devnet Readiness**

There’s a couple updates to the specs that are still open. Quickly reviewed with Guillaume 3 PRs to merge in asap for the testnet:

https://github.com/ethereum/EIPs/pull/8867

https://github.com/ethereum/EIPs/pull/8707

https://github.com/ethereum/EIPs/pull/8697

^ Leaving open for comments, but no objections raised during the call on these PRs.

### **3. Verkle, Binary, & Tree-agnostic development**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_12245bd6275db8c90b1255d273af807a.png)

Quick recap of recent conversations we’ve had around the tradeoffs of Binary vs. Verkle:

This is a topic that has come up often over the past few years, and even going all the way back to 2019/2020 – when [@gballet](https://x.com/gballet/status/1278259913454690304) was an author of [EIP-3102](https://eips.ethereum.org/EIPS/eip-3102): an initial proposal to migrate to a binary trie structure.

More recently, as many teams have made strong progress on the ZK proving side, there’s been renewed discussion around whether a ZK-based solution could be ready in a similar timeframe to Verkle (or soon thereafter), and allow us to skip straight to a fully SNARKed L1.

In this scenario, a binary trie structure is arguably preferable, in that it’s a friendlier option to current ZK proving systems, despite Verkle’s advantages in other dimensions (such as smaller tree/proof size and slower state growth). While Verkle is closer to “mainnet ready” today, it’s possible the gap closes over the next 1-2 years.

The challenge and discussion now is mostly centered around how we can optimize forward progress on R&D efforts, solve problems facing users today, while also making sure we properly evaluate other viable/evolving technologies to ensure we land on the best long-term path for the protocol + users.

**TLDR:**

it’s safe to say we plan on doing a bit more of at least two things over the next ~3-6 months:

(1) evaluate binary: invest meaningful bandwidth into exploring / benchmarking a binary tree structure, while collaborating closely with zk teams. Make sure we understand where we are today in terms of performance, hardware requirements (with which hash function etc.), and where things need to be in order to be viable on L1.

(2) tree-agnostic development: continue building the infrastructure and tooling necessary for statelessness, but lean into a tree-agnostic approach to optimize for reusability. This will give us flexibility to land on the best solution, whether it’s Verkle, Binary, or anything else. In any case, much of what has already been built (e.g. for state migration) will be a valuable and necessary component since it’s unlikely we stick with the current MPT for long.

If you are excited about making progress on statelessness and scaling the L1, you can join the conversation in our biweekly implementers call 🚀

### **4. Deletions in Verkle**

Discussion around whether _not_ having deletions in Verkle will bloat the state. There are also downsides to deletions though, as it may make the conversion process a bit more complicated. TBD on final decision, but no strong objections raised to supporting deletions in Verkle. Recommend watching the recording for anyone interested in better understanding the full picture on this topic.

### **5. Pectra Impact (7702, EOF, etc.)**

Ignacio gave an overview of some work he’s done to better understand the potential impact of EOF on Verkle. He created a [draft PR](https://github.com/jsign/EIPs/pull/2) with the changes required. Guillaume also shared some thoughts on things we need to be mindful of with [7702](https://eips.ethereum.org/EIPS/eip-7702). Namely around making sure that when you add something to the witness, you add the contract that the operation is delegated to instead of the account itself (otherwise the witness will be empty).

### **6. Verkle Sync**

Geth team recently had a discussion on the topic of sync, and came away with a few potential suggestions.

(1) around witness validation: full nodes can validate witnesses and make sure that no extra data is being passed as a way to prevent bloating of the witness. (e.g. flag a block that has too many leaves as invalid). Note: if we do introduce this rule, then we have to make the witness itself part of the block.

(2) snapshot per stem, rather than by hash like it currently is.

(3) how long to save the witness on disk: if we save it for something like a month or so, then it makes it a bit simpler for nodes who have been offline for a short period of time (e.g. 2-3 weeks) to rejoin the network. The tradeoff is it would add around 60GB.

## Call #23: August 26, 2024

[Agenda](https://github.com/ethereum/pm/issues/1121)

[Recording here](https://youtu.be/VD0P3RkhIjY)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): making solid progress on testing and the upcoming testnet. Managed to get the branch with latest gas schedule working with tests.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): have implemented everything for the testnet. Also working on a few cryptography improvements, and a big refactor to implement the transition. Might be able to implement test transition in next few weeks.

[@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): working on modifying how we save data in the DB, to implement a flat DB based on the stem of the tree. Also working on gas cost implementation and the transition.

[@GabRocheleau](https://twitter.com/GabRocheleau) for [@EFJavaScript](https://twitter.com/EFJavaScript): ready with all the gas cost updates, likewise just waiting for test vectors to confirm.

Somnath for [@ErigonEth](https://twitter.com/erigoneth): getting caught up with the changes up to the most recent testnet. Had an issue when trying to connect to peers on testnet-6, not getting any replies. Will debug with ops.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: have released latest fixtures, can be found [here](https://github.com/ethereum/execution-spec-tests/releases/tag/verkle%40v0.0.3).

### **2. Testing Verkle overview**

Next up, we had a brief [presentation](https://hackmd.io/@jsign/verkle-testing) from [@ignaciohagopian](https://x.com/ignaciohagopian) to walk through latest overall progress on the test framework for Verkle. The main branch for the test vectors can be found [here](https://github.com/ethereum/execution-spec-tests/tree/verkle/main) in the execution spec tests repo. And to look at existing tests you can find them [here](https://github.com/ethereum/execution-spec-tests/tree/verkle/main/tests/verkle) in the tests/verkle folder, where all the tests are separated by EIP. Encourage anyone interested to poke around, ask questions, and eventually open up new test cases :)

Ignacio also gave an overview of the changes that were made in geth to support the test framework, how the CI pipeline works, and a summary of all the test fixtures that exist today. The fixtures can be separated into 3 groups:

(1) Verkle-genesis, where everything happens in a post-merkle patricia tree (MPT) world,

(2) overlay-tree, which is running tests with a “frozen” MPT, and doing the block execution in the Verkle tree,

(3) consuming tests from previous forks, to check pre-Verkle execution isn’t broken

![image](https://storage.googleapis.com/ethereum-hackmd/upload_dfdf8c57c53353d60dc2465740f77117.png)

### **3. Testnet readiness check**

Next up, we went through and double checked each team’s readiness for the next testnet. Geth, Nethermind, and EthJS are ready as far as we can tell at this point, while Besu is still finishing up gas cost updates.

### **4. Binary Tree Exploration**

Last up, Guillaume shared a presentation which summarizes some recent discussions we’ve had, as there’s been a bit of a renewed push by some in the community to explore binary trees as a potential alternative to Verkle. Some of this push has been motivated by recent progress made in ZK proving performance, and a desire to provide something that is even more zk-friendly than Verkle to help accelerate the move towards an eventual fully SNARK-ified L1.

Highly recommend watching the recording to view Guillaume’s full presentation, but to give a rough TLDR: the main advantage of Verkle (apart from the fact that progress on Verkle is much further along today compared to a binary tree alternative) is that Verkle gives us small proofs (~400kb). And these proofs can enable stateless clients pretty much as soon as we ship Verkle.

On the other hand, the advantages of binary trees is that hashing performance / commitment computation are much faster in general. And are also more compatible with current ZK proving schemes. The other advantage is around quantum resistance, though of course still much debate around timelines for quantum, and there are several other areas of the protocol that will need to be upgraded as part of any post-quantum push.

The good news is: in either case, much of the work we’ve already done with Verkle is reusable in a binary tree design: (1) gas cost changes, (2) the single-tree structure, (3) the conversion & preimage distribution, and (4) (potentially) sync.

What would change with binary trees though include: (1) the proof system, and (2) the cryptography (replacing polynomial commitments/pedersen with hashes)

## Call #22: July 29, 2024

[Agenda](https://github.com/ethereum/pm/issues/1119)

[Recording here](https://youtu.be/W1SLIEQ3a5o)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): working on implementing the fill_cost updates for next testnet. Also did a series of measurements around witness size, and opened an EIP for the state transition. Will share more on this later in the call.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): have implemented all the changes discussed for testnet7. Still need to pass the Hive test.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: Spencer gave a quick summary of the current tests available for Verkle. At the moment, we only have transition tests, and no state tests. Will work on getting the remaining tests ready for the newest test cases that Ignacio has written, so we can run those prior to cutting the next testnet. Ideally will also soon make it easy for everyone to just run a command to build and test everything. Can find the latest Verkle tests [here](https://github.com/ethereum/execution-spec-tests/releases/tag/eip6800%40v0.0.1).

### **2. SSZ and witness size improvements**

[@gballet](https://x.com/gballet) shared a summary of three different approaches we are exploring around the encoding of witnesses. The current spec as-is results in unnecessarily large witnesses, due to some nuance around the handling by SSZ libraries. So we are exploring alternative approaches. Recommend watching the full recording to get all the details. But the tldr is that these alternative approaches look very promising, and should reduce witness size by 50% or more, to where the median witness size will be under 300kb, and the max witness size will be under 1MB. There are some future potential optimizations Guillaume is looking into as well which may reduce witness size by another 50%.

### **3. Kaustinen updates (testnet #7)**

We took a few mins to go through any last minute changes or additions we’d like to include in the next testnet: Kaustinen 7. The first topic was revisiting the verkle proof format, and changing it from SSZ/JSON into an opaque binary format. It’s generally agreed we should do it in the long-run, but still a question of whether to do it for this next testnet. Decision on the call was to wait and not do this in Kaustinen 7.

The next topic on Kaustinen 7 was whether to include the fill_cost updates. Decision on the call was to not include it, because it would slow things down significantly, and require a tricky rebase (merging it with more recent code).

### **4. EIP for the Verkle state transition**

Last up, [@ignaciohagopian](https://x.com/ignaciohagopian) walked us through a few recent PRs he put up with quick fixes to EIP-7612 and EIP-6800, as well as a new EIP for the actual transition (where we migrate all state from the Merkle Patricia Tree over to the Verkle Tree). This new EIP can be found at [EIP-7748](https://github.com/ethereum/EIPs/pull/8752).

The main goal of this new EIP is to formalize the state conversion algorithm that is being implemented by clients. (This is the same design that we have discussed for several months, but simply hadn’t been formalized into an EIP). Check out the linked PR for a more detailed explanation, but the short summary is that the state migration involves converting a fixed number of key-values each block (e.g. 10k values) over an extended period of time (e.g. 4 weeks). The number of key-values converted each block can be adjusted up or down, but is currently set to a conservative number to ensure that even modest hardware is able to keep up with the transition. This has already been successfully implemented in Geth, and more clients are following soon behind.

## Call #21: July 15, 2024

[Agenda](https://github.com/ethereum/pm/issues/1092)

### **1. Team updates**

@ignaciohagopian and @gballet for @go_ethereum: finishing up document on code chunking gas cost overhead in Verkle. Also working on more test vectors for the state conversion (converting state from Merkle to Verkle). And putting together first draft of the state conversion EIP, to help with coordination and act as the source of truth for test vectors. In addition, Guillaume has been collecting witness size data, preparing for the testnet relaunch, and busy merging more Verkle stuff into Geth so we can have a testnet on top of Dencun.

@DoctZed for @HyperledgerBesu: currently optimizing around storage in the DB, and moving some of the work to background threads. Also finishing up the updates to gas cost schedule.

@techbro_ccoli for testing team: continuing work on the genesis test vectors. Also discussed a bit with Guillaume about making the tests runnable by anyone, so that each client team can check prior to relaunching the next testnet. @techbro_ccoli to put together a document on how to run the tests easily. Can find the tests here: https://github.com/ethereum/execution-spec-tests/releases

### **2. Testnet readiness**

We went through the main items we want to include on the next testnet, to see where client teams are in terms of readiness. Geth is still working through a few items: notably fill_cost and other gas cost updates. Nethermind has implemented everything previously discussed at the client team interop, but still need to run latest test framework to verify. Besu is not yet complete, and need to wrap up some of the DB work they are currently in process of refactoring before finishing up testnet work.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_39205c7cb2c60e94ed208611c141230f.png)

### **3. Witness size measurements**

Next up, Guillaume shared some recent experiments he has been running around witness sizes, as well as some potential optimizations. Tldr: in the current version of the spec we store the state diff in a certain way with the old value and new value as optionals.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_9ebabc709fdfd0a38b3fb3694e87040b.png)

The issue with this approach is that very few SSZ libraries support optionals, and due to how other libraries handle this it makes for unnecessarily large witnesses.

So, one proposal to fix this: get rid of optionals by grouping all the suffixes together in an array of bytes, and grouping all the old values and new values likewise in their own lists.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_232fbaad5b7d3b0d15de3329986a5279.png)

Testing this new approach (vs. the current approach) results in significant reduction in witness size. The current approach has a median witness size of ~700kb. While the new approach has a median witness size of ~400kb. One note: we can further reduce the witness size significantly (possibly another 50%) by removing new_values, which would put witness size closer to 200kb.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_4df9b14961c3c2037fd05b8006262085.png)

Guillaume also shared a 2nd proposal for an optimization which could further reduce witness size. (Still need to implement it to get the numbers). Will come back to this discussion on a future call.

### **4. Other Misc EIP Updates**

Next up, we went through a few updates to EIPs including EIP-6800 and EIP-2935. See the complete list here: https://notes.ethereum.org/@gballet/S1YEC5fOA#/5.

This is mostly relevant for client devs and other implementers, but recommend watching the recording for anyone interested!

### **5. Code-access Gas Overhead**

Last up, @ignaciohagopian walked us through a document he put together, which provides a very helpful analysis of the cost of code chunking in Verkle.

For those not familiar: in order to fully support stateless clients in a post-Verkle world, we will have to add contract code to the tree (in addition to account data and contract storage). This is necessary for clients to statelessly validate blocks. This process of transforming contract bytecode into tree key-values is called “code-chunking”. But code-chunking is not free, and will add some new gas cost overhead which doesn’t exist today in a stateful world.

Ignacio’s document does a great job at summarizing, analyzing, and approximating what these costs will be with Verkle by looking at ~1 million recent mainnet transactions. The TLDR is that, on avg, we should expect around 30% gas cost overhead. The good news is there are several ways we can potentially mitigate this. Importantly, paying this extra cost will unlock stateless clients, which will allow us to significantly raise the gas limit (more than 30%) to more than offset this additional cost. See https://x.com/dankrad/status/1790721271321256214.

Highly recommend watching the recording, and also going through Ignacio’s full document here for anyone curious to learn more: https://hackmd.io/@jsign/verkle-code-mainnet-chunking-analysis.
