# [2025-12-10] - DustEthic Whitepaper - Relayers

> Working draft - conceptual only. This is not legal or financial advice. DustEthic is, at this stage, a design proposal, not a live system.

---

## 1. Executive summary

This section of the whitepaper describes the role of relayers in the DustEthic ecosystem, the commission models being considered, technical integration options, and some high-level examples of potential revenues.

DustEthic relayers are inspired by existing building blocks of Account Abstraction on Ethereum and L2s (ERC-4337, EntryPoint, UserOperations, bundlers, paymasters), and by ongoing work around EIP-7702 which allows externally owned accounts to temporarily benefit from smart-account-like functionality.

Main goals:
- Make dust donations simple for end users.
- Optimize gas costs through specialized relayers.
- Offer NGOs traceable and auditable donation flows.
- Provide a viable business model for operators, without speculative tokens or yield promises.

---

## 2. Context and the role of DustEthic relayers

### 2.1 The “dust” problem

“Dust” refers to small residual balances, typically too low to be moved economically because of gas fees or withdrawal fees. DustEthic’s idea is to transform these dormant amounts into aggregated micro-donations for partner NGOs.

Key challenge: the fixed cost of an on-chain transaction remains significant, even for very small amounts. This makes aggregation and gas optimization essential.

### 2.2 Specific role of a DustEthic relayer

A DustEthic relayer is a technical operator that:
- Receives donation intents from DustEthic-compatible wallets.
- Aggregates these intents (bundling) to reduce the average cost per donation.
- Submits transactions on-chain, paying gas on behalf of users (directly or via a paymaster).
- Gets reimbursed for gas and commission through a DustEthic contract and the donation flows.
- Publishes verifiable execution proofs for donors and NGOs.

In the proposed architecture, the relayer is a replaceable component. Multiple relayers can coexist and compete (fees, SLAs, supported networks, etc.).

---

## 3. Proposed high-level architecture

### 3.1 Technical inspirations

The DustEthic relayer architecture is inspired by:
- ERC-4337: UserOperations, EntryPoint, bundlers and paymasters to implement Account Abstraction on top of the base protocol.
- EIP-7702: “setCode”-style transactions allowing an EOA to temporarily behave like a smart account during a transaction.
- Existing AA relayer infrastructures (Gelato, Biconomy, etc.), which already provide gasless and paymaster services on various networks.

DustEthic does not reinvent these building blocks. It proposes a donation-oriented standardization layer on top.

### 3.2 Conceptual flow of a donation via a relayer

Simplified scenario:
1. The user opens a DustEthic-compatible wallet and sees their eligible dust balances (by network and token).
2. They configure a donation (for example: “donate all my USDC dust on this L2 to NGO X”).
3. The wallet constructs:
   - A UserOperation or meta-transaction containing:
     - The dust transfer instructions.
     - DustEthic metadata (NGO, campaign, tags).
4. The DustEthic relayer:
   - Receives and validates the request.
   - Batches multiple operations into a bundle when it makes economic sense.
   - Submits everything through a DustEthic contract (which can leverage the EntryPoint if ERC-4337 is used).
5. The DustEthic contract:
   - Moves funds from donors’ accounts to a DustEthic collection address dedicated to the NGO.
   - Logs the data needed for transparency (events, logs, campaign identifiers).
6. Periodically or after a threshold is reached, the contract forwards the aggregated funds to the NGO (an address controlled by the NGO itself).

### 3.3 Software layers

A DustEthic relayer can be broken down into several layers:
- Layer 1 - “AA / Relay”: ERC-4337 bundler and paymaster, or equivalent, possibly operated by a third-party provider.
- Layer 2 - “DustEthic Core”: dust-donation logic (contracts, aggregation rules, limits, logs).
- Layer 3 - “API and wallet / NGO integration”: wallet endpoints, NGO dashboards and exports, monitoring.

---

## 4. Considered commission models

### 4.1 Variables and notation

For a given period (for example one month):
- V: total donation volume processed by a relayer (in USD equivalent).
- c: global commission rate (for example 0.5 % to 3 % of volume).
- G: total gas costs paid by the relayer over the period.
- C_infra: infrastructure and operational costs (servers, audits, support).
- R_net: relayer’s net revenue.

Generic formula:
R_net = V × c - G - C_infra

The models below mainly differ in “who pays c” and how it is displayed.

### 4.2 Model 1 - Donor-side commission

Principle:
- For each donation, a percentage of the amount is taken to remunerate the relayer (and optionally a share for DustEthic Core).

