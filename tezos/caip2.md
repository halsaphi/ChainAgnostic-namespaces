---
namespace-identifier: tezos-caip2
title: Tezos Namespace - Blockchain ID Specification
author: Stanly Johnson (@stanly-johnson), Carlo van Driesten (@jdsika)
discussions-to: ["https://github.com/ChainAgnostic/CAIPs/pull/36", "https://gitlab.com/tezos/tezos/-/issues/1029", https://github.com/ChainAgnostic/namespaces/pull/40]
status: Draft
type: Standard
created: 2020-12-12
updated: 2024-02-28
requires: CAIP-2
supersedes: CAIP-26
---


# CAIP-2

*For context, see the [CAIP-2][] specification.*

## Rationale

In CAIP-2 a general blockchain identification scheme is defined. This is the implementation of CAIP-2 for the chain identification system of the Tezos namespace.

## Syntax

Blockchains in the `tezos` namespace are identified by their chain ID derived deterministically from a short, prefixed Blake-2B hash of their `genesis-block-hash`.

### Reference Definition

The method for calculating the hash of a given chain's genesis block (for use as a CAIP-2 chain ID) is as follows from the [Base58 Check Encoded Blake2B Hash][] reference implementation:

```ocaml
(* Net(15) *)
tezosB58CheckEncode('Net',
  firstFourBytes(
    blake2b(msg = tezosB58CheckDecode('B', genesisBlockHash),
            size = 32)))
```

### Chain ID alias

The Tezos community recognizes the different chains according to human readable names, which are called the [Networks][].
The Octez RPC, for example, allows you to connect to three predefined networks by alias:

```bash
# mainnet (default)
# sandbox
# ghostnet

> ./octez-node run --data-dir ~/tezos-ghostnet --network ghostnet
```

There is currently no algorithm to calculate the network's CAIP-2 chain ID from the `Networks` in the Octez reference implementation of Tezos.
In general it is determined by social consensus what chain ID is considered as `tezos:mainnet` and therefore reflected in the Octez `Networks` RPC implementation.
It is RECOMMENDED to use a separate chain registry to map an alias to the chain ID.

### Backwards Compatibility

Not applicable.

## Integrity guarantees for the chain ID

The third chapter of the [Tezos Position Paper][] is dedicated to the analysis of potential threats leading to a user connecting to the Tezos network for the first time and not ending up on the `tezos:mainnet`.
One of the main features of the technology is the [On-Chain Governance][] including its consensus mechanism based on [Proof-of-Stake][]. 
In combination with social consensus around periodic chain-state "checkpoints" used to bootstrap nodes and anchor shared ground-truth for, e.g. [Tezos Block Explorer][], a statistical analysis of the chain using TAPOS ("transactions as proof of stake") serves as guarantee of the integrity of the mainnet regarding these threat models. 
The current mainnet has been running without a pertinent protocol-level security breach or consensus attack since the `30th of June 2018`.

## Test Cases

This is a list of manually composed examples.
See [Tezos test network infrastructure][] for available public chains.
You can use the [Tezos RPC Interface][] to compute the chain id from a block hash as follows:

```bash
# Tezos Ghostnet (Long-running test network)
> ./octez-client compute chain id from block hash BLockGenesisGenesisGenesisGenesisGenesis1db77eJNeJ9
NetXnHfVqm9iesp
```

Now the CAIP-2 compliant chain ID can be constructed using the Tezos namespace as prefix:

```bash
# Tezos Mainnet
# Genesis block hash: BLockGenesisGenesisGenesisGenesisGenesisf79b5d1CoW2
tezos:NetXdQprcVkpaWU

# Tezos Ghostnet (Long-running test network)
# Genesis block hash: BLockGenesisGenesisGenesisGenesisGenesis1db77eJNeJ9
tezos:NetXnHfVqm9iesp
```

The following table includes the chain ID aliases through their human readable network names:

| Alias          | Chain ID                         |
| -------------- | -------------------------------- |
| tezos:mainnet  | tezos:NetXdQprcVkpaWU            |
| tezos:ghostnet | tezos:NetXnHfVqm9iesp            |

## References

- [Tezos Address Types][] - Important context on the Tezos system of addresses and key representations further specified in the `tezos-caip10` derived from [CAIP-10].
- [Tezos RPC Interface][] - Important context on communicating with Tezos nodes over RPC.
- [Tezos Block Explorer][] - Can be used to investigate block hashs on Mainnet and Ghostnet.
- [Chain ID Reference Implementation][] - Octez implementation for Tezos.
- [Octez][] - Main implementation for the Tezos standard.
- [Base58 Check Encoded Blake2B Hash][] - Octez reference implementation.
- [Taquito Typescript Library][] - Available Base58 functions in Typescript for Tezos.
- [Tezos Position Paper][] - Reference regarding mitigations in section 3.2.
- [On-Chain Governance][] - Tezos website with details about the on-chain governance system.

[CAIP-2]: https://chainagnostic.org/CAIPs/caip-2
[Tezos Address Types]: https://tezos.gitlab.io/introduction/howtouse.html#implicit-accounts-and-smart-contracts
[Tezos RPC Interface]: https://tezos.gitlab.io/introduction/howtouse.html#rpc-interface
[Networks]: http://tezos.gitlab.io/user/multinetwork.html?highlight=network%20name#test-networks
[Tezos Block Explorer]: https://tzstats.com/
[Chain ID Reference Implementation]: https://gitlab.com/tezos/tezos/-/blob/5bb8fd589cc8777f44c795b71acf3e0a5dcac06f/src/lib_crypto/chain_id.ml
[Octez]: https://research-development.nomadic-labs.com/announcing-octez.html
[Base58 Check Encoded Blake2B Hash]: https://gitlab.com/tezos/tezos/-/blob/5bb8fd589cc8777f44c795b71acf3e0a5dcac06f/src/lib_crypto/blake2B.ml
[Taquito Typescript Library]: https://tezostaquito.io/typedoc/functions/_taquito_utils.b58decode#b58decode
[CAIP-10]: https://chainagnostic.org/CAIPs/caip-10
[Tezos test network infrastructure]: https://teztnets.com/
[Tezos Position Paper]: https://tezos.com/position-paper.pdf
[On-Chain Governance]: https://tezos.com/governance
[Proof-of-Stake]: http://tezos.gitlab.io/active/proof_of_stake.html

## Rights

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
