# Gas cost remodeling

- [Gas cost remodeling](#gas-cost-remodeling)
  - [Recommended reading](#recommended-reading)
  - [How are costs related to stateless Ethereum?](#how-are-costs-related-to-stateless-ethereum)
  - [Which are the main drivers for gas cost changes?](#which-are-the-main-drivers-for-gas-cost-changes)

## Recommended reading

It’s highly recommended that you read the [Trees chapter](../trees/intro.md) and [Data encoding page](../trees/data-encoding.md) since those changes are fundamental to the need for a gas model reform.

## How are costs related to stateless Ethereum?

As discussed in other chapters, multiple protocol changes are needed to achieve Ethereum statelessness. Some of these changes must impact gas costs since they are profound changes that should reflect new CPU or bandwidth costs or mitigate new potential attack vectors.

Changing gas costs is always a complex problem since the gas cost model is part of the blockchain's public API. Although the protocol can’t promise fixed gas costs forever, it’s usually something that we must think carefully about since it can break existing contracts that have baked-in assumptions. Not all gas cost changes are negative, but the overall impact (positive or negative) depends on each case.

## Which are the main drivers for gas cost changes?

The following are reasons that motivate doing a gas cost remodeling:

- The block will contain an *execution witness* that includes new information—at a minimum, it will contain proof. The block size is affected by EVM instruction, which depends on the state.
- The new trees have a new grouping strategy for storage slots, which attempts to lower the gas cost access.
- Code is also included in the tree, so gas must be charged to access it when contracts are executed.

The only EIP proposed to address these changes is [EIP-4762](eip-4762.md), which is currently focused on the Verkle Trees as the new tree. For Binary Trees, the *execution witness* is not entirely clear today. The book will contain an explanation whenever the roadmap is clearer on this front, so stay tuned!