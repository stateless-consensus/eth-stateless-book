<!-- markdownlint-disable MD024 -->

# SIC calls history

## [**Call #28: December 2, 2024**](https://github.com/ethereum/pm/issues/1203)

[Recording here](https://www.youtube.com/watch?v=5bxvSLvc9LA)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): started working on EIP draft for binary tries. Also working on new execution spec tests from bugs that were found in latest devnet.

[@g11tech](https://x.com/g11tech) for [@EFJavaScript](https://twitter.com/EFJavaScript): been playing with devnet-7, and resolving some recently found issues.

[@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): Working on implementing the blockhash gas cost modification. Also implementing the proof verification. This will unblock some other stuff like Verkle sync (snap sync equivalent), and validating proof coming from the other client. Also working on optimizing stem generation by modifying how we call the Rust Verkle library.

### **2. Execution spec tests**

Update from Ignacio, sharing [v0.0.8](https://github.com/ethereum/execution-spec-tests/releases/tag/verkle%40v0.0.8).

After we previously launched devnet-7, we found a consensus bug with Nethermind. Debugged with Tanishq and found an edge case. Created a new test case to cover this. Idea going forward is that every bug we find in any devnet should result in a new execution spec test to cover it. Any new client that joins the next devnet will also be covered by these cases.

### **3. Preimage distribution and Portal Network**

[Piper](https://twitter.com/pipermerriam) from Portal Network joined to share some thoughts on preimage distribution with Portal.

Regarding preimage distribution for the Verkle migration: Piper mentioned that Portal can solve this, but probably isn’t the best solution. Portal is best for clients who want to grab a subset of the data on demand. With the preimage problem for Verkle, all of the clients would need to grab the full set of preimages. But there could be a “file-based approach” that could make sense in this case.

The file-based approach: S3 buckets being able to generate the file, and potentially having pre coded S3 buckets in clients that are already distributed. Has a predistributed trust model. Distributing big files like this from S3 buckets is a pretty straightforward approach. Alternative to S3 buckets, could use torrents.

Guillaume mentioned that Lukasz from Nethermind had previously indicated a preference for using an in-protocol p2p approach. But added that it seems clear that the CDN approach is the simplest and should probably go with that.

Next steps: make a spec on a potential format for the file.

### **4. State expiry**

Hadrien from OpenZeppelin joined to share some thoughts on state expiry.

One related question that was discussed in Devcon: do we want to resurrect based on reads or based on writes?

Hadrien: most of the storage accesses are as you expect related to reading and writing. And so one question is whether a read would extend the lifetime of an extension or not. There are a few slots that are being read, but never written to. For example in the case of ERC-721, when tokens are transferred the slot will often contain a zero because nobody is allowed to take the token. And anytime there is a transfer of that token, in the current implementation it will write another zero to reset, even if old value is a zero, because its cheaper than trying to read. Depending on how state expiry works, there may be different cost model depending on whether there is a zero because it was never written to the state, compared to if a zero gets explicitly written.

This is where the approach of state expiry shown in EIP-7736 comes in. Han joined to share some updates on this front.

Han is currently working on implementing 7736, and the changes needed on the Verkle part are done. Still working on the geth part. Once we get all the components complete and integrated can hopefully have a devnet to start testing out the various scenarios.

### **5. Stateless Transactions EIP**

Gajinder shared some of the latest thinking around changes needed to best support stateless clients.

One of the challenges is that stateless clients which don’t maintain any execution state would not be able to do any kind of local block building. This proposal is a partial remedy to this problem.

The basic idea is that with every transaction the transaction submitter will also have to submit an execution witness, and that execution witness will have a state diff + proof of the state diff. The execution witness would also bundle the parent state root, which is what a builder would look at to see whether it can include this tx in the particular block that it’s building.

Gajinder is currently drafting the EIP. Will have something to share soon.


## [**Call #27: November 4, 2024**](https://github.com/ethereum/pm/issues/1196)

[Recording here](https://www.youtube.com/watch?v=1PYu4_Ac1Po)

[no notes this week]

### [**Call #26: October 21, 2024**](https://github.com/ethereum/pm/issues/1186)

[Recording here](https://youtu.be/MJA1e95cfww)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): added around 30 new tests in the execution test spec repo, mostly covering edge cases regarding out of gas execution transactions. (This is relevant for Verkle because if you run out of gas, you might generate a partial witness). Guilluame has been working on speeding up witness generation, as well as the rebase on top of Cancun to get updated metrics.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): testing and debugging latest hive tests. Also have been progressing on the Nethermind implementation of the transition.

[@lu-pinto](https://github.com/lu-pinto) and [@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): Working on gas costs. Currently 2 tests are failing (regarding self-destruct). Also all of the `BLOCKHASH` tests are failing because the logic for pulling out the `BLOCKHASH` from the system contract is not yet implemented, but that’s up next. Have also completed some optimizations on the rust-verkle crypto library. And now have a working version of the flatDB based on stem. Next step here is to generate the preimage to be able to run this flatDB with mainnet blocks and compare performance. Lastly, continuing to work on integrating the [Constantine](https://github.com/mratsim/constantine) crypto library into Besu.

### **2. Circle STARKs seminar**

[Matan](https://sites.google.com/view/matanprasmashomepage) joined to share some info on an upcoming [SNARK-focused seminar](https://forms.gle/n3BtvJXLGrfbUPkV8) he will be leading, which will be open for anyone currently working on stateless. The seminar will be focused on bridging the gaps for upcoming items on the Verge roadmap, in particular exploring the topics of SNARKing Verkle, Circle STARKs, and STARKed binary hash trees (as a potential alternative to Verkle). No prerequisites or math background required for the course. Idea is accessible to lay audience. The seminar will likely be weekly and run for TBD number of weeks.

[Apply to join here!](https://forms.gle/n3BtvJXLGrfbUPkV8)

### **3. Verkle Metrics**

Guillaume shared an [updated document](https://www.notion.so/Verkle-measurements-123d9895554180e6ac17eddf76c692b6?pvs=21) which provides a helpful overview of latest Verkle metrics. This is data collected by replaying ~200k historical blocks (around the time of the Shanghai fork). While they don’t provide a perfect predicition for how things will look in the future, it does help give a solid approximation of what to expect.

Few highlights below, and check out the doc for the full report.

## **Witness size**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_74d2e508ca3499afb6064b4af670e4cf.png)

## **Database**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_71000583740af6e688511c8562c48227.png)

## **Tree key hashing: comparing Pedersen vs sha256**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_ce9bc497d137bad837dbf5f8e1661569.png)

### **4. Spec Updates**

Last up on this week’s call, a few quick points related to gas cost spec:

- `CREATE` gas cost: currently 32,000. But for Verkle, we’ve considered reducing this.
  - Decision: keep as-is for now.
- How much to charge when doing account creation (e.g. when doing a call and the address does not exist yet). And similar question for `SELFDESTRUCT`
  - Decision: open a convo with research team for further discussion

## [**Call #25: September 23, 2024**](https://github.com/ethereum/pm/issues/1149)

[Recording here](https://youtu.be/pfORU9ngjzI)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): just about ready for testnet relaunch. Most PRs merged. Also working on benchmarking. Some updates to the test fields, and fixed some tests around witness checks. Can also run the tests in stateless mode soon, where the witness acts as the prestate source of truth.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): no major updates. Working with Ignacio on tests. Will try to run the tests in stateless mode this week 🔥

[@kt2am1990](https://twitter.com/kt2am1990) and [@lu-pinto](https://github.com/lu-pinto) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): last week, working on the flat DB based on stem key. Completed some benchmarking to see impact on SLOAD. Found the perf was quite bad. Working with Kev on some optimizations. Also working on integrating [Constantine](https://github.com/mratsim/constantine) library into Besu, in order to compare perf. Made some good progress on getting the test fixtures to run. Will get back to finalziing gas costs after.

### **2. EIP-7702 in Verkle**

We quickly went through a [PR from Guillaume](https://github.com/ethereum/EIPs/pull/8896) to update the stateless gas costs EIP in order to support changes coming in [7702](https://eips.ethereum.org/EIPS/eip-7702). (Current plan is for 7702 to be included in Pectra, & 7702 containts a new type of tx: `authorization_list`. Info that is used to update some accounts)

TLDR is this updates the gas costs EIP so if you call these functions (`EXTCODESIZE`, `EXTCODEHASH` etc), then you also need to touch the `CODEHASH_LEAF_KEY`. Ping Guillaume with any comments/questions.

### **3. Partial Witness Charging**

Ignacio walked through a few scenarios where a bit more granularity may be needed around gas costs in Verkle. [See PR here.](https://github.com/gballet/go-ethereum/pull/495)

Scenario 1: if you don’t have enough gas to pay for whatever witness charging you have to do, then you don’t actually need to include that in the witness. (e.g. if you do a jump and don’t have enough available to pay, then you wouldn’t include that cold chunk in the witness)

Scenario 2: there are several places in execution where you have to include more than one leaf in the witness. In these cases, Geth was previously always including both leaves. Even if you didn’t have enough gas, it was already added to the witness.

For both scenarios we want to allow for better granularity / partial witness charging.

TLDR: only add things to the witness if you have the available gas for it

### **4. [Testnet-7 Check-in](https://kaustinen-testnet.ethpandaops.io/)**

Ending things with a quick check on testnet readiness. Pari said DevOps should have time to assist later this week. Recommended doing it locally first in case bugs are found, and then once it works locally switch to a public testnet 🥳

## [**Call #24: September 9, 2024**](https://github.com/ethereum/pm/issues/1149)

[Recording here](https://youtu.be/TTqikpo4R7g)

### **1. Team updates**

Gary for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): continuing work on stem-based flat database, and have a working implementation. Will simplify sync for Besu. Also making progress on gas cost changes and witness updates (EIP-4762). And planning on getting back to work on the transition stuff soon.

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): last week, discussed sync with the Geth team, and would like to figure out how performant it is to compute all of the leaves as they are needed (reading SLOAD + building a snapshot based on those leaves). Interested to understand Besu’s performance in this regard. Also completed a bunch of work on the testing framework. Gas costs are ready to go. Should be ready for new testnet in next few days.

[@GabRocheleau](https://twitter.com/GabRocheleau) for [@EFJavaScript](https://twitter.com/EFJavaScript): continued work to prepare for the upcoming testnet. Running the actual tests this week. Also made some upgrades to the [WASM cryptography library](https://github.com/ethereumjs/verkle-cryptography-wasm) so can begin creating proofs, and allows for stateful verkle state manager. Previously could only run blocks statelessly.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): mostly been working on getting the Hive test running. Found and fixed a few bugs. One change in the spec around gas costs, and continuing work on the transition. Working on some cryptography improvements as well.

Somnath for [@ErigonEth](https://twitter.com/erigoneth): integrating the gas cost changes and the witness calculation changes. Noticed some issues with the state management, but hopeful can resolve it in next week. Erigon has already started migrating everything to [Erigon 3](https://erigon.tech/the-rise-of-erigon3-and-our-new-release-cycle/), but current verkle work is based on Erigon 2. Will try to join devnet-7 soon after it launches.

[@g11tech](https://x.com/g11tech) for [@lodestar_eth](https://x.com/lodestar_eth): rebasing on latest from lodestar, which should provide a more stable base for testnet because of improved performance.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: we have the witness assertions now within the framework when filling tests. Can write a test, and assert that a balance is what you expect. More of a sanity check. Now have similar for witness-specific values. Next step is to optimize how the tests are filled, and improve the transition tool runtime.

### **2. Devnet Readiness**

There’s a couple updates to the specs that are still open. Quickly reviewed with Guillaume 3 PRs to merge in asap for the testnet:

https://github.com/ethereum/EIPs/pull/8867

https://github.com/ethereum/EIPs/pull/8707

https://github.com/ethereum/EIPs/pull/8697

^ Leaving open for comments, but no objections raised during the call on these PRs.

### **3. Verkle, Binary, & Tree-agnostic development**

![image](https://storage.googleapis.com/ethereum-hackmd/upload_12245bd6275db8c90b1255d273af807a.png)

Quick recap of recent conversations we’ve had around the tradeoffs of Binary vs. Verkle:

This is a topic that has come up often over the past few years, and even going all the way back to 2019/2020 – when [@gballet](https://x.com/gballet/status/1278259913454690304) was an author of [EIP-3102](https://eips.ethereum.org/EIPS/eip-3102): an initial proposal to migrate to a binary trie structure.

More recently, as many teams have made strong progress on the ZK proving side, there’s been renewed discussion around whether a ZK-based solution could be ready in a similar timeframe to Verkle (or soon thereafter), and allow us to skip straight to a fully SNARKed L1.

In this scenario, a binary trie structure is arguably preferable, in that it’s a friendlier option to current ZK proving systems, despite Verkle’s advantages in other dimensions (such as smaller tree/proof size and slower state growth). While Verkle is closer to “mainnet ready” today, it’s possible the gap closes over the next 1-2 years.

The challenge and discussion now is mostly centered around how we can optimize forward progress on R&D efforts, solve problems facing users today, while also making sure we properly evaluate other viable/evolving technologies to ensure we land on the best long-term path for the protocol + users.

**TLDR:**

it’s safe to say we plan on doing a bit more of at least two things over the next ~3-6 months:

(1) evaluate binary: invest meaningful bandwidth into exploring / benchmarking a binary tree structure, while collaborating closely with zk teams. Make sure we understand where we are today in terms of performance, hardware requirements (with which hash function etc.), and where things need to be in order to be viable on L1.

(2) tree-agnostic development: continue building the infrastucture and tooling necessary for statelessness, but lean into a tree-agnostic approach to optimize for reusability. This will give us flexibility to land on the best solution, whether it’s Verkle, Binary, or anything else. In any case, much of what has already been built (e.g. for state migration) will be a valuable and necessary component since it’s unlikely we stick with the current MPT for long.

If you are excited about making progress on statelessness and scaling the L1, you can join the conversation in our biweekly implementers call 🚀

### **4. Deletions in Verkle**

Discussion around whether *not* having deletions in Verkle will bloat the state. There are also downsides to deletions though, as it may make the conversion process a bit more complicated. TBD on final decision, but no strong objections raised to supporting deletions in Verkle. Recommend watching the recording for anyone interested in better understanding the full picture on this topic.

### **5. Pectra Impact (7702, EOF, etc.)**

Ignacio gave an overview of some work he’s done to better understand the potential impact of EOF on Verkle. He created a [draft PR](https://github.com/jsign/EIPs/pull/2) with the changes required. Guillaume also shared some thoughts on things we need to be mindful of with [7702](https://eips.ethereum.org/EIPS/eip-7702). Namely around making sure that when you add something to the witness, you add the contract that the operation is delegated to instead of the account itself (otherwise the witness wil be empty).

### **6. Verkle Sync**

Geth team recently had a discussion on the topic of sync, and came away with a few potential suggestions.

(1) around witness validation: full nodes can validate witnesses and make sure that no extra data is being passed as a way to prevent bloating of the witness. (e.g. flag a block that has too many leaves as invalid). Note: if we do introduce this rule, then we have to make the witness itself part of the block.

(2) snapshot per stem, rather than by hash like it currently is.

(3) how long to save the witness on disk: if we save it for something like a month or so, then it makes it a bit simpler for nodes who have been offline for a short period of time (e.g. 2-3 weeks) to rejoin the network. The tradeoff is it would add around 60GB.

## [**Call #23: August 26, 2024**](https://github.com/ethereum/pm/issues/1121)

[Recording here](https://youtu.be/VD0P3RkhIjY)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): making solid progress on testing and the upcoming testnet. Managed to get the branch with latest gas schedule working with tests.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): have implemented everything for the testnet. Also working on a few cryptography improvements, and a big refactor to implement the transition. Might be able to implement test transition in next few weeks.

[@kt2am1990](https://twitter.com/kt2am1990) for [@HyperledgerBesu](https://twitter.com/HyperledgerBesu): working on modifying how we save data in the DB, to implement a flat DB based on the stem of the tree. Also working on gas cost implementation and the transition.

[@GabRocheleau](https://twitter.com/GabRocheleau) for [@EFJavaScript](https://twitter.com/EFJavaScript): ready with all the gas cost updates, likewise just waiting for test vectors to confirm.

Somnath for [@ErigonEth](https://twitter.com/erigoneth): getting caught up with the changes up to the most recent testnet. Had an issue when trying to connect to peers on testnet-6, not getting any replies. Will debug with ops.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: have released latest fixtures, can be found [here](https://github.com/ethereum/execution-spec-tests/releases/tag/verkle%40v0.0.3).

### **2. Testing Verkle overview**

Next up, we had a brief [presentation](https://hackmd.io/@jsign/verkle-testing) from [@ignaciohagopian](https://x.com/ignaciohagopian) to walk through latest overall progress on the test framework for Verkle. The main branch for the test vectors can be found [here](https://github.com/ethereum/execution-spec-tests/tree/verkle/main) in the execution spec tests repo. And to look at existing tests you can find them [here](https://github.com/ethereum/execution-spec-tests/tree/verkle/main/tests/verkle) in the tests/verkle folder, where all the tests are separated by EIP. Encourage anyone interested to poke around, ask questions, and eventually open up new test cases :)

Ignacio also gave an overview of the changes that were made in geth to support the test framework, how the CI pipeline works, and a summary of all the test fixtures that exist today. The fixtures can be separated into 3 groups:

(1) Verkle-genesis, where everything happens in a post-merkle patricia tree (MPT) world,

(2) overlay-tree, which is running tests with a “frozen” MPT, and doing the block execution in the Verkle tree,

(3) consuming tests from previous forks, to check pre-Verkle execution isn’t broken

![image](https://storage.googleapis.com/ethereum-hackmd/upload_dfdf8c57c53353d60dc2465740f77117.png)

### **3. Testnet readiness check**

Next up, we went through and double checked each team’s readiness for the next testnet. Geth, Nethermind, and EthJS are ready as far as we can tell at this point, while Besu is still finishing up gas cost updates.

### **4. Binary Tree Exploration**

Last up, Guillaume shared a presentation which summarizes some recent discussions we’ve had, as there’s been a bit of a renewed push by some in the community to explore binary trees as a potential alternative to Verkle. Some of this push has been motivated by recent progress made in ZK proving performance, and a desire to provide something that is even more zk-friendly than Verkle to help accelerate the move towards an eventual fully SNARK-ified L1.

Highly recommend watching the recording to view Guillaume’s full presentation, but to give a rough TLDR: the main advantage of Verkle (apart from the fact that progress on Verkle is much further along today compared to a binary tree alternative) is that Verkle gives us small proofs (~400kb). And these proofs can enable stateless clients pretty much as soon as we ship Verkle.

On the other hand, the advantages of binary trees is that hashing performance / commitment computation are much faster in general. And are also more compatible with current ZK proving schemes. The other advantage is around quantum resistance, though of course still much debate around timelines for quantum, and there are several other areas of the protocol that will need to be upgraded as part of any post-quantum push.

The good news is: in either case, much of the work we’ve already done with Verkle is reusable in a binary tree design: (1) gas cost changes, (2) the single-tree structure, (3) the conversion & preimage distribution, and (4) (potentially) sync.

What would change with binary trees though include: (1) the proof system, and (2) the cryptography (replacing polynomial commitments/pedersen with hashes)

## [**Call #22: July 29, 2024**](https://github.com/ethereum/pm/issues/1119)

[Recording here](https://youtu.be/W1SLIEQ3a5o)

### **1. Team updates**

[@ignaciohagopian](https://x.com/ignaciohagopian) and [@gballet](https://x.com/gballet) for [@go_ethereum](https://twitter.com/go_ethereum): working on implementing the fill_cost updates for next testnet. Also did a series of measurements around witness size, and opened an EIP for the state transition. Will share more on this later in the call.

[@jasoriatanishq](https://twitter.com/jasoriatanishq) for [@nethermindeth](https://x.com/nethermindeth): have implemented all the changes discussed for testnet7. Still need to pass the Hive test.

[@techbro_ccoli](https://twitter.com/techbro_ccoli) for the testing team: Spencer gave a quick summary of the current tests available for Verkle. At the moment, we only have transition tests, and no state tests. Will work on getting the remaining tests ready for the newest test cases that Ignacio has written, so we can run those prior to cutting the next testnet. Ideally will also soon make it easy for everyone to just run a command to build and test everything. Can find the latest Verkle tests [here](https://github.com/ethereum/execution-spec-tests/releases/tag/eip6800%40v0.0.1).

### **2. SSZ and witness size improvements**

[@gballet](https://x.com/gballet) shared a summary of three different approaches we are exploring around the encoding of witnesses. The current spec as-is results in unnecessarily large witnesses, due to some nuance around the handling by SSZ libraries. So we are exploring alternative approaches. Recommend watching the full recording to get all the details. But the tldr is that these alternative approaches look very promising, and should reduce witness size by 50% or more, to where the median witness size will be under 300kb, and the max witness size will be under 1MB. There are some future potential optimizations Guillaume is looking into as well which may reduce witness size by another 50%.

### **3. Kaustinen updates (testnet #7)**

We took a few mins to go through any last minute changes or additions we’d like to include in the next testnet: Kaustinen 7. The first topic was revisiting the verkle proof format, and changing it from SSZ/JSON into an opaque binary format. It’s generally agreed we should do it in the long-run, but still a question of whether to do it for this next testnet. Decision on the call was to wait and not do this in Kaustinen 7.

The next topic on Kaustinen 7 was whether to include the fill_cost updates. Decision on the call was to not include it, because it would slow things down significantly, and require a tricky rebase (merging it with more recent code).

### **4. EIP for the Verkle state transition**

Last up, [@ignaciohagopian](https://x.com/ignaciohagopian) walked us through a few recent PRs he put up with quick fixes to EIP-7612 and EIP-6800, as well as a new EIP for the actual transition (where we migrate all state from the Merkle Patricia Tree over to the Verkle Tree). This new EIP can be found at [EIP-7748](https://github.com/ethereum/EIPs/pull/8752).

The main goal of this new EIP is to formalize the state conversion algorithm that is being implemented by clients. (This is the same design that we have discussed for several months, but simply hadn’t been formalized into an EIP). Check out the linked PR for a more detailed explanation, but the short summary is that the state migration involves converting a fixed number of key-values each block (e.g. 10k values) over an extended period of time (e.g. 4 weeks). The number of key-values converted each block can be adjusted up or down, but is currently set to a conservative number to ensure that even modest hardware is able to keep up with the transition. This has already been successfully implemented in Geth, and more clients are following soon behind.

## [**Call #21: July 15, 2024**](https://github.com/ethereum/pm/issues/1092)

### **1. Team updates**

@ignaciohagopian and @gballet for @go_ethereum: finishing up document on code chunking gas cost overhead in Verkle. Also working on more test vectors for the state conversion (converting state from Merkle to Verkle). And putting together first draft of the state conversion EIP, to help with coordination and act as the source of truth for test vectors. In addition, Guillaume has been collecting witness size data, preparing for the testnet relaunch, and busy merging more Verkle stuff into Geth so we can have a testnet on top of Dencun.

@DoctZed for @HyperledgerBesu: currently optimizing around storage in the DB, and moving some of the work to background threads. Also finishing up the updates to gas cost schedule.

@techbro_ccoli for testing team: continuing work on the genesis test vectors. Also discussed a bit with Guillaume about making the tests runnable by anyone, so that each client team can check prior to relaunching the next testnet. @techbro_ccoli to put together a document on how to run the tests easily. Can find the tests here: https://github.com/ethereum/execution-spec-tests/releases

### **2. Testnet readiness**

We went through the main items we want to include on the next testnet, to see where client teams are in terms of readiness. Geth is still working through a few items: notably fill_cost and other gas cost updates. Nethermind has implemented everything previously discussed at the client team interop, but still need to run latest test framework to verify. Besu is not yet complete, and need to wrap up some of the DB work they are currently in process of refactoring before finishing up testnet work.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_39205c7cb2c60e94ed208611c141230f.png)

### **3. Witness size measurements**

Next up, Guillaume shared some recent experiments he has been running around witness sizes, as well as some potential optimizations. Tldr: in the current version of the spec we store the state diff in a certain way with the old value and new value as optionals.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_9ebabc709fdfd0a38b3fb3694e87040b.png)

The issue with this approach is that very few SSZ libraries support optionals, and due to how other libraries handle this it makes for unnecessarily large witnesses.

So, one proposal to fix this: get rid of optionals by grouping all the suffixes together in an array of bytes, and grouping all the old values and new values likewise in their own lists.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_232fbaad5b7d3b0d15de3329986a5279.png)

Testing this new approach (vs. the current approach) results in significant reduction in witness size. The current approach has a median witness size of ~700kb. While the new approach has a median witness size of ~400kb. One note: we can further reduce the witness size significantly (possibly another 50%) by removing new_values, which would put witness size closer to 200kb.

![image](https://storage.googleapis.com/ethereum-hackmd/upload_4df9b14961c3c2037fd05b8006262085.png)

Guillaume also shared a 2nd proposal for an optimization which could further reduce witness size. (Still need to implement it to get the numbers). Will come back to this discussion on a future call.

### **4. Other Misc EIP Updates**

Next up, we went through a few updates to EIPs including EIP-6800 and EIP-2935. See the complete list here: https://notes.ethereum.org/@gballet/S1YEC5fOA#/5.

This is mostly relevant for client devs and other implementers, but recommend watching the recording for anyone interested!

### **5. Code-access Gas Overhead**

Last up, @ignaciohagopian walked us through a document he put together, which provides a very helpful analysis of the cost of code chunking in Verkle.

For those not familiar: in order to fully support stateless clients in a post-Verkle world, we will have to add contract code to the tree (in addition to account data and contract storage). This is necessary for clients to statelessly validate blocks. This process of transforming contract bytecode into tree key-values is called “code-chunking”. But code-chunking is not free, and will add some new gas cost overhead which doesn’t exist today in a stateful world.

Ignacio’s document does a great job at summarizing, analyzing, and approximating what these costs will be with Verkle by looking at ~1 million recent mainnet transactions. The TLDR is that, on avg, we should expect around 30% gas cost overhead. The good news is there are several ways we can potentially mitigate this. Importantly, paying this extra cost will unlock stateless clients, which will allow us to significantly raise the gas limit (more than 30%) to more than offset this additional cost. See https://x.com/dankrad/status/1790721271321256214.

Highly recommend watching the recording, and also going through Ignacio’s full document here for anyone curious to learn more: https://hackmd.io/@jsign/verkle-code-mainnet-chunking-analysis.
