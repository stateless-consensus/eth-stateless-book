# Preimage generation and distribution

- [Preimage generation and distribution](#preimage-generation-and-distribution)
  - [Recommended background](#recommended-background)
  - [Context](#context)
  - [Preimages file](#preimages-file)
    - [Distribution](#distribution)
    - [Verifiability](#verifiability)
    - [Generation and encoding](#generation-and-encoding)
  - [Usage](#usage)

[Note: this page is heavily based on an [existing research post](https://ethresear.ch/t/state-tree-preimages-file-generation/21651?u=ihagopian) from one of the book authors]

## Recommended background

This topic is diving into a topic that can’t be understood correctly in isolation; we recommend previous reading of:

- The [State Conversion high-level explanation](./intro.md).
- [EIP-7612 explanation](./eip-7612.md).
- [EIP-7748 explanation](./eip-7748.md).

## Context

As mentioned in the [EIP-7748 Preimages section](./eip-7748.md#preimages), we need to calculate new keys when *conversion units* are moved from the MPTs to the new tree. This calculation requires having the necessary data.

For any of the proposed new trees, we need to calculate the new tree key for:

- An account: we require the account’s address.
- A storage slot: we require the account’s address and storage slot number.
- A [code-chunk](./../code-chunking/intro.md): we require the contract's address and its code.

When we do the [EIP-7748 walking](./eip-7748.md#big-picture), clients only have access to the MPT key, which results from hashing addresses or storage slots. Most clients only have these hashed values, so they don’t have enough information to do the listed new tree key calculations. They must have a *preimages file* to map each MPT key hash to the preimage.

## Preimages file

Now we dive into different dimensions of this preimage file:

- Distribution
- Verifiability
- Generation and encoding
- Usage

### Distribution

Given that the preimage file is somehow generated, how does this file reach all nodes in the network? The consensus is that it’s probably OK to expect clients to download this file through multiple CDNs. This is compared to relying on alternatives like the Portal network, in-protocol distribution, or including block preimages.

Other discussed options are:

- Having an in-protocol p2p distribution mechanism.
- Distributing the required preimages packed inside each block.

This topic is highly contentious since these options have different tradeoffs regarding complexity, required bandwidth in protocol hotpaths, and compression opportunities. If you’re interested there’s an [older document](https://hackmd.io/@jsign/vkt-preimage-generation-and-distribution) summarizing many discussions around the topic.

### Verifiability

As mentioned above, full nodes will receive this file from somewhere that can be a potentially untrusted party or a hacked supply chain. If the file is corrupt or invalid, the full node will be blocked at some point in the conversion process.

The file is easily verifiable by doing the described tree walk, reading the expected preimage, calculating the keccak hash, and verifying that it matches the client's expectations. After this file is verified, it can be safely used whenever the conversion starts, with the guarantee that the client can’t be blocked by resolving preimages — having this guarantee is critical for the stability of the network during the conversion period. This verification time must be accounted for in the time delay between EIP-7612 activation and EIP-7748 *CONVERSION_START_TIMESTAMP* timestamp*.*

Of course, other ways to verify this file are faster but require more assumptions. For example, since anyone generating the file would get the same output, client teams could generate themselves and hardcode the file's hash/checksum. When the file is downloaded/imported, the verification can compare the hash of the file with the hardcoded one.

### Generation and encoding

Now that we know which information the file must contain and in which order this information will be accessed, we can consider how to encode this data in a file. Ideally, we’d like an encoding that satisfies the following properties:

- Optimize for the expected usage reading pattern: the state conversion is a task running in the background while the main chain runs, so reading the required information should be efficient.
- Optimize for size: as mentioned before, the file has to be distributed somehow. Bandwidth is a precious resource; using less is better.
- Low complexity: this file will only be used once, so a simple encoding format is good. It doesn’t make sense to reinvent the wheel by creating new complex formats unless they offer exceptional benefits while taking longer to spec out and test.

There’s a very simple and obvious candidate encoding that can be described as follows following the example we explored before: `[address_A][storage_slot_A][storage_slot_B][address_B][address_C][storage_slot_A]...`. We directly concatenate the raw preimages next to each other in the expected walking order.

This encoding has the following benefits:

- The encoding format has zero overhead since no prefix bytes are required. Although preimage entries have different sizes (20 bytes for addresses and 32 bytes for storage slots), the EL client can know how many bytes to read next depending on whether they should resolve an address or storage slot in the tree walk.
- The EL client always does a forward-linear read of the file, so there are no random accesses. The upcoming *Usage* section will expand on this.

If you are interested in knowing how this file can be generated and some analysis about the efficiency of the format, see [this research post](https://ethresear.ch/t/state-tree-preimages-file-generation/21651#p-52669-diving-deeper-into-encoding-efficiency-11).

## Usage

It is worth mentioning some facts about how EL clients can use this file:

- Since the file is read linearly, persisting a cursor indicating where to continue reading from at the start of the next block is useful.
- Keeping a list of cursor positions for the last X blocks helps handle reorgs. If a reorg occurs, it’s very easy to seek into the file to the corresponding place again.
- Clients can also preload the next X blocks preimages in memory while the client is mostly idle in slots, avoiding extra IO in the block hot path execution.
- If keeping the whole file on disk is too annoying, you can delete old values past the chain finalization point. We doubt this is worth the extra complexity, but it’s an implementation detail up to EL client teams.