Example (conceptual only):
- c = 1 % of the donated amount.
- The user sees something like:
  - “You donate 1.00, of which 0.99 goes to the NGO and 0.01 to DustEthic infrastructure.”

Pros:
- Direct alignment between donation volume and revenue.
- Simple to understand.

Cons:
- Some donors may be sensitive to “fees on donations”.
- Requires very clear and honest communication.

### 4.3 Model 2 - NGO-side commission

Principle:
- The NGO accepts that each DustEthic donation is credited net of fees.
- The user sees “You donate 1.00 to NGO X”, and the NGO receives for example 0.99.

Pros:
- Very simple donor experience.
- Similar to many existing PSP and donation-platform models.

Cons:
- NGOs care about their global fee level.
- Must be justified by incremental donation flows (dust that would otherwise be lost).

### 4.4 Model 3 - Split commission

Principle:
- Commission is shared between donor and NGO.
- Example:
  - 0.5 % on the donor side.
  - 0.5 % on the NGO side.

Pros:
- More balanced cost sharing.
- Can reduce perceived fees on both sides.

Cons:
- More complex to explain.
- May require careful UX to avoid confusion.

### 4.5 Model 4 - B2B subscription for wallets and platforms

Principle:
- Wallets or platforms pay a subscription (or buy “credits”) from a DustEthic relayer for an included transaction volume.
- Costs are handled at a B2B level, possibly without showing explicit fees to the donor.

Pros:
- Aligned with SaaS-like models used by some AA providers.
- Predictable costs for wallet partners.

Cons:
- Requires specific commercial agreements.
- Might limit DustEthic to actors with sufficient scale.

### 4.6 Possible combinations

A single DustEthic ecosystem can allow multiple models to coexist, for example:
- Default: NGO-side commission with an upper cap.
- For some premium wallets: B2B coverage of fees.
- For specific campaigns: reduced or zero commission (sponsored by a third party).

---

## 5. Potential gains for a DustEthic relayer

These scenarios are purely illustrative and do not constitute revenue promises.

### 5.1 Example assumptions

Assume:
- Scenario A - low volume: V = 50,000 USD per month.
- Scenario B - mid volume: V = 500,000 USD per month.
- Scenario C - high volume: V = 5,000,000 USD per month.
- Commission rate: c = 1 %.
- Costs (gas + infra) represent 30 % of gross commission.

Then:
- Gross commission = V × c.
- R_net ≈ 70 % of V × c.

### 5.2 Simplified calculations

- Scenario A:
  - Gross commission: 50,000 × 1 % = 500.
  - R_net ≈ 350.
- Scenario B:
  - Gross commission: 500,000 × 1 % = 5,000.
  - R_net ≈ 3,500.
- Scenario C:
  - Gross commission: 5,000,000 × 1 % = 50,000.
  - R_net ≈ 35,000.

Critical reading:
- Below a certain volume, a standalone relayer may struggle to fund audits, support and robust infrastructure.
- Beyond a certain volume, a specialized relayer or an already established AA provider can find a meaningful business opportunity.

### 5.3 Gas-cost sensitivity

Key factors:
- Target networks (cheap L2s vs congested L1).
- Aggregation strategy (minimum thresholds per bundle).
- Throttling policy to avoid processing single tiny donations one by one.

Conclusion: in practice, DustEthic relayers will likely want to prioritize L2s for most dust flows.

---

## 6. Integrating relayers into the DustEthic ecosystem

### 6.1 Wallet-side integration

A DustEthic-compatible wallet should ideally:
- Clearly display:
  - Detected, eligible dust.
  - The applicable fee model (NGO-side, donor-side, split, B2B).
- Use a DustEthic SDK or metadata schema to:
  - Build donation operations.
  - Reference the chosen relayer(s).
  - Retrieve donation-tracking information (hash, status, NGO, campaign).

Important option:
- Either let the user select the relayer, or at least make the chosen relayer visible (for example “DustEthic Relayer A, L2 X, 1 % commission”).

### 6.2 NGO-side integration

For NGOs, a DustEthic relayer should offer:
- A dashboard:
  - Received amounts by campaign, token, network.
  - List of transactions with on-chain hashes.
- Exports (CSV, API) to internal accounting or CRM tools.
- Cash-out options:
  - Keeping funds in crypto.
  - Converting to fiat via partners (subject to regulatory and KYC/AML constraints).

### 6.3 Integration with existing AA providers

A DustEthic relayer can be:
- Operated directly by an existing AA provider (Gelato, Biconomy, etc.).
- Or a DustEthic layer on top of various providers, with:
  - Multi-network routing.
  - Normalized donation logs.
  - Unified UX for wallets and NGOs.

