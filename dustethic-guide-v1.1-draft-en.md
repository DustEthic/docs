# DUSTETHIC - THE COMPLETE GUIDE [2025-11-06]
> Version française: [Le Guide Complet](./dustethic-guide-v1.1-draft-fr.md)

**Version**: 1.1-draft  
**Date**: 2025-11-06  
**Last updated**: [2025-11-06]  
**Status**: Phase 0 - Scoping document

> [!WARNING]
> Phase 0 - Scoping document  
> - Informational document - not financial or legal advice.  
> - Some capabilities depend on ERC-4337, paymasters and L2 usage.  
> - Reference amounts are in crypto units. Fiat equivalents are only indicative.  
> - Transparency required: commission (published degressive schedule), aggregation windows, supported networks, chosen gas option and **campaign cap** must be publicly displayed.  
> - **Campaign cap**: gas + commission + technical reserve ≤ public threshold (e.g., 15%).

---

## 🎯 Core principle

**We reason in crypto, not in fiat.**  
Amounts are counted in native units of the chosen chain. Example: you donate 0.0100 ETH, the NGO receives 0.0090 ETH if the announced commission is 10%. This logic **neutralizes volatility in the split** between participants. The **fiat value remains fluctuating** until each party converts.  
In this guide, euro equivalents exist only for readability.

### 🔌 Gas policy v0.2 - default

- **L2-first**: operations prioritized on low-fee L2s (e.g., Optimism, Arbitrum) so gas cost is marginal.  
- **Conditional execution**: execute only when the **donation-to-fee ratio** turns green.  
- **Relayer gas pool**: the relayer maintains a pool of the native gas token (e.g., ETH on L2 EVM). **No conversion is taken from donations** to fund the commission.  
- **Optional safety net**: if the gas pool is insufficient, a **minimal, on-chain documented conversion** may buy the gas token, without changing the split formula.  
- **Standard display**:  
  - Calculation formula: `NGO net = Aggregated amount - reimbursed gas - relayer commission - technical reserve`  
  - Commission is displayed **as a percentage of the donated crypto** (e.g., 7% in ETH if donation is in ETH)  
- **Transparency**: the chosen gas option (pool, L2-first, safety net) and the **campaign cap** (e.g., 15%) are **publicly declared** by each relayer.

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
- [7) Donors - how to](#7-donors---how-to)  
- [8) NGOs - integration, accounting, compliance](#8-ngos---integration-accounting-compliance)  
- [9) Relayers - minimum requirements DustEthic v0.1](#9-relayers---minimum-requirements-dustethic-v01)  
- [10) Market references and positioning](#10-market-references-and-positioning)  
- [11) Roadmap](#11-roadmap)  
- [12) Join the project](#12-join-the-project)  
- [13) License](#13-license)  
- [14) Notes and references](#14-notes-and-references)

---

## 1) The real problem today

- On Ethereum L1, **gas is paid in ETH** and can exceed small donations. On L2 it is much lower but never zero.  
- Several EVM L2s also use **ETH as gas token** (e.g., Arbitrum, Optimism). Polygon PoS uses **POL** after the MATIC→POL migration.  
- Consequence: an isolated micro-donation is often inefficient on L1, sometimes acceptable on L2, and depends on network conditions.

---

## 2) The solution proposed by DustEthic

**Aggregation + on-chain transparency + split in crypto units**:

- **Relayers** aggregate micro-donations during a limited period, then perform **one grouped transfer** to the NGO.  
- Split **in crypto units**, with **gas reimbursed first** and **published degressive commission**.  
- Publication of a **campaign cap**: gas + commission + technical reserve ≤ public threshold (e.g., 15%).  
- NGO share and commission are **expressed as a percentage of the donated crypto**, not as a fiat equivalent.  
- Traceability is via public explorers (e.g., Etherscan for Ethereum).

**Existing technical bricks**:

- **Account Abstraction ERC-4337** with **paymasters** to sponsor donor-side gas.  
- **EIP-2612 permit** where available, for signature-based approvals without an on-chain approve transaction.

---

## 3) Realistic operational flow

**Step 1 - Donation**  
- Donation via AA smart account with paymaster: **gas sponsored**, donor does not pay directly.  
- Donation via EOA + token with permit: **gasless approval**, then relayed donation.  
- Donation via EOA + token without permit: a **paid approval** may be required depending on the token.

**Step 2 - Aggregation**  
- Donations collected in an aggregation smart contract. Recommended triggers: amount threshold, max time window, acceptable gas window.

**Step 3 - Grouped transfer**  
- A single transaction ships funds to the NGO.  
- Standardized formula:  
  - `NGO net = Aggregated amount - reimbursed gas - relayer commission - technical reserve`

**Step 4 - Public reporting**  
- Donations, any conversions and the final transfer are visible on the chain explorer.

---

## 4) Gas, conversions and design options

**Physical constraints**: on EVM, gas is paid in the chain’s **native token**. Examples: ETH on Ethereum, Optimism, Arbitrum. **POL** on Polygon PoS.  
To respect the principle “no conversion for the commission,” DustEthic **v0.2** proposes **explicit options** to fund gas:

- **Option A - Relayer gas pool**: relayer maintains the required gas token pool. No conversion from donations.  
- **Option B - Minimal documented conversion**: pro-rata in-kind skim to buy the gas token, logged on-chain, without affecting the split formula beyond gas cost.  
- **Option C - L2-first**: prioritize low-fee L2s so gas is marginal.  
- **Option D - Sponsors**: paymasters funded by partners who top up gas and get reimbursed periodically.

---

## 5) Volatility - principles and strategies

**Rule**: the split is performed in crypto units. Percentages remain constant; the euro value varies until the NGO and relayer convert.

**Stablecoins**: reduce volatility but **do not eliminate risk** (depeg, address freezes, issuer risk).

**Post-receipt strategies**:  
- **NGO**: sell immediately, sell partially, or hold depending on internal policy and risk appetite.  
- **Relayer**: periodic selling, holding, or a mixed approach.

---

## 6) Actors and responsibilities

- **Donors**: send small amounts, ideally via AA to avoid paying gas directly.  
- **Relayers**: operate aggregation, publish public parameters, comply with v0.1 and keep **signed logs** of sensitive operations (with on-chain links).  
- **NGOs**: receive directly in their wallet, set a conversion policy and basic compliance procedures.

---

## 7) Donors - how to

1) Choose a DustEthic-compliant relayer.  
2) Connect your wallet.  
3) Select the NGO.  
4) Enter the amount in **crypto**.  
5) Sign. Depending on the case, the donation is gasless via AA or permit. Otherwise a paid approval may occur depending on the token.

