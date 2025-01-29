# The Stateless Ethereum Book

- [What is Stateless Ethereum?](#what-is-stateless-ethereum)
- [Why is stateless important?](#why-is-stateless-important)
- [Main benefits](#main-benefits)
- [Purpose of this book](#purpose-of-this-book)

## What is Stateless Ethereum?

Stateless Ethereum is an update to the Ethereum protocol, in which blocks become self-contained units of execution. There is no longer a need to download the entire state of the Ethereum, all the required information is packaged inside the block. 

## Why is stateless important?

Stateless Ethereum brings forth a lot of the scalability and usability features that the Ethereum community has been waiting for for a long time.

In pratical terms, this means:

 * reduced validator hardware requirements: IO, disk space, and computation.
 * as a result, a higher gas limit, since the lower gas limit was imposed by these hardware requirements.
 * faster sync times, as a node doesn't need more than an EL block to join the network.
 * an easy implementation of state expiry, a feature that has eluded Ethereum since before 2018.
 * trustless light clients, that directly follow the chain without needing a third party to provide the state.
 * better decentralization, as it makes it possible to cheaply create private pools.

## Main benefits

Statelessness in Ethereum brings significant benefits by addressing critical scalability and decentralization challenges. It brings many benefits in terms of scalability, decentralization, features and ease of use.

### Scalability

By removing the need for clients to store a great amount of data, validators can process more transactions per block, increasing the throughput. Thus, we enable:

 * **higher TPS**, since the IO bottleneck is the principal hindrance to increasing the gas limit.
 * **no required synchronization**, since all the data needed to execute a block is packaged with it.
 * **reduced disk footprint**, for non-block builders validators, wallets and simple nodes. In the case of verkle trees, block builders will also benefit from a reduced disk footprint.

### Decentralization

By reducing the hardware requirements for validating nodes makes it feasible for more participants to run nodes on lightweight devices. This fosters decentralization by:

 * **lowering entry barriers**, where new roles with reduced hardware and monetary investments are created, for new actors to help secure some aspects of the network (see rainbow staking).
 * **enabling users to create private staking pools**, where hardware and monetary resources can be pooled collaboratively to participate in the network.
 * **reducing the trust placed in centralized data providers**, removing the need for lightweight clients to trust a centralized entity.

### Innovative features

Statelessness also opens the door to innovative features, including:

 * **state expiry**, which limits the growth of historical state data.
 * **rainbow staking**, which enhances flexibility in staking mechanisms by creating many niches for low-stake nodes to participate in the network’s security.
 * **secure light clients**, which is the consequence of not having to trust a centralized authority when using the blockchain.

### Ease of use

Additionally, by reducing the state proof size, statelessness facilitates more seamless cross-chain communication, laying the groundwork for improved interoperability between the Ethereum L1 and its L2s.

## Purpose of this book

This book is designed to serve as a comprehensive resource for understanding and contributing to our work on stateless Ethereum.

### Goals of This Book

 * **Explain the Vision**: Provide an in-depth explanation of the motivation behind stateless Ethereum, including its potential impact on scalability and decentralization.
 * **Technical Guidance**: Offer clear and detailed instructions for developers, researchers, and contributors to engage with and extend our work.
 * **Knowledge Sharing**: Educate readers about various aspects of stateless block execution, and their role in achieving stateless Ethereum.
 * **Encourage Collaboration**: Foster a community of like-minded individuals by providing resources, tools, and best practices for collaborative development.

### Who is this book for?

This book is intended for:

 * **Developers**: Interested in contributing to the implementation of stateless Ethereum.
 * **Researchers**: Exploring the new designs enabled by stateless Ethereum, and learning how client architecture is impacted by these choices.
 * **Learners**: Seeking to deepen their understanding of this major evolution of the Ethereum protocol.