In all cases, the “DustEthic - Relayers Standard” should define minimal requirements:
- Metadata format.
- On-chain and off-chain events.
- Transparency around fees and partners.

---

## 7. Governance, transparency and security

### 7.1 Public contracts and standards

To reduce risk:
- DustEthic contracts used by relayers should be open source.
- The “DustEthic - Relayers Standard” should be publicly available and versioned.

Goal: allow auditors, NGOs and communities to inspect the code and follow its evolution.

### 7.2 Proof of correct execution

Expected:
- Each donation flow must be traceable:
  - Transaction hash.
  - Amount, token, network.
  - Relayer used.
  - Beneficiary NGO and optional campaign.
- Relayers should expose:
  - Public endpoints to check a donation based on a DustEthic identifier.
  - Aggregated reports for NGOs and, to some extent, for the public.

### 7.3 Key management and operational risk

Main risks:
- Operator key compromise.
- Misconfigured gas limits.
- Griefing attacks against poorly protected paymasters.

Possible mitigations:
- Use smart accounts, multi-sig or social-recovery mechanisms for operator accounts.
- Enforce per-operation, per-campaign and per-period limits.
- Implement emergency pause mechanisms for abnormal behavior.

---

## 8. Caveats and critical analysis

In an “devil’s advocate” spirit, several aspects require careful attention.

### 8.1 Economic viability

- The model heavily depends on global aggregated volume.
- There is a risk of a long “cold start” period if wallet and NGO adoption is slow.
- Pressure to keep fees low may make it hard for relayers to break even.

### 8.2 Regulatory constraints

- Depending on the jurisdiction, aggregating and forwarding funds to NGOs may be seen as:
  - A payment service.
  - A financial-intermediation activity.
- KYC/AML requirements can become significant, especially when fiat conversions are involved.

Country- or region-specific legal analysis will be required before any production deployment.

### 8.3 Transparency vs privacy

- Maximum transparency is positive for trust, but:
  - It may reveal sensitive patterns (for example donations to NGOs in high-risk countries).
  - It may require anonymization or pseudonymization mechanisms in some cases.

### 8.4 Fast evolution of AA and L2s

- The ERC-4337 / EIP-7702 / L2 / relayer ecosystem is evolving very quickly.
- An overly rigid DustEthic standard risks becoming obsolete.
- Conversely, a too vague standard would be hard to implement correctly.

Conclusion: standard governance must accept regular updates, with strong focus on backward compatibility.

---

## 9. Proposed roadmap for DustEthic relayers

Indicative proposal (not binding):

1. Phase 0 - Internal prototype  
   - 1 reference relayer.  
   - 1 or 2 supported L2s.  
   - 1 or 2 pilot NGOs.  
   - Minimal UX to test end-to-end (test wallet, basic dashboard).

2. Phase 1 - Standardization  
   - Public drafting of “DustEthic - Relayers Standard v1”.  
   - Publication on GitHub (EN/FR).  
   - Definition of an initial SDK or integration schema for wallets.

3. Phase 2 - Multi-relayer ecosystem  
   - Opening to multiple independent operators.  
   - Definition of listing criteria (technical, ethical, SLAs).  
   - Wallet-side ability to select or switch relayers.

4. Phase 3 - Relayer marketplace and broader governance  
   - Public DustEthic relayer marketplace (fees, networks, track record).  
   - NGO and wallet participation in standard governance (structure to be defined).

---

## 10. References (for this relayer section)

These external sources do not define DustEthic but describe the technical building blocks that inspire this design.

1. ERC-4337 - Account Abstraction: UserOperation, EntryPoint, bundlers, paymasters - official specification and technical overviews.  
2. OpenZeppelin “Account Abstraction” documentation - explanation of ERC-4337 key elements and validation / execution logic.  
3. ERC-4337 overviews (for example Etherscan, Stackup) - alt-mempool concept, operational flow and UX implications.  
4. Articles and guides on paymasters and gasless transactions (Alchemy, GoldRush, Biconomy, etc.).  
5. Articles and documentation on EIP-7702 and the Pectra upgrade (Consensys, Alchemy, Circle, QuickNode, etc.).  
6. Biconomy docs on Account Abstraction, smart accounts and paymasters, including gasless-transaction tutorials.  
7. Gelato Relay documentation and blog posts - relayer infrastructure for gasless transactions and multi-network paymasters.  
8. General blog posts and tutorials about gasless transactions, meta-transactions and AA provider business models.  