**Cost for the donor**: ideally zero via AA or permit. Otherwise, only the initial approval if required by the token. Final transfer gas is **mutualized** and deducted before the NGO payout.

**Tracking**: each donation and the final transfer are visible on the chain explorer.

---

## 8) NGOs - integration, accounting, compliance

**Recommended wallet**: **Safe** (ex-Gnosis Safe) multi-signature for custody.  
**Fiat conversion**: via a registered exchange, per your policies.  
**Accounting**: record value upon receipt, define a conversion policy, track addresses.  
**Minimal compliance**: even in non-custodial, adopt **address screening** and a written policy. References: **OFAC** and **FATF R.15 / Travel Rule**. Exact obligations depend on your jurisdiction and status.

---

## 9) Relayers - minimum requirements DustEthic v0.1

**Transparency**  
- Open-source code. Public parameters: **degressive commission** (suggested max 15%), **campaign cap**, aggregation windows, supported networks, chosen gas option.  
- **Signed logs** and on-chain links; **CSV** export.  
- Readable on-chain dashboard.

**Non-custodial**  
- Funds held by smart contracts. Technical governance with no unilateral withdrawal power.

**Technical governance**  
- Admin roles under **Safe multi-sig**. Timelock on critical changes. Emergency procedures.

**Security**  
- Independent audit before mainnet. Bug bounty after launch.

**Gas and conversions**  
- Explicitly choose Option A, B, C or D and display publicly.  
- **Execute only if donation-to-fee ratio is favorable**; **gas reimbursed first**.  
- Commission always as a percentage of donated crypto (published **degressive** schedule).  
- If a gas conversion is necessary, log it on-chain.

**AA and compatibility**  
- ERC-4337 and paymaster support recommended on L2. The published EntryPoint is the reference implementation.  
- Fallback: **EIP-2771** meta-transactions if 4337/7702 (AA) is not supported.

**Asset allowlist v1**  
- Ethereum and EVM L2s: ETH, USDC, USDT.  
- Polygon PoS: possible but **gas in POL** - plan the logistics accordingly.  
- Reject by default illiquid, taxed, honeypot tokens or no-permit tokens if UX becomes impractical.

