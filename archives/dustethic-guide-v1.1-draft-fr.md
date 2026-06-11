# DUSTETHIC - THE COMPLETE GUIDE [2025-11-13]
> French version: [Le guide complet](./dustethic-guide-v1.1-draft-fr.md)

**Version**: 1.1-draft  
**Date**: 2025-11-13  
**Last updated**: [2025-11-13]  
**Status**: Phase 0 - Scoping document

> [!WARNING]
> Phase 0 - Scoping document  
> - Informational document - not financial or legal advice.  
> - Some capabilities depend on ERC-4337, paymasters and L2 usage.  
> - Reference amounts are expressed in crypto units. Fiat equivalents are for readability only.  
> - Transparency required: fee schedule (published tier), aggregation windows, supported networks, chosen gas option and **campaign cap** must be disclosed publicly.  
> - **Campaign cap**: gas + fee + technical reserve ≤ public threshold (e.g. 15%).

---

## 🎯 Core principle

**We reason in crypto units, not in fiat.**  
Amounts are counted in native units of the chain in use. Example: you donate 0.0100 ETH, the NGO receives 0.0090 ETH if the announced fee is 10%. This logic **neutralises volatility in the split** between actors. The **fiat value remains floating** until each party converts its share.  
In this guide, fiat equivalents are provided for readability only.

### 🔌 Gas policy v0.2 - default

- **L2-first**: operations are prioritised on low-fee L2s (e.g. Optimism, Arbitrum) so that gas cost stays marginal.  
- **Conditional execution**: only execute when the **`Aggregated donations / estimated gas` ratio** reaches a favourable threshold **T**.  
  - DustEthic v0.2 recommendation: `T ≥ 30` by default (≈ 3% gas max before fees).  
  - Example with a 7% fee: `T ≈ 33` keeps ≈ 90% net for the NGO.  
- **Relayer gas pool**: the relayer maintains a pool of the native gas token (e.g. ETH on L2 EVM). **No conversion is made on donations** to fund the fee.  
- **Optional safety net**: if the gas pool is insufficient, a **minimal, on-chain documented conversion** may be triggered to buy gas token, without changing the distribution formula.  
- **Standard display**:  
  - Calculation formula: `NGO net = Aggregated amount - reimbursed gas - relayer fee - technical reserve`  
  - Fee displayed **as a percentage of the donated crypto** (e.g. 7% in ETH if the donation is in ETH)  
- **Transparency**: chosen gas option (gas pool, L2-first, safety net) and **campaign cap** (e.g. 15%) are **disclosed publicly** by each relayer.

---

## 📚 Table of contents

