# Testing

- [Testing](#testing)
  - [Execution spec tests](#execution-spec-tests)
  - [Filling \& running tests](#filling--running-tests)
  - [CI](#ci)

Ethereum stateless protocol changes cut deep into the protocol since it must simultaneously change many angles, such as the state tree, EVM gas costs, and opcode functioning.

## Execution spec tests

One strategy for testing is using devnets, where multiple clients join and can check if they reach a consensus on executing automatically generated transactions or letting users experiment with the new features. While they’re very useful, it’s pretty costly to spin up a devnet, mainly in the early to mid stages of protocol development for clients — mainly on the coordination front.

A more efficient approach is creating a comprehensive [test suite runnable by every client](https://ethereum.github.io/execution-spec-tests/main/). Clients can efficiently run these test suites while they’re in development, both manually and in CIs. Since this doesn’t require a devnet, there’s no required coordination or overheads, allowing us to make progress faster. Moreover, they allow us to precisely describe corner case scenarios, which would be hard to replicate on every development spin-up.

There’s currently an [active branch in execution-spec-tests](https://github.com/ethereum/execution-spec-tests/tree/verkle/main/tests/verkle), which contains tests for:

- EIP-4762
- EIP-6800
- EIP-7709
- EIP-7612

The current test suite contains ~200 test fixtures, which have uncovered more than 15 consensus bugs throughout many EL clients, usually in intricate corner cases. It is still far from perfect, and there’s always ongoing effort to improve it. For example, if any bug is detected in a devnet, adding it as a new test is important.

Note that filling tests for such deep protocol changes involved more than creating the tests; it also involved making big changes in the testing framework and Geth’s `evm t8n` filling tools. This effort was made collaboratively between the Stateless Consensus and the STEEL teams at the EF.

## Filling & running tests

The [execution spec test documentation](https://ethereum.github.io/execution-spec-tests/main/filling_tests/) is an excellent resource for understanding the overall process. Next, we’ll provide concrete steps referencing which are the right branches to be used to fill the test properly:

1. [Install the required prerequisites to use the testing framework](https://ethereum.github.io/execution-spec-tests/main/getting_started/installation/).
2. [Pull the main branch from this repo](https://github.com/gballet/go-ethereum).
    1. Run `go build -o evm ./cmd/evm`
    2. Save the generated binary in `PATH_A`
3. [Pull the `verkle/main` branch from the execution-spec-tests repo](https://github.com/ethereum/execution-spec-tests/tree/verkle/main)
    1. Run `uv run fill --fork Verkle -v -m blockchain_test -n auto --evm-bin=<PATH_A>` to fill the tests. You can use whatever extra flags are described in the testing framework documentation to filter fillings.
4. In the `fixtures` folder, you’ll find the generated fixtures.

The command for running the test depends on your EL client. For example, in Geth, you should `go run ./cmd/blocktest <json fixture path>`.

## CI

In the Geth branch used for stateless development, there are GitHub Actions workflows:

- [Fill & consume tests](https://github.com/gballet/go-ethereum/blob/kaustinen-with-shapella/.github/workflows/spec-tests-branch.yml)
- [Consume stable fixtures](https://github.com/gballet/go-ethereum/blob/kaustinen-with-shapella/.github/workflows/stable-spec-tests.yml)

These are run on every new PR, so there’s quick feedback if a new feature or bug fix has a regression — they can also be helpful for other EL teams to include in their pipelines. You can also semi-derive the steps mentioned in the *Filling tests* section by reading the CI steps.
