# 32-byte code-chunker

- [32-byte code-chunker](#32-byte-code-chunker)
  - [Background reading](#background-reading)
  - [How does it work?](#how-does-it-work)
  - [Runtime usage](#runtime-usage)
  - [Efficiency](#efficiency)
  - [Implementations](#implementations)

## Background reading

To get a proper background on where this code chunker fits into stateless Ethereum, read the [*Trees*](intro.md) introductory chapter and the [*Code chunking*](data-encoding.md#code-chunking) section of *Data encoding*.

## How does it work?

This approach aims to store code in full 32-byte chunks while efficiently encoding the necessary metadata to validate jump destinations. [This proposal](https://github.com/ipsilon/eof/blob/eof0-dense/spec/eofv0_verkle.md#encode-only-invalid-jumpdests-dense-encoding) contrasts with the [31-byte chunker](31-byte-code-chunker.md) by storing metadata separately rather than prepending it to each chunk.

The goals remain similar:

- Output the code itself as a list of 32-byte blobs (chunks), which will be stored as tree leaves.
- Provide a mechanism, using separate metadata, for an EVM interpreter to detect if a `JUMP(I)` to any byte offset is valid (i.e., targets a `JUMPDEST` opcode and not PUSHDATA).

Instead of adding metadata to every chunk, the "Dense Encoding" variant method focuses only on potential ambiguities:

- The account bytecode is partitioned into full 32-byte chunks.
- We identify only the chunks that contain invalid jump destinations. An invalid jump destination is a `0x5b` byte that occurs within the data part of a PUSH instruction.
- A map `invalid_jumpdests[chunk_index] = first_instruction_offset` is created. first_instruction_offset indicates the offset within the chunk where the first actual instruction begins.
- This map is then encoded very efficiently using a Variable Length Quantity (VLQ) scheme (specifically, [LEB128](https://en.wikipedia.org/wiki/LEB128)) applied to a combined value representing the distance between invalid chunks and the first_instruction_offset.
- This densely encoded metadata is stored probably prepended as a custom table for legacy contracts, i.e., before the actual originalbytecodes. EOF contracts can't have invalid jumps since this is validated at deployment time.

## Runtime usage

When a `JUMP(I)` occurs:

- The EVM checks if the target byte offset contains the `JUMPDEST` (`0x5b`) opcode.
- If the chunk index is not in the `invalid_jumpdests` map, the jump is valid (assuming step 1 passed). If it is present in the map, the EVM must perform a quick analysis of that specific chunk, using the associated `first_instruction_offset` from the map, to parse the instructions within the chunk and confirm whether the target 0x5b is indeed a `JUMPDEST` opcode and not part of PUSHDATA.

## Efficiency

This method leverages the observation that invalid jumpdests are rare in typical contracts. The code itself is stored in full 32-byte chunks (`ceil(N/32)` chunks for code length N). The metadata overhead is very low on average (~0.1%) and has a worst-case overhead of 3.1% (if every chunk contained an invalid `JUMPDEST`). For example, for contract size limits of 24KiB, 64KiB and 256KiB the maximum table overhead would require 24, 64 and 256 code-chunks respectively, assuming a best case table encoding of 1 byte per entry. Note that after 128KiB the table won't fit into the 128 code-chunks reserved in the account header.

## Implementations

For a Python implementation, you can refer to [the spec](https://github.com/ipsilon/eof/blob/eof0-dense/spec/eofv0_verkle.md#reference-encoding-implementation). There is also a [Go implementation](https://github.com/jsign/chunking-analysis/blob/f819b28c7efaee1d0b630a722bbac494ddf0cd1d/analysis/z32bytechunker/z32bytechunker.go#L107-L190).
