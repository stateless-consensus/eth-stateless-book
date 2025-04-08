# Tree shape

- [Tree shape](#tree-shape)
  - [Accounts and code-length stats](#accounts-and-code-length-stats)
  - [Stems type counts](#stems-type-counts)
  - [Stems filling stats](#stems-filling-stats)
  - [Final words](#final-words)

The following tables are the result of a static analysis of current mainnet data to understand better what the shape of the new proposed trees would look like. This data can be reproduced by using the `analysis` tool in the [eth-stateless](https://github.com/jsign/eth-stateless) repository. All data presented here corresponds to mainnet data at block number 22181932 (~2025-04-02).

## Accounts and code-length stats

```text

+-----------+-----------+-----------+
| Accounts                          |
+-----------+-----------+-----------+
| eoas      | contracts | total     |
+-----------+-----------+-----------+
| 239157739 | 46990900  | 286148639 |
+-----------+-----------+-----------+

+-------------+---------+--------+-------+-------+
| Code length                                    |
+-------------+---------+--------+-------+-------+
| sum         | average | median | p99   | max   |
+-------------+---------+--------+-------+-------+
| 32303445535 | 687     | 45     | 11293 | 24576 |
+-------------+---------+--------+-------+-------+

```

This data provides a baseline for understanding account and contract code-length distribution.

## Stems type counts

As explained in the [Data-encoding - Grouping](https://www.notion.so/trees/data-encoding.md#grouping), the new tree has different kinds of stems:

- Account headers stem: encode accounts’ basic data and optimizes top storage slots and code chunks.
- Storage slots stem: grouping of 256 contiguous storage slots for an account.
- Code chunks stem: grouping of 256 contiguous code chunks for an account.

It is helpful to understand the distribution of these kinds of stems in the current mainnet data:

```text
+-----------------------+-----------+--------+
| Stems type counts                          |
+-----------------------+-----------+--------+
| name                  | total     | %      |
+-----------------------+-----------+--------+
| Accounts header stems | 286148639 | 23.31% |
+-----------------------+-----------+--------+
| Storage-slots stems   | 939462320 | 76.51% |
+-----------------------+-----------+--------+
| Code-chunks stems     | 2209323   | 0.18%  |
+-----------------------+-----------+--------+
| Total = 1227820282                         |
+-----------------------+-----------+--------+

```

A total of 1.23 billion stems already provide helpful information to predict the depth of the new potential trees:

- A [Verkle Tree](https://www.notion.so/trees/vkt-tree.md) would have a depth of ~4 (i.e., log_256(1.23b))
- A [Binary Tree](https://www.notion.so/trees/binary-tree.md) would have a depth of ~31 (i.e., log_2(1.23b))

EL clients can employ engineering optimizations to deal with the high depth of Binary Trees, which can be explored more in-depth in this [external article](https://hackmd.io/@jsign/binary-tree-notes#Tree-serialization). In any case, knowing this depth helps predict special cases of protocol proofs.

For example, for [FOCIL](https://eips.ethereum.org/EIPS/eip-7805) or stateless mempools, assuming the depths predicted above, proving a single account nonce/balance in both cases then means:

- Verkle Tree: 4*32+[C1/C2]=160 bytes from required commitments plus the IPA/Multiproof (28+1)*32+32=576 bytes. So, a total proof size of ~736 bytes.
- Binary Tree:
  - For contracts: (31+8)*32=1.22KiB.
  - For EOAs: (31+1)*32~=1024 bytes for EOAs.
  - The difference is because the eight levels in the group can be compressed better in the EOA case since there are known empty hashes in siblings.

The example above is only relevant for the described cases (i.e., stateless mempool or FOCIL). For mainnet blocks, each proof type will shine differently in different dimensions (i.e., proof size, time to compute, verification time, etc). In particular, for mainnet blocks, a Binary Tree proof would be SNARKified compared to the Merkle Tree proof, which makes sense in the mentioned cases. Also note that in some inclusion list scenarios, it can make sense to leverage aggregating more than one account opening in the same proof.

## Stems filling stats

An interesting question is to understand how “filled” these 256-groups are. Although EL clients can efficiently serialize these groups with different strategies, it is still worth understanding this dimension to build the right mental model of how a mainnet tree would look.

```text
+-----------------------+---------+--------+-----+-----+
| Stems non-zero values count distribution             |
+-----------------------+---------+--------+-----+-----+
| name                  | average | median | p99 | max |
+-----------------------+---------+--------+-----+-----+
| Accounts header stems | 4       | 2      | 74  | 194 |
+-----------------------+---------+--------+-----+-----+
| Storage slots stems   | 1       | 1      | 5   | 256 |
+-----------------------+---------+--------+-----+-----+

+-------------------------------------------------+
| Storage-slots stems with single non-zero values |
+-------------------------------------------------+
| 868061562                                       |
+-------------------------------------------------+

```

The first table shows stats on how filled stem groups are for different kinds of stems. For example, account header stems always have two non-zero value items (BASIC_DATA and CODE_HASH), but optionally, they can have more items in the case of contracts (64 storage slots and 128 code-chunks).

The *Storage-slot stems with single non-zero values* table zooms in on how many storage-slot stems only have one non-zero value:

- ~92% of storage-slot stems (868m/939m), and ~71% of all stems (868m/1.23b)
- These items reflect Solidity hashmap usages since those entries are spread in all the storage slot address space, generating stems with only one non-zero value.
- The ratio of 71% suggests that EL implementations must heavily optimize the efficient serialization of these stems.
- For [leaf-level state expiry](https://eips.ethereum.org/EIPS/eip-7736), expired data in these stems wouldn’t reclaim much space since we’re only deleting 32 bytes (single non-zero value) but keep the corresponding stems, which aren’t collapsed — this is more significant in a Binary Tree scenario than a Verkle Tree one because of the arity. However, this needs to be investigated in detail since it depends on how many of these stems expire in an average expiry epoch.

## Final words

The provided static analysis helps understand the shape of the new tree in the mainnet. But there’s another dimension, which is how the grouping allows more efficient state access, but that will be explored soon by doing a dynamic analysis of mainnet state access patterns.
