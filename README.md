# cliquebait
Test your DApps easily with this one weird trick!

## Overview
Cliquebait is a compact, fun, and easy to use Docker image that spins up a single-node Proof of Authority blockchain with Geth. It also generates some accounts and gives them ether, as well as unlocks them for use via Web3.

#### Why?
Cliquebait simplifies integration and accepting testing of smart contract applications by offering a clean ephemeral testing environment that closely resembles a real blockchain network. The key differences are a shorter block time (3 seconds by default) and a VERY high gas limit from the start.

#### But geth and parity have `--dev` now!
Very true! However, `geth` only has `--dev` mode as of v1.7.3, which also introduced slight changes to the JSON-RPC interface. Additionally, without careful configuration to avoid lazy block mining, you lose the ability to test things like waiting for block intervals and such.

## Quickstart
Simply run `docker run --rm -it -p 8545:8545 foamspace/cliquebait:latest` and connect to `http://localhost:8545/` using your Web3 interface of choice!


## Advanced Usage

### Create more (or less) accounts on startup
Simply supply the `ACCOUNTS_TO_CREATE` environment variable to `docker run`. The value must be numeric, in base 10, and at least 1

For example (creates 10 accounts on startup): `docker run --rm -it -p 8545:8545 -e ACCOUNTS_TO_CREATE=10 foamspace/cliquebait:latest`

### Tweak the genesis block
One may mount a custom genesis block to `/cliquebait/cliquebait.json` to really fine tune their network's behavior. The only key thing to keep in mind is that the `extraData` field MUST remain the same. Cliquebait supplies the address of an ephemeral "authority" for the network on startup, and for the image to behave properly this must remain as-is.

Alternatively, you may pass in a genesis JSON directly via the `GENESIS_JSON` environment variable. The same rules regarding `extraData` apply!

### Use specific accounts
If you have an account JSON file compatible with geth's keystore, you may embed it into a specially crafted JSON file (see `sample-extra-accounts.json`) and supply it to cliquebait. You may then mount this file as `/extra-accounts.json`, and cliquebait will allocate ether and unlock the account for use in Web3. Note that this involves supplying the password to the account in plaintext, so be careful! If you prefer not to have the account unlocked, you may simply add an `alloc` in the genesis block.
