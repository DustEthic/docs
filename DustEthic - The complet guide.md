# DUSTETHIC - LE GUIDE COMPLET [2025-10-21]

**Version**: 1.1-draft  
**Date**: Octobre 2025  
**Dernière mise à jour**: [2025-10-21]  
**Statut**: Phase 0 - Document de cadrage

> [!WARNING]
> Phase 0 - Document de cadrage
> - Document informatif - pas un conseil financier ou juridique.
> - Certaines capacités dépendent d’ERC-4337, de paymasters et de l’usage de L2.
> - Les montants de référence sont en unités crypto. Les équivalents € ne sont qu’indicatifs.
> - Transparence exigée: commission, fenêtres d’agrégation, réseaux supportés et option gas choisie doivent être affichés publiquement.

---

## 🎯 Core principle

**Think in crypto, not in fiat.**  
Amounts are counted in the native units of the chain. Example: you donate 0.0100 ETH, the NGO receives 0.0090 ETH if the publicly declared commission is 10%. This **neutralizes volatility in the split** among actors. **Fiat value still fluctuates** until each party converts.  
In this guide, fiat is only used to aid understanding.

### 🔌 Gas policy v0.1 - default

- **L2-first**: operate primarily on low-fee L2s (for example Optimism, Arbitrum) so gas is marginal.  
- **Relayer gas pool**: the relayer maintains a pool of the chain’s gas token (for example ETH on EVM L2). **No donor funds are converted to finance the relayer’s commission.**  
- **Optional safety net**: if the gas pool runs short, a **minimal, on-chain documented conversion** can be triggered to acquire gas token, without changing the standard split formula.  
- **Standard display**:  
  - Formula: `NGO net = Gross amount - gas - network fees - commission`  
  - Commission shown **as a percentage of the donated crypto** (for example 7% in ETH when the donation is in ETH)  
- **Transparency**: the chosen gas option (gas pool, L2-first, safety net) is **declared publicly** by each relayer.

---

## 📚 Table of contents

1) The real problem today  
2) DustEthic’s proposed solution  
3) Realistic operational flow  
4) Gas, conversions and design options  
5) Volatility - principles and strategies  
6) Actors and responsibilities  
7) Donors - how it works  
8) NGOs - integration, accounting, compliance  
9) Relayers - minimum requirements for spec v0.1  
10) Market references and positioning  
11) Roadmap  
12) Join the project  
13) License  
14) Notes and references

---

## 1) The real problem today

- On Ethereum L1, **gas is paid in ETH** and may exceed small donations. On L2, fees are much lower, but not zero.  
- Several EVM L2s also use **ETH as gas** (for example Arbitrum, Optimism). Polygon PoS uses **POL** since the MATIC to POL migration.  
- Result: a single micro-donation is often inefficient on L1, sometimes acceptable on L2, and depends on current network conditions.

---

## 2) DustEthic’s proposed solution

**Aggregation + on-chain transparency + splitting in crypto units**:

- **Relayers** aggregate micro-donations during a limited window, then execute **one grouped transfer** to the NGO.  
- NGO share, network fees and relayer commission are **calculated as a percentage of the donated crypto**, not a fiat equivalent.  
- Transparency via public explorers (for example Etherscan on Ethereum).

**Existing technical building blocks**:

- **Account Abstraction ERC-4337** with **paymasters** to sponsor user gas.  
- **EIP-2612 permit** where available, enabling approval by signature without an on-chain approve transaction.

---

## 3) Realistic operational flow

**Step 1 - Donation**  
- Smart account with AA and paymaster: **gas is sponsored**, donor does not pay directly.  
- EOA + token with permit: **approval without gas**, then relayed donation.  
- EOA + token without permit: an **on-chain approval** may be required, depending on the token.

**Step 2 - Aggregation**  
- Donations are collected in an aggregation smart contract. Recommended triggers: amount threshold, max time window, acceptable gas window.

**Step 3 - Grouped transfer**  
- A single transaction sends funds to the NGO.  
- Standard formula:  
  - `NGO net = Gross amount - gas - network fees - commission`

**Step 4 - Public reporting**  
- Donations, any minimal conversions and the final payout are visible on the chain explorer.

---

## 4) Gas, conversions and design options

**Physical constraints**: on EVM, gas is paid in the chain’s **native token**. Examples: ETH on Ethereum, Optimism, Arbitrum. **POL** on Polygon PoS.  
To uphold the principle “no conversion for the commission”, DustEthic v0.1 proposes explicit options for gas funding:

- **Option A - Relayer gas pool**: relayer keeps the required gas token. No conversion on donor funds.  
- **Option B - Minimal documented conversion**: pro-rata skim to buy gas token, logged on-chain, and not changing the split beyond the gas cost.  
- **Option C - L2-first**: operate primarily on low-fee L2s so gas is marginal.  
- **Option D - Sponsored paymasters**: partners preload the gas pool and are periodically reimbursed.

---

## 5) Volatility - principles and strategies

**Rule**: the split is made in crypto units. Percentages stay constant, fiat value varies until the NGO or relayer converts.

**Stablecoins**: reduce volatility but **do not eliminate risk** (for example depeg, address freeze, issuer risk).

**After receiving**:  
- **NGO**: sell immediately, sell partially, or hold depending on internal policy and risk tolerance.  
- **Relayer**: sell regularly, hold, or mixed approach.

---

## 6) Actors and responsibilities

