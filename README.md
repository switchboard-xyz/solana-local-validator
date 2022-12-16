# Solana Local Validator GH Action

Start a local Solana validator with a feature set mirrored from devnet, testnet,
mainnet, or all features enabled. See
[scfsd](https://github.com/FrankC01/solana-gadgets/tree/main/rust/scfsd) for
more info.

## Inputs

| Variable       | Default   | Description                                                                                                                                                                                                       |
| -------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| rust-version   | stable    | **Toolchain name to use.**<br />Ex. stable, nightly, nightly-2019-04-20, or 1.32.0                                                                                                                                |
| solana-version | stable    | **The version of Solana to install for the local validator.**<br />Can be set to stable, beta, edge, or a specific version like v1.14.10                                                                          |
| anchor-version | undefined | **The version of Anchor to install. If not provided then Anchor will _NOT_ be installed.**<br />Can be set to any anchor tag [https://github.com/coral-xyz/anchor/tags](https://github.com/coral-xyz/anchor/tags) |
| cluster        | local     | **Set the cluster to enable a set of features for to mirror production environments.**<br />Options: all, local, devnet, testnet, mainnet                                                                         |
| args           | N/A       | **Optional arguements to be passed to solana-test-validator such as a list of accounts to copy.**<br />Example: "--url `https://api.devnet.solana.com` --clone 2TfB33aLaneQb5TNVwyDz3jSZXS6jdW2ARw1Dgf84XCG"      |
| quiet          | true      | **Whether to suppress the validator console output.**                                                                                                                                                             |

## Basic Example

```yml
name: Example Workflow
on: push
jobs:
  example_workflow:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Start Local Validator
        uses: switchboard-xyz/solana-local-validator@v0.1
        with:
          solana-version: stable
          anchor-version: v0.26.0
          cluster: devnet
      # Local validator will continue to run for any downstream steps
      - name: Test Local Validator
        run: |
          curl -sS http://localhost:8899 -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":1, "method":"getBlockHeight"}'
```

## Advanced Example

The following example is how to startup a local validator with the Switchboard
program pre-loaded:

```yml
name: Example Workflow
on: push
jobs:
  example_workflow:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Start Local Validator
        uses: switchboard-xyz/solana-local-validator@v0.1
        with:
          solana-version: v1.14.10
          anchor-version: v0.26.0
          cluster: devnet
          quiet: false
          args:
            "--url https://api.devnet.solana.com  --clone
            2TfB33aLaneQb5TNVwyDz3jSZXS6jdW2ARw1Dgf84XCG --clone
            J4CArpsbrZqu1axqQ4AnrqREs3jwoyA1M5LMiQQmAzB9 --clone
            CKwZcshn4XDvhaWVH9EXnk3iu19t6t5xP2Sy2pD6TRDp --clone
            BYM81n8HvTJuqZU1PmTVcwZ9G8uoji7FKM6EaPkwphPt --clone
            FVLfR6C2ckZhbSwBzZY4CX7YBcddUSge5BNeGQv5eKhy"
      # Local validator will continue to run for any downstream steps
      - name: Test Local Validator
        run: |
          curl -sS http://localhost:8899 -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":1, "method":"getBlockHeight"}'
```
