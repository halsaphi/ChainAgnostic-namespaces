---
namespace-identifier: tezos-caip10
title: Tezos Namespace - Account ID Specification
author: Carlo van Driesten (@jdsika)
discussions-to: https://github.com/ChainAgnostic/namespaces/pull/40
status: Draft
type: Standard
created: 2022-12-06
updated: 2024-02-28
requires: ["CAIP-2", "CAIP-10"]
---

*For context, see the [CAIP-10][] specification.*

## Rationale

Tezos supports the use of multiple public-key signature schemes; for this reason, a given account can have multiple addresses, each of which is prefixed with `tz1` ([Ed25519][] curve), `tz2` ([Secp256k1][] curve), `tz3` ([NIST P256][] curve), or `tz4` (BLS12-381 curve from the [BLS family][]) referenced in the [Tezos address types glossary][].
After the prefix, the rest of the account address is a [Base58 Check Encoded Blake2B Hash][] of a public key of the type corresponding to the prefix.

## Syntax

The syntax of a Tezos address matches the following regular expression (note the [58-character alphabet][base58]):

`(tz1|tz2|tz3|tz4)[1-9A-HJ-NP-Za-km-z]{33}`

## Chain IDs

For how to compute a valid `chainId` segment or a list of examples, see [the CAIP-2 profile](./caip2.md).

| Alias          | Chain ID                         |
| -------------- | -------------------------------- |
| tezos:mainnet  | tezos:NetXdQprcVkpaWU            |
| tezos:ghostnet | tezos:NetXnHfVqm9iesp            |

Tezos addresses are invariable across networks.

## Test Cases

Manually composed examples follow:

```bash
# Ed25519-key address on Tezos Mainnet
# Genesis block hash: BLockGenesisGenesisGenesisGenesisGenesisf79b5d1CoW2
tezos:NetXdQprcVkpaWU:tz1MJx9vhaNRSimcuXPK2rW4fLccQnDAnVKJ

# NIST p256-key address on Tezos Ghostnet
# Genesis block hash: BLockGenesisGenesisGenesisGenesisGenesis1db77eJNeJ9
tezos:NetXnHfVqm9iesp:tz3btDQsDkqq2G7eBdrrLqetaAfLVw6BnPez
```

## References

- [Tezos address types][] - Important context on the Tezos system of addresses and key representations.
- [Tezos Smart Contract][]: General definition of a Tezos smart contract.
- [CAIP-2][]: Chain ID Specification.
- [CAIP-10][]: Account ID Specification.
- [Ed25519][] - Ed25519: high-speed high-security signatures.
- [Secp256k1][] - Elliptic curve used in Bitcoin's public-key cryptography.
- [NIST P256][] - One of the most used elliptic curves including native support in some mobile devices.
- [BLS family][] - BLS12-381 is a pairing-friendly elliptic curve construction from the BLS family, with embedding degree 12.

[Tezos address types]: https://tezos.gitlab.io/introduction/howtouse.html#implicit-accounts-and-smart-contracts
[Tezos address types glossary]: https://tezos.gitlab.io/active/glossary.html#implicit-account
[Tezos Smart Contract]: https://opentezos.com/tezos-basics/smart-contracts#general-definition-of-a-tezos-smart-contract
[CAIP-2]: https://chainagnostic.org/CAIPs/caip-2
[CAIP-10]: https://chainagnostic.org/CAIPs/caip-10
[Base58]: https://datatracker.ietf.org/doc/html/draft-msporny-base58-03
[Ed25519]: https://ed25519.cr.yp.to/
[Secp256k1]: https://en.bitcoin.it/wiki/Secp256k1
[NIST P256]: https://csrc.nist.gov/csrc/media/events/workshop-on-elliptic-curve-cryptography-standards/documents/papers/session6-adalier-mehmet.pdf
[BLS family]: https://eprint.iacr.org/2002/088
[Base58 Check Encoded Blake2B Hash]: https://gitlab.com/tezos/tezos/-/blob/5bb8fd589cc8777f44c795b71acf3e0a5dcac06f/src/lib_crypto/blake2B.ml

## Rights

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