- **Donors**: send small amounts, ideally via AA to avoid paying gas directly.  
- **Relayers**: operate aggregation, publish public parameters, comply with v0.1 spec and keep a public log for sensitive operations.  
- **NGOs**: receive directly to their wallet, define a conversion policy and minimal compliance procedures.

---

## 7) Donors - how it works

1) Pick a relayer that complies with DustEthic.  
2) Connect your wallet.  
3) Choose the NGO.  
4) Enter the amount in **crypto units**.  
5) Sign. Gasless via AA or permit when available. Otherwise a one-time approval may be needed depending on the token.

**Cost for donors**: ideally zero via AA or permit. Otherwise only the initial approval when required by the token. Gas for the final grouped transfer is **mutualized** and deducted before the NGO payout.

**Tracking**: each donation and the final payout are visible on the chain explorer.

---

## 8) NGOs - integration, accounting, compliance

**Recommended wallet**: **Safe** multi-signature for custody.  
**Fiat conversion**: via a registered exchange, per your internal policy.  
**Accounting**: record value at receipt time, define conversion policy, track addresses.  
**Minimal compliance**: even in non-custodial setups, adopt **basic address screening** and a written policy. References: **OFAC** and **FATF R.15 / Travel Rule**. Exact obligations depend on your jurisdiction and status.

---

## 9) Relayers - minimum requirements for DustEthic v0.1

**Transparency**  
- Open-source code. Public parameters: commission rate, aggregation windows, supported networks, chosen gas option.  
- Readable on-chain dashboard.

**Non-custodial**  
- Funds held by smart contracts. Technical governance without unilateral withdrawal power.

**Technical governance**  
- Admin roles under **Safe** multi-sig. Timelock for critical changes. Emergency procedures.

**Security**  
- Independent audit before mainnet. Bug bounty after launch.

**Gas and conversions**  
- Explicitly choose Option A, B, C or D and display it publicly.  
- Commission always as a percentage of the donated crypto.  
- If any gas conversion is required, log it on-chain.

**AA and compatibility**  
- ERC-4337 and paymaster support recommended on L2. The published EntryPoint is the reference implementation.

**v1 allowlist of assets**  
- Ethereum and EVM L2: ETH, USDC, USDT.  
- Polygon PoS: viable but **gas in POL** - plan logistics accordingly.  
- By default reject illiquid, taxed, honeypot tokens or those without permit if UX becomes impractical.

**Minimal compliance**  
- Proportionate AML policy, basic screening, logging of refusals.

---

## 10) Market references and positioning

- Crypto donation platforms for **regular size donations** already exist and often convert immediately to fiat.  
  - **Every.org**: instant USD conversion, broker fee about 1% + network fees.  
  - **The Giving Block**: commercial packages and processing fees.  
- **DustEthic** focuses on **micro-donations via aggregation** and **native crypto splits** on low-fee L2s.

---

## 11) Roadmap

**Phase 0 - Foundations [Q4 2025]**  
- Spec v0.1  
- v1 allowlist of assets and networks  
- Aggregation contracts and paymaster design - testnet  
- Security and governance policy

**Phase 1 - Development [2026]**  
- Open-source reference implementation  
- Tests on Sepolia and relevant L2s  
- Third-party audit  
- Pilots with 1 relayer and 2 NGOs

**Phase 2 - Launch [2026+]**  
- Mainnet + 2 L2 deployments  
- 3-5 compliant relayers, 10+ NGOs  
- Community dashboard

**Phase 3 - Expansion [2027+]**  
- More L2s, possibly other EVM ecosystems  
- Broader governance if traction emerges

---

## 12) Join the project

- **Developers**: smart accounts, paymasters, aggregators. Start with ERC-4337 and EntryPoint docs.  
- **NGOs**: test with a **Safe** wallet and an internal conversion policy.  
- **Relayers**: pilot an L2-first implementation and publish gas and delay metrics.  
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

- **ERC-4337 - Account Abstraction**: docs and EntryPoint  
  - https://docs.erc4337.io/  
  - https://docs.erc4337.io/smart-accounts/entrypoint-explainer.html  
  - OpenZeppelin AA overview: https://docs.openzeppelin.com/contracts/5.x/account-abstraction

- **EIP-2612 - Permit**  
  - OpenZeppelin guide: https://docs.openzeppelin.com/contracts-cairo/2.x/guides/erc20-permit  
  - QuickNode tutorial: https://www.quicknode.com/guides/ethereum-development/transactions/how-to-use-erc20-permit-approval

- **Gas - definitions and network costs**  
  - Etherscan Gas Tracker: https://etherscan.io/gastracker  
  - Optimism - fee estimates: https://docs.optimism.io/app-developers/transactions/estimates  
  - Arbitrum - gas in ETH FAQ: https://docs.arbitrum.io/learn-more/faq  
  - Polygon PoS - MATIC to POL migration: https://polygon.technology/blog/matic-to-pol-migration-is-99-complete-everything-you-need-to-know

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
  - FATF - Best Practices Travel Rule (2025, PDF): https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/Best-Practices-Travel-Rule-Supervision.pdf

- **Existing donation platforms**  
  - Every.org crypto - 1% broker + network: https://www.every.org/crypto and https://support.every.org/hc/en-us/articles/1500007842902-Are-there-any-fees-for-donating-cryptocurrency  
  - The Giving Block - offers and fee packages: https://thegivingblock.com/ and https://thegivingblock.com/packages/

---

**End of DustEthic Guide v1.1-draft**
