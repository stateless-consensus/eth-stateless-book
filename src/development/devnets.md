# Devnets

As a way to test this very complex change to the protocol, devnets are frequently run to assess the code quality and make sure that every client implementation agrees on the spec. This has helped identify many corner cases of the specification as well as find many issues in client implementations.

This typically includes:

 * A faucet so that developers can obtain tokens to deploy their contracts on the devnet.
 * A block explorer, which displays witness content.
 * A witness explorer, which is a tool that we can use to look at the produced witnesses as well as figuring out what are the specific gas costs related to Stateless Ethereum.
 * The specification sheets that lists the peculiarities of this devnet.
 * Various RPC endpoints to interact with the devnet.
 * A forkmon instance to check that all the nodes on the devnet agree on what the head block is.
 * Information about the genesis block and other network configuration items, that any node that wants to join the devnet will need.

There’s currently an active [devnet-7](https://verkle-gen-devnet-7.ethpandaops.io) which can be used for core-developers and users to make progress in the implementations of proposed protocol changes. The latest devnet is always available at the address [kaustinen-testnet.ethpandaops.io](https://kaustinen-testnet.ethpandaops.io).

## Joining the devnet

For execution layer developers implementing stateless EIPs or anybody interested in just following the devnet in order to use their node as a private RPC endpoint, we recommend using the two called [lodestar-quickstart](https://github.com/ChainSafe/lodestar-quickstart).

Make sure that [docker](https://docker.io) is installed on your system. Sync the repository for the first time:

```
$ git clone https://github.com/ChainSafe/lodestar-quickstart # sync repository
$ cd lodestar-quickstart
```
​
Then, prepare the devnet configuration directory and launch the sync (repeat these steps each time):

```
$ rm -rf k7data # don't forget to use these commands between runs
$ ./setup.sh --network kaustinen7 --dataDir k7data
```
​
Some of these commands might require `sudo` or super-user privileges.

This will start a container for the consensus layer clients and a container for the execution layer client. If you want to test your execution layer client, just ask `lodestar-quickstart` to only start the CL:

```
$ rm -rf k7data # don't forget to use these commands between runs
$ ./setup.sh --network kaustinen7 --dataDir k7data --justCL
```
​
Then, start the EL manually on the side. For example, with geth:

```
$ go run ./cmd/geth --cache.preimages init \
    path/to/lodestar-quickstart/k7data/network-configs/gen-devnet-7/metadata/genesis.json
$ go run ./cmd/geth --syncmode=full --bootnodes \
    "enode://548ff025abb1522c5257f50765abd21754b7ea7159a176a9b96c738ee6456fc378a11c09a62d55d92684634cd32a9cad498f5649256caf693dab77f961a169f6@167.235.68.89:30303" \
    --authrpc.secret=path/to/lodestar-quickstart/k7data/jwtsecret
​```
