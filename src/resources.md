# Resources

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

### Advanced technical write-ups

- [Inner Product Argument - Dankrad Feist](https://dankradfeist.de/ethereum/2021/07/27/inner-product-arguments.html)
- [PCS Multiproof - Dankrad Feist](https://dankradfeist.de/ethereum/2021/06/18/pcs-multiproofs.html)
- [Deep dive into Circle-STARKs FFT - Ignacio Hagopian](https://ihagopian.com/posts/deep-dive-into-circle-starks-fft)

### EIPs

- [EIP-6800](https://eips.ethereum.org/EIPS/eip-6800), structure of a Verkle Tree
- [EIP-4762](https://eips.ethereum.org/EIPS/eip-4762), gas costs changes for Verkle Trees
- [EIP-2935](https://eips.ethereum.org/EIPS/eip-2935) and [EIP-7709](https://eips.ethereum.org/EIPS/eip-7709), save historical block hashes in the state
- [EIP-7612](https://eips.ethereum.org/EIPS/eip-7612) and [EIP-7748](https://eips.ethereum.org/EIPS/eip-7748), about the state tree conversion
- [EIP-7864](https://eips.ethereum.org/EIPS/eip-7864), about the proposed scheme for Binary Trees

### Measurements & Metrics

- [Verkle Metrics](https://verkle.info/verkle-measurements)
- [State Tree Preimages Generation](https://ethresear.ch/t/state-tree-preimages-file-generation/21651)
