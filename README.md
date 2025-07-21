# **Foundry Smart Contract Lottery**

## _Project Overview_

This project implements a decentralized lottery (raffle) smart contract where players enter by sending ETH and potentially win the entire prize pool. Randomness is securely sourced via Chainlink VRF v2.5, using the VRFConsumerBaseV2Plus interface to guarantee tamper-proof winner selection.

## _Technologies Used_

- Solidity 0.8.19
- Foundry (Forge & Anvil)
- Chainlink VRF v2.5 (VRFConsumerBaseV2Plus)
- Sepolia Testnet for deployment and testing

## _Dependencies_

The project uses these dependencies (installed via Forge):

```bash
forge install cyfrin/foundry-devops@0.2.2
forge install smartcontractkit/chainlink-brownie-contracts@1.1.1
forge install foundry-rs/forge-std@v1.8.2
forge install transmissions11/solmate@v6
```

## _Installation & Setup_

1. Clone the repository:

```bash
git clone https://github.com/WhatFate/foundry-smart-contract-lottery.git
cd foundry-smart-contract-lottery
```

2. Install dependencies:

```bash
make install
```

## _Build_

Compile the smart contracts:

```bash
make build
```

## _Testing_

Run all tests locally (including mocks and integration):

```bash
make test
```

## _Deployment_

### Deploy the contract and configure Chainlink VRF subscription

The deployment script (`script/DeployRaffle.s.sol`) automates:

- Creating and funding a Chainlink VRF subscription (on local and Sepolia networks)
- Deploying the `Raffle` contract with parameters from `HelperConfig`
- Adding the deployed `Raffle` contract as a consumer to the VRF subscription

Run the deployment using:

```bash
make deploy-sepolia
```

Or manually with Foundry scripts:

```bash
forge script script/DeployRaffle.s.sol --broadcast --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

### Notes on deployment scripts:

- `HelperConfig.s.sol` provides network-specific config, including addresses, subscription IDs, and parameters for Sepolia and local Anvil fork.
- `Interactions.s.sol` contains scripts to create and fund VRF subscriptions and add consumers, which the deploy script calls automatically if needed.

## _Usage_

After deployment, players can enter the raffle by sending at least the configured entrance fee in ETH to the `enterRaffle()` function.

The contract uses Chainlink Keepers (or manual calls to `performUpkeep`) to trigger the random winner selection process based on time intervals.

## Additional Notes

- The randomness uses Chainlink VRF v2.5 with native ETH payment support.
- The contract resets after each round, allowing continuous lottery cycles.
- The project is designed for easy adaptation to other networks by modifying `HelperConfig`.

---

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
