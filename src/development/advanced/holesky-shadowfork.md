# Testing the Holesky shadowfork with geth's devmode

## Prerequisites

 - Geth's [kaustinen-with-shapella](https://github.com/gballet/go-ethereum/tree/kaustinen-with-shapella) branch
 - Download the [pre-pectra holesky database](https://drive.google.com/file/d/1LN7ZCI48dll-8fguqb1LXfB4tAzS5YT-/view?usp=sharing) and unpack it:

```
> tar --zstd -xf snapshot_holesky.tar.zstd
```

This will create a directory called `geth` in your current directory. Save the full path to this directory in `GETH_DIR`.

Make sure to keep a copy of the tar file, as the unpacked database will be written to and the transition will be irreversibly marked as finished.

 - Run the conversion:

```
> go run ./cmd/geth --datadir=$GETH_DIR --dev --dev.period=2 --override.cancun=1900000000 --override.verkle=(date "+%s") --override.overlay-stride=10000
```

Note that:

 * `--dev.period=2` causes a block to be created every 2s, which allows for faster conversion.
 * `--override.cancun=1900000000` is meant to set Cancun far out in the future, as this branch doesn't support Cancun. When Pectra ships, `--override.pectra=` will also need to be specified far out in the future (unless the rebase finally completes).
 * `--override.verkle=(date "+%s")` is meant to set the Verkle fork to happen at the current time, so that the next block will trigger the Verkle conversion.
 * `--override.overlay-stride=10000` sets how many leaves get converted per block. The recommended value is 10k but higher numbers mean the conversion will complete faster.

  - Check the conversion status by RPC:

```
> curl -s -X POST -H "Content-Type: application/json"  -d '{ "id": 7, "jsonrpc": "2.0", "method": "debug_conversionStatus", "params": ["latest"]}' http://localhost:8545 | jq '.result.ended'
```
