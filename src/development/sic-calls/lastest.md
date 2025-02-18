# Lastest SIC call

## [**Call #30: Febuary 10, 2025**](https://github.com/ethereum/pm/issues/1263)

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

[@gballet](https://x.com/gballet) explained a proposal from the geth team regarding how some operations in EIP-4762 should have other *atomicity* semantics.
The motivation from the geth team is simplifing the implementation in their codebase.

The current way EIP-4762 works is that some operations that expect to add more than one leaf to the access events aren’t atomic:

<img src="https://hackmd.io/_uploads/B1D2cTPFkx.png" width="200"/>

Received feedback:

- [@jasoriatanishq](https://x.com/jasoriatanishq) for Nethermind claims this change might make their implementation more complex.
- [@ignaciohagopian](https://x.com/ignaciohagopian) from [@StatelessEth](https://x.com/StatelessEth) raises concerns about whether this change would make the spec more complex since now *atomicity* in witness additions is something we must explain very well to avoid consensus bugs. Today isn’t required since any addition is expected to be atomic, and there’s no “group level” atomicity concept.

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
