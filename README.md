# Onchain VRF

_Making randomness collective, trustless, permissionless_

> 🏆 Grand Prize Winner of the Aleo zkHouse Token2049 Singapore 🏆

## Problem

How can we have randomness for onchain turn-based games that is:

1. fair
2. transparent
3. decentralized

## Solution

An Onchain Verifiable Random Function that consists of pre-game commitments with per-round additive randomness generation.

### How it works

#### Pre-game

- all users generate a _User Secret Hash_ using a User Secret of their choice
- combine everyone's User Secret Hash into a _Game Hash_

#### At every round for each player

1. Generate new Random Number, which is a hash function of:
   1. User Secret
   2. Game Hash
   3. Previous Random Number
   4. Turn Number
2. Circuit ensures User Secret Hash equals to Pre-game User Secret Hash

### How to run

```bash
./demo.sh
```

### How to build

```bash
leo build
```

### How to deploy

The contract is deployed at `testnet3` at `at1gvyv90m3rqyzmq4qtyt9a0m6s2m8qdlcee9prefhvvuf70pn8upq4k0wqn`

You can deploy your own running:

```bash
./deploy.sh
```

## Future Work

The current implementation has limitations that can be addressed future iterations, namely:

- As arrays are not currently available, we have hardcoded the demonstration of the VRF to 4 players
- The Poseidon hash function (or any hash function) in Leo only takes in a single input. In our case, where we have a fan in of 4 to the hash function, we had to opt for a workaround of summing the 4 inputs together to form a single input. Though this sounds ‘dirty’, there is no real cause of concern as there is no easy way to manipulate the game hash, previous number and user secret. The output random number is still very reliable
- Currently we are using struct for the registration and returning the random number and also the input for the function to generate the next random number, which we originally wanted to use Record for. Ideally we integrate the use of records for the initialization process somehow to allow some sort of additional commitment on-chain for their secrets.
- There are some additional checks that we should add in order to enforce the order of random number generation and prevent going back via the use of a previous output
- The comparison bug found during the hackathon also prevented proper constraints to work as expected.
- We need to come up with a way for players to query the previous random number via the Leo program.
