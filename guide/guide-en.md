# DustEthic Guide

**Status: Phase 0 - design** · **Version: v0.1-draft** · **Date: 2026-06-11** · **License: CC BY 4.0**

> Entry point for understanding DustEthic as a whole: the problem, the model, the actors and the path of a donation. The detailed technical specification lives in the [Wallets Whitepaper](../livre-blanc-wallets/livre-blanc-wallets-en.md).

## Contents

- [DustEthic in one sentence](#dustethic-in-one-sentence)
- [The dust problem](#the-dust-problem)
- [A standard, not a platform](#a-standard-not-a-platform)
- [The actors](#the-actors)
- [A donation, step by step](#a-donation-step-by-step)
- [The two modes](#the-two-modes)
- [Scope of the standard](#scope-of-the-standard)
- [The six principles](#the-six-principles)
- [Wallet compatibility](#wallet-compatibility)
- [Project status](#project-status)
- [Going further](#going-further)

## DustEthic in one sentence

DustEthic is an open specification describing how a crypto wallet can turn a user's residual balances into a direct donation to an NGO, with no intermediary, no commission and no token.

It is not a service, a company or a fundraiser. It is a public technical convention that any wallet can implement.

## The dust problem

Open your crypto wallet and count the leftovers: a few cents of a stablecoin forgotten on one network, fractions of a token stuck on another, old airdrop tokens that have become illiquid. Individually these amounts are worth almost nothing. You can neither sell them (network fees exceed their value), nor use them, nor simply sweep them to a cause.

This "dust" sits idle and clutters. It is not an isolated case: it is the situation across a large share of crypto wallets. At the scale of the ecosystem, it represents a considerable amount of fractional value that is currently unusable.

## A standard, not a platform

DustEthic does not collect donations and never holds funds. The project defines a shared set of rules, then leaves each actor autonomous.

In practice, this means no intermediary stands between the donor and the NGO. The donation goes from the user's wallet to the NGO's address. DustEthic describes how that operation should unfold so that it is transparent, safe and free, nothing more.

## The actors

The model involves only three roles, with no new intermediary.

**The user (the donor)** owns the dust. They choose the NGO, configure their preferences and sign the operation. Their funds stay in their wallet until the explicit signature.

**The wallet** is the central actor. It is the one that implements the standard: it detects the dust, presents the interface, prepares the operation and sends it after signature. It never holds the funds in custody.

**The NGO (the beneficiary)** publishes verifiable receiving addresses and receives donations directly, with no commission deducted.

## A donation, step by step

The typical flow has five steps, identical regardless of the chosen mode.

1. **Detection.** The wallet lists balances per token and per network, and identifies those below a threshold configured by the user (for example: less than 2 USD per token).
2. **Configuration.** The user selects which tokens to include, sets their dust threshold and chooses their sweep mode (manual or periodic).
3. **NGO choice.** The user selects an organization from a public, verifiable list, or enters a custom receiving address.
4. **Signature.** The user signs the operation in their wallet, like a normal transaction. Depending on the wallet's configuration and the third-party services used, network fees are covered in stablecoin, sponsored, or paid in the native token. The user does not necessarily need to hold the network's native token.
5. **Trace.** The transaction appears immediately on the blockchain explorer. A receipt is generated locally by the wallet.

The distribution formula is deliberately simple: **gross amount - network fees = net to the NGO**. DustEthic adds no commission. An implementation that took a commission would not be compliant with the standard.

## The two modes

The standard provides two integration modes, at the wallet's discretion.

**Immediate mode (default).** Each dust goes directly from the user's wallet to the NGO's wallet, in a single transaction, with no waiting and no aggregation. This is the mode suited to the majority of cases, thanks to recent advances in wallets and in network-fee coverage services.

**Aggregated mode (optional).** For truly microscopic dust or illiquid tokens, the wallet may use an aggregation smart contract that groups donations over a short period before a single transfer to the NGO. This smart contract is non-custodial: funds remain recoverable by the user as long as the aggregation window is open. It is a refinement, not the core of the standard.

## Scope of the standard

DustEthic describes how a donation is made, not for whom or with whom. This strict separation protects each actor's autonomy.

**What the standard defines:**

- The minimal in-wallet user interface (detection, consent, signature).
- The fund distribution formula (gross amount - network fees = net to NGO).
- On-chain transparency requirements (full traceability of flows).
- Security requirements (non-custodial, opt-in, atomic operations).
- The initial whitelist of supported assets and networks.

**What the standard leaves free:**

- The user's choice of NGO or cause.
- The design and brand of the implementing wallet.
- The chosen integration mode (immediate or aggregated).
- The dust threshold, configurable by the user.
- The third-party services used for network-fee coverage.

## The six principles

Six principles are non-negotiable. An implementation that breaks any one of them is not compliant.

1. **Non-custodial.** Funds stay in the user's wallet until the explicit signature. No actor holds the funds in custody at any point.
2. **Zero commission.** DustEthic extracts no economic value.
3. **Strong opt-in.** Every donation requires explicit consent. No hidden automation. The periodic mode stays optional and revocable in one click.
4. **On-chain transparency.** All transactions are public and auditable on the blockchain explorer. No opaque intermediate step.
5. **Modularity.** The standard is compatible with classic EOA wallets, ERC-4337 smart accounts, and EOAs temporarily made smart via EIP-7702.
6. **Minimalism.** Zero token, zero managed funds, zero new intermediary actor. The standard relies solely on the ecosystem's existing building blocks.

## Wallet compatibility

The standard does not impose a single signature model. Three wallet architectures are supported.

- **Classic EOA.** Traditional private-key wallet. The user signs a standard transaction. Network fees are paid in the native token, unless a third-party service covers them.
- **Smart account (ERC-4337).** Smart-contract-based wallet. The user signs a UserOperation, processed by a bundler. A paymaster may sponsor the fees or have them paid in stablecoin.
- **Temporarily smart EOA (EIP-7702).** A classic wallet acting on a one-off basis as a smart account for a transaction. Combines the simplicity of the EOA with the flexibility of the smart account.

## Project status

DustEthic is in Phase 0: a design phase, volunteer-run and non-profit. There is no token, no fundraising and no commercial product.

The project applies radical transparency: the specification is public on GitHub, in French and English, and any improvement proposal or criticism can be submitted via Issue or Pull Request. The standard is a draft (v0.1-draft), incomplete and meant to evolve in the open.

## Going further

- **[Wallets Whitepaper](../livre-blanc-wallets/livre-blanc-wallets-en.md)**: the detailed technical specification, the recommended entry point for teams building a wallet.
- **Official site**: https://dustethic.org
- **Community**: [GitHub](https://github.com/DustEthic) · [Discord](https://discord.gg/fVFc26GV) · [Bluesky](https://bsky.app/profile/dustethic.bsky.social)

---

*This document is published under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license. DustEthic v0.1-draft, Phase 0.*
