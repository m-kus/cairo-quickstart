# Cairo quickstart

## Set up the environment

### 1. Install Scarb

Scarb is a Cairo package and compiler toolchain manager.

The quickest installation option is:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh
```

See more options in the [docs](https://docs.swmansion.com/scarb/download.html)

### 2. Install Foundry

Starknet Foundry is a toolchain for developing smart contracts for Starknet.

Make sure you have [Rust](https://www.rust-lang.org/tools/install) installed!

```sh
curl -L https://raw.githubusercontent.com/foundry-rs/starknet-foundry/master/scripts/install.sh | sh
```

And then run:

```sh
snfoundryup
```

## Create new project

### 3. Initialize new package

```sh
snforge init hello
cd hello
```

Foundry has scaffolded a simple smart contract for us!

### 4. Run tests

```sh
snforge test
```

## Create testnet account

We will work with Starknet Sepolia testnet.

### 5. Generate new account

Set RPC endpoint once to reuse in the next steps.

```sh
export STARKNET_RPC="https://starknet-sepolia.public.blastapi.io/rpc/v0_7"
```

This command will generate an address for our test account contract (using a predefined AA wallets):

```sh
sncast account create --url $STARKNET_RPC --name test
```

It's not deployed yet, we need to send some money to this address first.

### 6. Top up the balance

Use the faucet https://blastapi.io/faucets/starknet-sepolia-eth

> [!WARNING]  
> Some faucets do not correctly handle addresses with leading zeros stripped, if you see "Invalid address" error, just add a zero after '0x'

### 7. Deploy the account

> [!TIP]  
> Testnet RPC may lag behind a bit, if you receive an empty balance error, try again in a few secs

```sh
sncast account deploy --url $STARKNET_RPC --name test --fee-token eth
```

## Deploy the contract

Before deploing our contract we need to declare its class, i.e. "upload the code".

### 8. Declare contract class

> [!NOTE]
> Here `HelloStarknet` is the name of our module behind the `#[starknet::contract]` attribute

```sh
sncast --account test declare --url $STARKNET_RPC --fee-token eth --contract-name HelloStarknet
```

### 9. Deploy contract instance

> [!NOTE]
> Change `class-hash` according to the output on the previous step

```sh
sncast --account test deploy --url $STARKNET_RPC --fee-token eth --class-hash 0x77f8cf6977335519a1050447de23e2365fee99ef1e9fd3412bff1a49870eb78
```

We have created a new instance of our contract class! Our contract has no constructor hence we didn't pass any arguments.

## Interact with the contract

Finally we can play with our contract: any method requiring state update is "invoke", read-only methods are "call".

### 10. Invoke contract

```sh
export HELLO_ADDR=0xf5bff9c8f1c65cfcf0ad60ef0bd4aa6cd81759e43dadabc48de4433b126ad8
```

```sh
sncast --account test invoke --url $STARKNET_RPC --fee-token eth --contract-address $HELLO_ADDR --function "increase_balance" --calldata 42
```

### 11. Call contract

```sh
sncast call --url $STARKNET_RPC --contract-address $HELLO_ADDR --function "get_balance"
```

## Resources

Cairo tutorials:
* Build fully onchain games with [Dojo](https://book.dojoengine.org/tutorial/dojo-starter)
* Build verifiable ML pipelines with [Orion](https://orion.gizatech.xyz/academy/tutorials)
* Deploy Circom circuit verifier with [Garaga](https://garaga.gitbook.io/garaga/deploy-your-snark-verifier-on-starknet)
* Write in Solidity with Cairo precompiles using [Kakarot](https://docs.kakarot.org/starknet/architecture/cairo-precompiles)
* Use data feeds from [Pragma](https://docs.pragma.build/v1/Resources/Consuming%20Data%20Feed)
* Find more at https://www.starknet.io/tutorials/

Demo projects for inspiration:
* Recognize when people are smiling with [Vibe check](https://github.com/Hyle-org/vibe-check)
* Bitcoin script engine [Shinigami](https://github.com/keep-starknet-strange/shinigami)
* Fiat onramp via zkTLS with [zkRamp](https://github.com/keep-starknet-strange/zkramp)
* [AI agents](https://github.com/keep-starknet-strange/agentstark) on Starknet
* Fully onchain road fighter [Drive AI](https://github.com/cartridge-gg/drive-ai)
* Beer brewing game [Beer Baron](https://github.com/cartridge-gg/beer-baron)
* A Telegram mini app game [Beast Slayers](https://github.com/cartridge-gg/beast-slayers)

## Get help

Join Starknet [Discord](https://discord.gg/4vUZATYu) server.