**Minimal compliance**  
- Proportionate AML policy, basic screening, logging of refusals.

---

## 10) Market references and positioning

- Traditional crypto donation platforms exist and mainly target medium to large donations with fast fiat conversion.  
  - **Every.org**: instant USD conversion, 1% broker commission + network fees.  
  - **The Giving Block**: subscription packages and processing fees, details provided commercially.  
- **DustEthic** focuses on **micro-donations via aggregation**, **native crypto split** on low-fee L2s, with **published campaign cap** and **degressive commission**.

---

## 11) Roadmap

**Phase 0 - Foundations [Q4 2025]**  
- Standard v0.1 specification  
- Asset and network allowlist v1  
- Aggregation and paymaster contract design - testnet  
- Security and governance policy

**Phase 1 - Development [2026]**  
- Open-source reference implementation  
- Tests on Sepolia and corresponding L2s  
- Third-party audit  
- Pilots with 1 relayer and 2 NGOs

**Phase 2 - Launch [2026+]**  
- Mainnet deployment + 2 L2s  
- 3-5 compliant relayers, 10+ NGOs  
- Community dashboard

**Phase 3 - Extension [2027+]**  
- More L2s, possibly other EVM ecosystems  
- Broader governance if traction

---

## 12) Join the project

- **Developers**: smart accounts, paymasters, aggregators. See ERC-4337 and EntryPoint references to start.  
- **NGOs**: test with a **Safe** wallet and an internal conversion policy.  
- **Relayers**: run an L2-first implementation and publicly document gas metrics and delays.  
- **Community**: feedback, translations, content.

Useful links:  
- Website: https://dustethic.org  
- Discord: https://discord.gg/fVFc26GV  
- GitHub: https://github.com/DustEthic  
- Bluesky: @dustethic.bsky.social

---

## 13) License

- Text: **CC BY 4.0**  
- Future code: **MIT**

---

## 14) Notes and references

- **ERC-4337 - Account Abstraction**: Docs and EntryPoint  
  - https://docs.erc4337.io/  
  - https://docs.erc4337.io/smart-accounts/entrypoint-explainer.html  
  - OpenZeppelin AA overview: https://docs.openzeppelin.com/contracts/5.x/account-abstraction

- **EIP-2612 - Permit**  
  - OpenZeppelin guide: https://docs.openzeppelin.com/contracts-cairo/2.x/guides/erc20-permit  
  - QuickNode tutorial: https://www.quicknode.com/guides/ethereum-development/transactions/how-to-use-erc20-permit-approval

- **Gas - definitions and network costs**  
  - Etherscan Gas Tracker: https://etherscan.io/gastracker  
  - Optimism - fee estimates: https://docs.optimism.io/app-developers/transactions/estimates  
  - Arbitrum - FAQ gas in ETH: https://docs.arbitrum.io/learn-more/faq  
  - Polygon PoS - MATIC→POL migration: https://polygon.technology/blog/matic-to-pol-migration-is-99-complete-everything-you-need-to-know

- **Stablecoin volatility**  
  - USDC depeg March 2023 (academic analysis): https://www.mdpi.com/2674-1032/3/4/30  
  - Press summary: https://www.investopedia.com/usdc-loses-peg-7254222

- **Custody security**  
  - Safe (ex-Gnosis Safe): https://docs.safe.global/ and https://safe.global/wallet

- **Compliance - AML, OFAC, FATF**  
  - OFAC - virtual currency FAQ: https://ofac.treasury.gov/faqs/topic/1626  
  - OFAC - Guidance PDF: https://ofac.treasury.gov/media/913571/download?inline=  
  - FATF - Targeted Update R.15 (2024): https://www.fatf-gafi.org/en/publications/Fatfrecommendations/targeted-update-virtual-assets-vasps-2024.html  
  - FATF - Targeted Update (2025, PDF): https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/2025-Targeted-Upate-VA-VASPs.pdf.coredownload.pdf  
  - Best Practices Travel Rule (2025, PDF): https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/Best-Practices-Travel-Rule-Supervision.pdf

- **Existing donation platforms**  
  - Every.org crypto - 1% broker + network: https://www.every.org/crypto and https://support.every.org/hc/en-us/articles/1500007842902-Are-there-any-fees-for-donating-cryptocurrency  
  - The Giving Block - pricing via packages: https://thegivingblock.com/ and https://thegivingblock.com/packages/

---

**End of DustEthic Guide v1.1-draft**
