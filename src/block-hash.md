# BLOCKHASH state

- [BLOCKHASH state](#blockhash-state)
  - [Motivation](#motivation)
  - [Setting up the stage…](#setting-up-the-stage)
  - [Fixing the last missing piece…](#fixing-the-last-missing-piece)
  - [Backward compatibility](#backward-compatibility)

## Motivation

In a stateless Ethereum world, we require that a block provides only the data needed for its execution, compared to today’s reality of forcing the clients to store the whole state. This applies not only to plain re-execution but also to SNARK proofs.

Usually, all the required state for executing EVM instructions rely on data stored in the state tree. For example, `CALL`, `BALANCE`, `SLOAD`, and `EXTCODEHASH` always involve reading the state to resolve their execution. Since this data lives in a merkelized tree, we can generate state proofs to provide this data trustlessly.

There’s a particular exception for `BLOCKHASH`. Before the [Pectra update](https://pectra.wtf/) (planned for Q2 2025), executing `BLOCKHASH` relied on information not stored in the tree. This meant that execution clients must store the last 256 block hashes in a separate storage. This isn’t a big ask since full nodes store the whole blockchain anyway, so accessing this information was easy. While the blockchain is a merkelized structure, and you could provide proof for the block hashes, this would require up to 256 block headers for the evidence, which is prohibitive.

## Setting up the stage…

In Pectra, the protocol took an important step towards fixing this problem by delivering [EIP-2935](https://eips.ethereum.org/EIPS/eip-2935). This EIP adds a new rule to the protocol where each new block includes the previous block hash in a system contract.

This means that `BLOCKHASH` can now be resolved from the state tree, as all other EVM instructions do. EIP-2935 doesn’t force `BLOCKHASH` to use the state tree, so clients shouldn’t change how they execute `BLOCKHASH`.

## Fixing the last missing piece…

The last required change is finally requiring `BLOCKHASH` to be resolved from the state tree rather than from the blockchain. This is where [EIP-7709](https://eips.ethereum.org/EIPS/eip-7709) comes into play.

The change is quite simple. `BLOCKHASH` now involves a state tree access, which can be done by reading the corresponding storage slot from the EIP-2935 contract. This implies that the gas cost for the instruction should now map to an equivalent `SLOAD`.

## Backward compatibility

Despite EIP-2935 storing 8191 historical block hashes, the `BLOCKHASH` instruction only serves the last 256 values. This is required to avoid a breaking change in the instruction semantics (i.e., current `BLOCKHASH` executions asking for older values must still return `0`).
