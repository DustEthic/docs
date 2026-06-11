# Wallets Whitepaper - DustEthic

**Status: Phase 0 - design** · **Version: v0.1-draft** · **Date: 2026-06-11** · **License: CC BY 4.0**

> Technical specification for teams building a wallet. For a non-technical overview, read the [Guide](../guide/guide-en.md) first.

## Contents

- [About this document](#about-this-document)
- [Language conventions](#language-conventions)
- [The model in brief](#the-model-in-brief)
- [Normative scope](#normative-scope)
- [The six principles](#the-six-principles)
- [Flow architecture](#flow-architecture)
- [Immediate mode](#immediate-mode)
- [Aggregated mode](#aggregated-mode)
- [Wallet compatibility](#wallet-compatibility)
- [Network fee coverage](#network-fee-coverage)
- [Supported assets and networks](#supported-assets-and-networks)
- [Receipt and traceability](#receipt-and-traceability)
- [Security](#security)
- [NGO list](#ngo-list)
- [Standard compliance](#standard-compliance)
- [Status, versioning and contribution](#status-versioning-and-contribution)

## About this document

This whitepaper specifies how a wallet implements the DustEthic standard. It is aimed at wallet developers and architects.

Points still open at this stage of the design are marked as follows:

> **To be defined (v0.x)**: description of the open question.

These callouts are not oversights: they are design decisions not yet settled, stated openly in line with the project's radical transparency. Any proposal to settle them is welcome via Issue or Pull Request.

## Language conventions

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT** and **MAY** are to be interpreted as described in RFC 2119 / BCP 14. A compliant implementation satisfies all **MUST** and **MUST NOT** requirements.

## The model in brief

DustEthic describes how a wallet turns a user's residual balances (dust) into a direct donation to an NGO, with no intermediary, no commission and no token. The wallet is the central actor: it detects, presents, prepares and sends the operation after signature, without ever holding the funds in custody.

The distribution formula is: **gross amount - network fees = net to the NGO**. No other deduction is allowed.

## Normative scope

**The standard defines (the wallet MUST comply):**

- The minimal user interface: dust detection, explicit consent, signature.
- The distribution formula: gross amount - network fees = net to NGO, with no commission.
- On-chain transparency requirements: full traceability of flows.
- Security requirements: non-custodial, opt-in, atomic operations.
- The initial whitelist of supported assets and networks.

**The standard leaves free (at the wallet's or user's discretion):**

- The user's choice of NGO or cause.
- The wallet's design and brand.
- The chosen integration mode (immediate or aggregated).
- The dust threshold, configurable by the user.
- The third-party services used for network-fee coverage.

## The six principles

1. **Non-custodial.** Funds MUST remain in the user's wallet until the explicit signature. No actor MUST hold the funds in custody at any point.
2. **Zero commission.** The wallet MUST NOT extract any economic value from the donation. An implementation taking a commission is not compliant.
3. **Strong opt-in.** Every donation MUST receive explicit consent. No hidden automation. A periodic mode MAY be offered; it MUST remain optional and revocable in a single gesture.
4. **On-chain transparency.** All transactions MUST be public and auditable on the blockchain explorer. No opaque intermediate step.
5. **Modularity.** The wallet MAY be a classic EOA, an ERC-4337 smart account, or an EOA temporarily made smart via EIP-7702.
6. **Minimalism.** The standard introduces no token, no managed funds and no new intermediary actor. It relies on the ecosystem's existing building blocks.

## Flow architecture

The typical donation flow has five steps, identical regardless of mode.

1. **Detection.** The wallet lists balances per token and per network, and identifies those below the user-configured threshold.
2. **Configuration.** The user selects which tokens to include, sets their dust threshold and chooses their sweep mode (manual or periodic).
3. **NGO choice.** The user selects an organization from a public, verifiable list, or enters a custom receiving address.
4. **Signature.** The user signs the operation. Depending on the wallet's configuration, network fees are covered in stablecoin, sponsored, or paid in the native token.
5. **Trace.** The transaction appears immediately on the explorer. A receipt is generated locally by the wallet.

> **To be defined (v0.x)**: the recommended default dust threshold. The value remains user-configurable in all cases; only a recommended default is left to set.

## Immediate mode

Default mode.

- **Principle.** The dust goes directly from the user's wallet to the NGO's wallet, in a single transaction. No aggregation, no waiting.
- **When to use it.** For the majority of cases: dust whose value exceeds the cost of a paymaster (typically above a few cents on L2).
- **Technical flow.** Wallet → (paymaster for network fees if needed) → NGO. One signature, one on-chain transaction.
- **Advantages.** Immediate donation, trivial audit, no waiting, no intermediate smart contract to deploy or audit.

## Aggregated mode

Optional mode. It is a refinement, not the core of the standard.

- **Principle.** Several dusts are gathered in an aggregation smart contract over a short period, then transferred to the NGO in a single outgoing transaction.
- **When to use it.** For truly microscopic dust (below the cost of a paymaster), illiquid tokens requiring a grouped swap, or simpler NGO-side accounting (one transaction instead of a hundred).
- **Technical flow.** Wallets → aggregation smart contract → NGO.
- **Non-custodial.** The smart contract MUST remain non-custodial: funds MUST stay recoverable by the user as long as the aggregation window is open.
- **Advantages.** Pooled network fees, better efficiency, simplified NGO accounting.

> **To be defined (v0.x)**: the aggregation window duration, the reference interface of the aggregation smart contract, its audit requirements, and the exact behavior for illiquid-token swaps.

## Wallet compatibility

The standard does not impose a single signature model. Three architectures are supported.

- **Classic EOA.** Private-key wallet. The user signs a standard transaction. Network fees are paid in the native token, unless covered by a third-party service.
- **Smart account (ERC-4337).** The user signs a UserOperation, processed by a bundler. A paymaster MAY sponsor the fees or have them paid in stablecoin.
- **Temporarily smart EOA (EIP-7702).** A classic wallet acting on a one-off basis as a smart account for a transaction. Combines the simplicity of the EOA with the flexibility of the smart account.

## Network fee coverage

The standard does not prescribe a single fee-payment mechanism. Depending on the wallet's architecture and configuration, network fees MAY be:

- paid in the network's native token by the user;
- sponsored by a paymaster;
- paid in stablecoin via a paymaster.

The goal is that the user does not necessarily need to hold the network's native token to make a donation. The choice of third-party service is left free to the wallet.

## Supported assets and networks

The standard provides for an initial whitelist of assets and networks. Its role is to frame the first implementations around proven assets and networks.

> **To be defined (v0.x)**: the exact composition of the initial whitelist (assets and networks), along with the criteria for additions and the update process. It will be settled publicly before the first reference implementation.

## Receipt and traceability

Every donation MUST be traceable on-chain: the transaction is public on the relevant network's explorer. In addition, the wallet SHOULD generate a receipt locally for the user.

> **To be defined (v0.x)**: the minimal content of the local receipt (required fields) and its format, to ensure consistency across wallets.

## Security

- **Non-custodial.** At no point does a third party hold the funds. See principle 1.
- **Opt-in.** No operation without explicit consent. See principle 3.
- **Atomicity.** Operations SHOULD be atomic: either the donation succeeds, or the user's state remains unchanged, with no exploitable intermediate state.

> **To be defined (v0.x)**: the precise atomicity requirements for aggregated mode and multi-network cases.

## NGO list

The user freely chooses the beneficiary, either from a public, verifiable list or via a custom address. The public list aims to reduce the risk of address error and to provide beneficiaries with verified addresses.

> **To be defined (v0.x)**: the governance of the public NGO list (who verifies, by which criteria, through what addition and removal process, and where it is hosted).

## Standard compliance

An implementation is compliant if it meets all of the following conditions:

- It takes no commission; only the network-fee cost is deducted from the gross amount.
- It never holds the funds in custody (non-custodial).
- It requires explicit consent for every donation, and any periodic mode is optional and revocable.
- It ensures full on-chain traceability, with no opaque intermediate step.
- It respects the initial whitelist of assets and networks.

A formal EIP is being drafted to propose this standard to the Ethereum ecosystem. It is not published at this stage.

## Status, versioning and contribution

This document is a draft (v0.1-draft), in Phase 0, meant to evolve in the open. Successive versions will be tracked in the repository's `CHANGELOG.md`.

Any improvement proposal or criticism is submitted via Issue or Pull Request on the [DustEthic/docs](https://github.com/DustEthic) repository.

---

*This document is published under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license. DustEthic v0.1-draft, Phase 0.*
