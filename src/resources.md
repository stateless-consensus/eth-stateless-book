# Resources

- [Resources](#resources)
  - [Introductory material](#introductory-material)
  - [Video presentations](#video-presentations)
  - [Advanced material](#advanced-material)
    - [Overviews](#overviews)
    - [Advanced technical write-ups](#advanced-technical-write-ups)
    - [EIPs](#eips)
    - [Measurements \& Metrics](#measurements--metrics)

Over the past years, many articles and talks have been made about stateless, mainly on the topic of [Verkle Trees](./trees/vkt-tree.md). With recent updates to the roadmap, we will continue expanding this section, increasing our coverage of [Binary Trees](./trees/binary-tree.md) as well.

## Introductory material

- [Why stateless?](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html) - Dankrad Feist’s introduction on his blog
- [Possible futures of the Ethereum protocol, part 4: The Verge](https://vitalik.eth.limo/general/2024/10/23/futures4.html) - Vitalik’s latest update on the stateless roadmap
- [Verkle Trees](https://vitalik.eth.limo/general/2021/06/18/verkle.html) - Vitalik’s intro on Verkle Trees

## Video presentations

- [Anatomy of a stateless client - Guillaume Ballet, April 2024](https://www.youtube.com/watch?v=yFJxVSbQNcI&pp=ygUdYW5hdG9teSBvZiBhIHN0YXRlbGVzcyBjbGllbnQ%3D): an overview of how stateless improves client maintainability
- [Recipes for a Stateless Ethereum](https://www.youtube.com/watch?v=gfzkidjJf8g) - Guillaume Ballet, March 2024: a good summary of the use cases of stateless Ethereum
- [Verkle Trees 101 - Guillaume Ballet, Ignacio Hagopian, Josh Rudolf, April 2024](https://www.youtube.com/watch?v=H_M9bjwtMhU) - an overview of the EIPs used in Verkle Trees (a lot of it still current for Binary Tress)
- [Verkle sync: bring a node up in minutes - Guillaume Ballet, Tanishq Jasoria, September 2023](https://www.youtube.com/watch?v=AJDJvMS8LIE) - a presentation of how stateless clients improve sync algorithms
- [The Verge: Converting the Ethereum State to Verkle Trees - Guillaume Ballet, July 2023](https://www.youtube.com/watch?v=F1Ne19Vew6w) - a presentation of the conversion process
- [Ava Labs Systems Seminar: Verkle trees for statelessness - Guillaume Ballet, October 2023](https://youtu.be/uGNmG3ZpWlU?si=OEFWP8Vesz-NRU9g) - a comprehensive overview of the states of stateless development as of October 2023
- [DevCon: How Verkle Trees Make Ethereum Lean and Mean - Guillaume Ballet, Oct 2022](https://www.youtube.com/watch?v=Q7rStTKwuYs&t)
- [Verkle Tries for Ethereum State - Dankrad Feist, Sept 2021](https://www.youtube.com/watch?v=RGJOQHzg3UQ&t) - PEEPanEIP presentation

## Advanced material

### Overviews

- [Overview of tree structure](https://blog.ethereum.org/2021/12/02/verkle-tree-structure) - a blog post describing the structure of a Verkle Tree, which is still current with the Binary Tree proposal.
- [Overview of cryptography used in Verkle](https://hackmd.io/PgsD0I0dQHOGuDx7D6o-dg#Cryptography-used-in-Verkle-Tries)
- [Anatomy of a Verkle proof](https://ihagopian.com/posts/anatomy-of-a-verkle-proof) - same as above
- [Stateless and Verkle Trees](https://www.youtube.com/watch?v=f7bEtX3Z57o) (video) - a dated presentation giving an overview of the stateless effort and Verkle Trees in particular

### State expiry

 - [https://ethresear.ch/t/state-expiry-in-protocol-vs-out-of-protocol/23258](https://ethresear.ch/t/state-expiry-in-protocol-vs-out-of-protocol/23258)
 - [https://ethresear.ch/t/the-future-of-state-part-1-oopsie-a-new-type-of-snap-sync-based-wallet-lightclient/23395/1](https://ethresear.ch/t/the-future-of-state-part-1-oopsie-a-new-type-of-snap-sync-based-wallet-lightclient/23395/1)
 - [https://ethresear.ch/t/the-future-of-state-part-2-beyond-the-myth-of-partial-statefulness-the-reality-of-zkevms/23396/1](https://ethresear.ch/t/the-future-of-state-part-2-beyond-the-myth-of-partial-statefulness-the-reality-of-zkevms/23396/1)
 - [https://ethresear.ch/t/compression-based-state-expiry/23443](https://ethresear.ch/t/compression-based-state-expiry/23443)

### Analysis

 - [https://ethresear.ch/t/ethereum-bytecode-and-code-chunk-analysis/22847](https://ethresear.ch/t/ethereum-bytecode-and-code-chunk-analysis/22847)
 - [https://ethereum-magicians.org/t/not-all-state-is-equal/25508/3](https://ethereum-magicians.org/t/not-all-state-is-equal/25508/3)
 - [https://ethresear.ch/t/data-driven-analysis-on-eip-7907/23850](https://ethresear.ch/t/data-driven-analysis-on-eip-7907/23850)
 - [https://ethresear.ch/t/a-small-step-towards-data-driven-protocol-decisions-unified-slowblock-metrics-across-clients/23907](https://ethresear.ch/t/a-small-step-towards-data-driven-protocol-decisions-unified-slowblock-metrics-across-clients/23907)

### State gas costs

 - [https://ethereum-magicians.org/t/the-case-for-eip-8032-in-glamsterdam-tree-depth-based-storage-gas-pricing/25619](https://ethereum-magicians.org/t/the-case-for-eip-8032-in-glamsterdam-tree-depth-based-storage-gas-pricing/25619)
 - [https://ethereum-magicians.org/t/eip-8058-contract-bytecode-deduplication-discount/25933](https://ethereum-magicians.org/t/eip-8058-contract-bytecode-deduplication-discount/25933)

### Misc

 - https://ethresear.ch/t/a-short-note-on-post-quantum-verkle-explorations/22001

### Advanced technical write-ups

- [Inner Product Argument - Dankrad Feist](https://dankradfeist.de/ethereum/2021/07/27/inner-product-arguments.html)
- [PCS Multiproof - Dankrad Feist](https://dankradfeist.de/ethereum/2021/06/18/pcs-multiproofs.html)
- [Deep dive into Circle-STARKs FFT - Ignacio Hagopian](https://ihagopian.com/posts/deep-dive-into-circle-starks-fft)

### EIPs

#### Included

- [EIP-2935](https://eips.ethereum.org/EIPS/eip-2935), save historical block hashes in the state
- [EIP-6780](https://eips.ethereum.org/EIPS/eip-6780), deactivate SELFDESTRUCT

#### Championned

- [EIP-4762](https://eips.ethereum.org/EIPS/eip-4762), gas costs changes for Verkle Trees
- [EIP-7709](https://eips.ethereum.org/EIPS/eip-7709), bypass the EVM to read block hashes in the state
- [EIP-7612](https://eips.ethereum.org/EIPS/eip-7612) and [EIP-7748](https://eips.ethereum.org/EIPS/eip-7748), about the state tree conversion
- [EIP-7864](https://eips.ethereum.org/EIPS/eip-7864), about the proposed scheme for Binary Trees
- [EIP-8037](https://eips.ethereum.org/EIPS/eip-8037), State Creation Gas Cost Increase
- [EIP-8038](https://eips.ethereum.org/EIPS/eip-8038), State-access gas cost update
- [EIP-8125](https://eips.ethereum.org/EIPS/eip-8125), Temporary Contract Storage

#### Proposed, but not actively championned

- [EIP-2584](https://eips.ethereum.org/EIPS/eip-2584), Trie format transition with overlay trees
- [EIP-2926](https://eips.ethereum.org/EIPS/eip-2926), MPT-based code chunking
- [EIP-3102](https://eips.ethereum.org/EIPS/eip-3102), binary trees
- [EIP-6800](https://eips.ethereum.org/EIPS/eip-6800), structure of a Verkle Tree
- [EIP-7736](https://eips.ethereum.org/EIPS/eip-7736) (verkle) leaf-based state expiry
- [EIP-7545](https://eips.ethereum.org/EIPS/eip-7545), proof verification precompile
- [EIP-8032](https://eips.ethereum.org/EIPS/eip-8032), Size-Based Storage Gas Pricing
- [EIP-8058](https://eips.ethereum.org/EIPS/eip-8058), Contract Bytecode Deduplication Discount

### Measurements & Metrics

- [Verkle Metrics](https://verkle.info/verkle-measurements)
- [State Tree Preimages Generation](https://ethresear.ch/t/state-tree-preimages-file-generation/21651)