- [Core principle](#-core-principle)  
- [Gas policy v0.2 - default](#-gas-policy-v02---default)  
- [1) The real problem today](#1-the-real-problem-today)  
- [2) The solution proposed by DustEthic](#2-the-solution-proposed-by-dustethic)  
- [3) Realistic operational flow](#3-realistic-operational-flow)  
- [4) Gas, conversions and design options](#4-gas-conversions-and-design-options)  
- [5) Volatility - principles and strategies](#5-volatility---principles-and-strategies)  
- [6) Actors and responsibilities](#6-actors-and-responsibilities)  
- [7) Donors - how it works](#7-donors---how-it-works)  
- [8) NGOs - integration, accounting, compliance](#8-ngos---integration-accounting-compliance)  
- [9) Relayers - minimum requirements for DustEthic v0.1](#9-relayers---minimum-requirements-for-dustethic-v01)  
- [10) Market references and positioning](#10-market-references-and-positioning)  
- [11) Roadmap](#11-roadmap)  
- [12) Join the project](#12-join-the-project)  
- [13) Licence](#13-licence)  
- [14) Notes and references](#14-notes-and-references)

---

## 1) The real problem today

- On Ethereum L1, **gas fees are paid in ETH** and can exceed small donations. On L2 they are much lower, but never zero.  
- Several EVM L2s also use **ETH as gas token** (e.g. Arbitrum, Optimism). Polygon PoS uses **POL** following the MATIC→POL migration.  
- As a result, an isolated micro-donation is often inefficient on L1, sometimes acceptable on L2, and depends heavily on network conditions.

---

## 2) The solution proposed by DustEthic

**Aggregation + on-chain transparency + splitting in crypto units**:

- **Relayers** aggregate micro-donations over a limited period, then perform **one grouped payout** to the NGO.  
- Splitting happens **in crypto units**, with **gas reimbursed first** and a published **degressive fee schedule**.  
- A **campaign cap** is published: gas + fee + technical reserve ≤ public threshold (e.g. 15%).  
- The NGO share and the fee are **expressed as a percentage of the donated crypto**, not a fiat equivalent.  
- Traceability is ensured via public explorers (e.g. Etherscan for Ethereum).

**Building blocks already exist**:

- **Account Abstraction ERC-4337** with **paymasters** to sponsor donor gas.  
- **EIP-2612 permit**, when available, for approvals by signature without an on-chain `approve` transaction.

---

## 3) Realistic operational flow

**Step 1 - Donation**  
- Donation via AA smart account with paymaster: **gas sponsored**, donor does not pay directly.  
- Donation via EOA + token with permit: **approval without gas**, then relayed donation.  
- Donation via EOA + token without permit: a **paid approval** may be required, depending on the token.

**Step 2 - Aggregation**  
- Donations are collected into an aggregation smart contract. Recommended triggers: amount threshold, maximum time window, acceptable gas window.

**Step 3 - Grouped payout**  
- A single transaction sends the funds to the NGO.  
- Standardised formula:  
  - `NGO net = Aggregated amount - reimbursed gas - relayer fee - technical reserve`

**Step 4 - Public breakdown**  
- Donations, any conversions and the final payout can be inspected on the chain explorer.

---

## 4) Gas, conversions and design options

**Physical constraints**: on EVM, gas is paid in the **native token** of the chain. Examples: ETH on Ethereum, Optimism, Arbitrum; **POL** on Polygon PoS.  
To respect the principle “no donation conversion for the fee”, DustEthic **v0.2** proposes explicit **gas funding options**:

- **Option A - Relayer gas pool**: the relayer maintains a pool of the required gas token. No conversion on donations.  
- **Option B - Minimal documented conversion**: pro-rata skim in kind to buy gas token, logged on-chain, without affecting the split formula beyond gas cost.  
- **Option C - L2-first**: operate primarily on low-fee L2s so gas is marginal.  
- **Option D - Sponsors**: paymasters funded by partners who supply gas and are reimbursed periodically.

---

## 5) Volatility - principles and strategies

**Rule**: splitting happens in crypto units. Percentages stay constant; fiat value moves until the NGO and relayer convert.

**Stablecoins**: reduce volatility but **do not remove risk** (depeg, address freezes, issuer risk).

**Post-receipt strategies**:  
- **NGO**: sell immediately, sell partially, or hold according to internal policy and risk tolerance.  
- **Relayer**: regular selling, holding, or a mixed approach.

---

## 6) Actors and responsibilities

- **Donors**: send small amounts, ideally via AA so they don’t pay gas directly.  
- **Relayers**: operate aggregation, publish public parameters, comply with v0.1 and keep **signed logs** of sensitive operations (with on-chain links).  
- **NGOs**: receive funds directly into their wallet, define a conversion policy and basic compliance procedures.

---

## 7) Donors - how it works

1) Choose a DustEthic-compliant relayer.  
2) Connect your wallet.  
3) Select the NGO.  
4) Enter the amount in **crypto**.  
5) Sign. Depending on the setup, the donation is gasless via AA or permit. Otherwise a paid approval may be required depending on the token.

**Cost for the donor**: ideally zero via AA or permit. Otherwise, only the initial approval if required by the token. Gas for the final grouped payout is **mutualised** and deducted before the NGO payout.

**Tracking**: each donation and the final payout are visible on the chain explorer.

---

## 8) NGOs - integration, accounting, compliance

