---
eip: <to be assigned>
title: Dust-to-Donation Wallet Standard
description: A wallet-level standard to convert residual token balances into direct, commission-free donations to NGOs.
author: TODO (name <email> or @githubhandle)
discussions-to: TODO (Ethereum Magicians thread URL)
status: Draft
type: Standards Track
category: ERC
created: 2026-06-11
---

> **Working draft - not yet submitted.** This is an early draft prepared in DustEthic's Phase 0. It is not published in the `ethereum/EIPs` repository and carries no assigned EIP number. Open points are marked **[To be defined]**.

## Abstract

This proposal defines a wallet-level standard, named DustEthic, that lets a crypto wallet turn a user's residual token balances ("dust") into a direct donation to a Non-Governmental Organization (NGO). The donation goes from the user's wallet to the NGO's address with no intermediary, no commission and no new token. The standard specifies a minimal in-wallet interface (detection, consent, signature), a distribution formula, on-chain transparency requirements and security requirements, while leaving the choice of NGO, the wallet design and the fee-coverage service free.

## Motivation

Across the ecosystem, a large share of wallets hold token balances too small to be moved profitably: network fees exceed their value. These balances are neither sellable, usable, nor easily directed to a cause. They remain idle.

Existing approaches that aggregate or route such balances tend to introduce an intermediary that takes a commission, hold funds in custody, or rely on a dedicated token. DustEthic avoids all three. It defines a shared convention that any wallet can implement on top of existing ecosystem primitives, so that residual balances can be donated directly, transparently and free of charge.

## Specification

The key words "MUST", "MUST NOT", "SHOULD", "SHOULD NOT" and "MAY" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

### Distribution formula

A compliant implementation MUST apply the formula:

No other deduction is permitted. An implementation that takes a commission MUST NOT claim compliance.

### Core principles

A compliant wallet:

1. MUST remain non-custodial: funds stay in the user's control until the explicit signature.
2. MUST NOT extract any commission from the donation.
3. MUST require explicit, per-donation consent; any periodic mode MUST be optional and revocable.
4. MUST ensure full on-chain traceability, with no opaque intermediate step.
5. MAY be implemented on a classic EOA, an ERC-4337 smart account, or an EOA temporarily acting as a smart account via EIP-7702.
6. MUST NOT introduce a token, managed funds, or a new intermediary actor.

### Donation flow

The flow has five steps, identical regardless of mode: detection, configuration, NGO choice, signature, trace. A receipt SHOULD be generated locally by the wallet in addition to the on-chain trace.

### Integration modes

- **Immediate mode (default).** The dust is transferred directly from the user's wallet to the NGO's wallet in a single transaction.
- **Aggregated mode (optional).** Dust is gathered in a non-custodial aggregation contract over a short window, then transferred to the NGO in a single outgoing transaction. Funds MUST remain recoverable by the user while the window is open.

### Open specification points

- **[To be defined]** Recommended default dust threshold.
- **[To be defined]** Aggregation window duration and the reference interface of the aggregation contract.
- **[To be defined]** Composition of the initial asset and network whitelist, and its update process.
- **[To be defined]** Minimal required fields and format of the local receipt.
- **[To be defined]** Precise atomicity requirements for aggregated and multi-network cases.
- **[To be defined]** Governance of the public NGO list.

## Rationale

The design keeps the standard minimal on purpose: it relies on existing primitives (EOA transactions, ERC-4337, EIP-7702, paymasters) rather than introducing new on-chain infrastructure. Mandating zero commission and non-custodial behavior is what differentiates a donation convention from a routing service and is therefore expressed as a hard compliance requirement rather than a recommendation.

## Backwards Compatibility

This proposal introduces no change to existing consensus or token behavior. It defines wallet-level behavior on top of existing transaction and account-abstraction mechanisms, and is therefore additive. **[To be defined]** explicit interaction notes with ERC-4337 and EIP-7702 flows.

## Reference Implementation

**[To be defined]** A reference implementation will be provided once the open specification points are settled. See the [DustEthic Wallets Whitepaper](../livre-blanc-wallets/livre-blanc-wallets-en.md) for the current technical detail.

## Security Considerations

Donations are irreversible once signed and settled on-chain. Implementations MUST surface the destination address clearly before signature and SHOULD reduce address-entry error by offering a verified public list. The aggregation contract, when used, MUST be non-custodial and SHOULD be independently audited before use. **[To be defined]** detailed threat model for the aggregation contract and multi-network operations.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
