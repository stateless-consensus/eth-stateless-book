# The Stateless Ethereum Book

- [The Stateless Ethereum Book](#the-stateless-ethereum-book)
  - [What is Stateless Ethereum?](#what-is-stateless-ethereum)
  - [Why is statelessness important?](#why-is-statelessness-important)
  - [Main benefits](#main-benefits)
    - [Scalability](#scalability)
    - [Decentralization](#decentralization)
    - [Innovative features](#innovative-features)
    - [Ease of use](#ease-of-use)
  - [Purpose of this book](#purpose-of-this-book)
    - [Goals of This Book](#goals-of-this-book)
    - [Who is this book for?](#who-is-this-book-for)

## What is Stateless Ethereum?

Stateless Ethereum is an update to the Ethereum protocol, in which blocks become self-contained units of execution. There is no longer a need to download the entire state of Ethereum, as all the required information is packaged inside the block.

## Why is statelessness important?

Stateless Ethereum brings forth many scalability and usability features that the Ethereum community has been anticipating for a long time.

In practical terms, this means:

- Reduced validator hardware requirements: IO, disk space, and computation.
- As a result, a higher gas limit, since the lower gas limit was imposed by these hardware requirements.
- Faster sync times, as a node doesn't need more than an EL block to join the network.
- Easy implementation of state expiry, a feature that has eluded Ethereum since before 2018.
- Trustless light clients that directly follow the chain without needing a third party to provide the state.
- Better decentralization, as it makes it possible to cheaply create private pools.

## Main benefits

Statelessness in Ethereum brings significant benefits by addressing critical scalability and decentralization challenges.

### Scalability

By removing the need for clients to store a large amount of data, validators can process more transactions per block, increasing throughput. Thus, we enable:

- **Higher TPS**, since the IO bottleneck is the principal hindrance to increasing the gas limit.
- **No required synchronization**, since all the data needed to execute a block is packaged with it.
- **Reduced disk footprint**, for non-block builders, validators, wallets, and simple nodes. In the case of verkle trees, block builders will also benefit from a reduced disk footprint.

### Decentralization

By reducing the hardware requirements for validating nodes, it becomes feasible for more participants to run nodes on lightweight devices. This fosters decentralization by:

- **Lowering entry barriers**, where new roles with reduced hardware and monetary investments are created, allowing new actors to help secure some aspects of the network (see rainbow staking).
- **Enabling users to create private staking pools**, where hardware and monetary resources can be pooled collaboratively to participate in the network.
- **Reducing the trust placed in centralized data providers**, removing the need for lightweight clients to trust a centralized entity.

### Innovative features

Statelessness also opens the door to innovative features, including:

- **State expiry**, which limits the size of the active state.
- [**Rainbow staking**](https://ethresear.ch/t/unbundling-staking-towards-rainbow-staking/18683), which enhances flexibility in staking mechanisms by creating many niches for low-stake nodes to participate in the network’s security.
- **Secure light clients**, which is the consequence of not having to trust a centralized authority when using the blockchain.

### Ease of use

Additionally, by reducing the state proof size, statelessness facilitates more seamless cross-chain communication, laying the groundwork for improved interoperability between the Ethereum L1 and its L2s.

## Purpose of this book

This book is designed to serve as a comprehensive resource for understanding and contributing to our work on Stateless Ethereum.

### Goals of This Book

- **Explain the vision**: Provide an in-depth explanation of the motivation behind Stateless Ethereum, including its potential impact on scalability and decentralization.
- **Technical guidance**: Offer clear and detailed instructions for developers, researchers, and contributors to engage with and extend our work.
- **Knowledge sharing**: Educate readers about various aspects of stateless block execution and their role in achieving Stateless Ethereum.
- **Encourage collaboration**: Foster a community of like-minded individuals by providing resources, tools, and best practices for collaborative development.

### Who is this book for?

This book is intended for:

- **Developers**: Interested in contributing to the implementation of Stateless Ethereum.
- **Researchers**: Exploring the new designs enabled by Stateless Ethereum and learning how client architecture is impacted by these choices.
- **Learners**: Seeking to deepen their understanding of this major evolution of the Ethereum protocol.