**Recommended wallet**: **Safe** (ex-Gnosis Safe) with multi-signature for custody.  
**Fiat conversion**: via a registered exchange, according to internal policies.  
**Accounting**: book the value at reception time, define a conversion policy, track addresses.  
**Minimal compliance**: even in a non-custodial setup, adopt **address screening** and a written policy. References: **OFAC** and **FATF R.15 / Travel Rule**. Exact obligations depend on jurisdiction and status.

---

## 9) Relayers - minimum requirements for DustEthic v0.1

**Transparency**  
- Open-source code. Public parameters: **degressive fee schedule** (suggested max 15%), **campaign cap**, aggregation windows, supported networks, chosen gas option.  
- **Signed logs** and on-chain links; **CSV** export.  
- Readable on-chain dashboard.

**Non-custodial**  
- Funds held by smart contracts. Technical governance without unilateral withdrawal power.

**Technical governance**  
- Admin roles under **Safe multi-sig**. Timelock on critical changes. Emergency procedures.

**Security**  
- Independent audit before mainnet. Bug bounty after launch.

**Gas and conversions**  
- Explicitly choose Option A, B, C or D and display it publicly.  
- **Execute only if the ratio `Aggregated donations / estimated gas ≥ T`** (T ≥ 30 recommended); **gas reimbursed first**.  
- Fee always expressed as a percentage of the donated crypto (published **degressive** schedule).  
- If gas conversion is necessary, log it on-chain.

**AA and compatibility**  
- ERC-4337 support and paymaster recommended on L2. The published EntryPoint is the implementation reference.  
- Fallback: **EIP-2771** meta-transactions where 4337/7702 (AA) is not supported.

**v1 asset whitelist**  
- Ethereum & EVM L2s: ETH, USDC, USDT.  
- Polygon PoS: possible, but **gas in POL** - plan logistics accordingly.  
- By default reject illiquid, taxed, honeypot tokens or those without permit support if UX becomes impractical.

**Minimal compliance**  
- Proportionate AML policy, basic screening, logging of refusals.

---

## 10) Market references and positioning

- “Classic” crypto donation platforms exist and mainly target medium or large donations with fast fiat conversion.  
  - **Every.org**: instant conversion to USD, 1% broker fee + network fees.  
  - **The Giving Block**: subscription packages and processing fees, details provided commercially.  
- **DustEthic** focuses on **micro-donations via aggregation**, **native crypto splitting** on low-fee L2s, with a published **campaign cap** and **degressive fee schedule**.

---

## 11) Roadmap

**Phase 0 - Foundations [Q4 2025]**  
- v0.1 standard specification  
- v1 asset and network whitelist  
- Aggregation and paymaster contract design - testnet  
- Security and governance policy

**Phase 1 - Development [2026]**  
- Open-source reference implementation  
- Sepolia and matching L2 testing  
- Third-party audit  
- Pilots with 1 relayer and 2 NGOs

**Phase 2 - Launch [2026+]**  
- Mainnet + 2 L2 deployments  
- 3–5 compliant relayers, 10+ NGOs  
- Community dashboard

**Phase 3 - Expansion [2027+]**  
- More L2s, possibly other EVM ecosystems  
- Broader governance if traction

---

## 12) Join the project

- **Developers**: smart accounts, paymasters, aggregators. ERC-4337 and EntryPoint are good starting points.  
- **NGOs**: test with a **Safe** wallet and an internal conversion policy.  
- **Relayers**: run an L2-first implementation and publicly document gas metrics and delays.  
- **Community**: feedback, translations, content.

Useful links:  
- Website: https://dustethic.org  
- Discord: https://discord.gg/fVFc26GV  
- GitHub: https://github.com/DustEthic  
- Bluesky: @dustethic.bsky.social

---

## 13) Licence

- Text: **CC BY 4.0**  
- Future code: **MIT**

---

## 14) Notes and references

(See French version for the same reference list. The URLs are identical.)

---

**End of DustEthic Guide v1.1-draft**
