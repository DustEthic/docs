# [2025-12-10] DustEthic - Wallet White Paper v1.0 (EN)

## 0. Important disclaimer - document status

This document combines two types of information:

1. **Current and verifiable elements**
   - Description of the DustEthic Standard as published on the official repositories and on dustethic.org.
   - Public technical references about wallets, account abstraction, ERC 4337, EIP 7702, paymasters, relayers, etc.

2. **Projections, examples and design assumptions**
   - All amounts, percentages, adoption rates, donation volumes, fee models, revenue scenarios and roadmaps in this document are **working illustrations**.
   - They do not describe the actual performance of any wallet, relayer, NGO or of the DustEthic project, and they may change or be discarded.

As a result:

- This document is **not**:
  - a promise of profit, yield or future performance,
  - a financial product offering,
  - a contractual commitment from DustEthic or from any partner.

- Each stakeholder (wallet, relayer, NGO, user) must:
  - perform their own technical, legal, tax and regulatory checks,
  - adapt or reject the examples if they do not fit their context.

- If there is any conflict between this document and:
  - the DustEthic Standard published on the official repositories,
  - or the most recent technical documentation,
  then **the most recent official sources** prevail.

This document must be read as a **design and discussion support**, not as a final description of a production system.

---

## 1. Context and objectives

DustEthic aims to turn "dust" in non custodial wallets (small unusable balances) into aggregated, traceable micro donations to public interest projects, through an open documented standard.

Wallets are a key actor of the DustEthic Standard. Without them, there is no dust detection, no usable UX, and no large scale distribution.

This white paper focuses on:

- the functional role of wallets in the DustEthic ecosystem
- technical integration options (EOA, Account Abstraction, EIP 4337, EIP 7702)
- commission models considered for wallets
- examples of potential gains (non binding)
- the functional mockup of the DustEthic module inside a wallet

---

## 2. Role of the wallet in the DustEthic Standard

### 2.1 Actors recap

In the DustEthic vision, at least the following actors are involved:

- Donor: end user, owner of the non custodial wallet
- Wallet: application that exposes the UX and signs operations
- DustEthic Relayer / Aggregator: service that aggregates micro amounts and handles donation logic
- Paymaster / Bundler (AA): technical service that sponsors gas and handles UserOperation packaging
- NGO / Beneficiary project: final recipient of funds
- DustEthic Standard: public rules, APIs and best practices

### 2.2 Wallet position

The wallet is:

- the UX entry point: activation, consent, configuration, monitoring
- the consent gatekeeper: nothing happens without explicit opt in and signature
- the technical router:
  - it chooses which flows to expose to DustEthic (tokens, networks, frequencies)
  - it decides, or lets the user decide, whether a wallet side commission is acceptable

The DustEthic Standard should stay neutral on exact business choices, but:

- enforces transparency on fees
- enforces a minimum level of traceability
- forbids turning DustEthic into a financial product (no yield promise)

---

## 3. User journeys inside the wallet

### 3.1 Activation (strong opt in)

1. The user opens their non custodial wallet.
2. A "DustEthic" module is offered:
   - as a dedicated tab
   - as a card inside a "utilities" hub
   - or via a suggestion banner ("Turn your dust into donations").

3. The intro screen explains in plain language:
   - what "dust" is
   - how aggregation and donation work
   - who the actors are (wallet, relayer, NGO)
   - how fees and commissions are handled, with transparency

4. The user must:
   - accept DustEthic terms and privacy notice
   - acknowledge that DustEthic does not create any yield or investment product
   - select one or more NGO categories or a list of NGOs

### 3.2 Initial configuration

The user chooses at least:

- Networks and tokens included:
  - for example: specific Ethereum L2s, optionally L1 with care
- Dust threshold per token:
  - for example: balances under 2 USD equivalent count as "dust" (configurable)
- Action mode:
  - Mode 1: "Manual sweep" - user triggers the operation from time to time
  - Mode 2: "Periodic sweep" - for example monthly or quarterly
  - Mode 3 (optional): "Opportunistic sweep" - for example when some condition is met during a transaction

### 3.3 Manual sweep (minimal viable scenario)

1. The user opens "DustEthic" and taps "Scan my dust".
2. The wallet:
   - queries the blockchain or an indexer for balances
   - applies threshold rules
   - builds a list of "donation candidates" per token / network

3. The user chooses:
   - which tokens to include or exclude
   - the NGO or NGO category (if not already configured)

4. The wallet displays a summary:
   - total estimated donation amount (in stablecoin or USD equivalent)
   - total fees and commissions (relayer + wallet), clearly itemized
   - estimated gas cost and who pays it

5. The user signs the transaction or UserOperation:
   - directly for EOA wallets
   - or via a smart wallet AA flow
   - DustEthic does not change the wallet signature model

### 3.4 Automatic modes and safeguards

Automatic modes are powerful but sensitive.  
Prudence principles:

- explicit activation by the user, with a monthly donation cap
- a clear "pause" and "disable" option
- hard limit per operation (for example not more than X USD per sweep)
- clear notifications after each sweep

---

## 4. Technical architecture on the wallet side

### 4.1 EOA vs Account Abstraction

Two main cases:

1. Classic EOA wallets:
   - user signs regular transactions to a DustEthic Aggregator contract
   - gas is paid in the native token, unless an external mechanism sponsors it

2. Account Abstraction capable wallets (ERC 4337, EIP 7702):
   - user signs UserOperations or "sponsored" transactions
   - a Paymaster covers gas fees
   - DustEthic logic can be integrated with less friction in existing flows

The DustEthic Standard must be compatible with both, but should encourage AA integrations for better UX (gas sponsorship, batching, programmable logic).

### 4.2 Integration with relayers, bundlers, paymasters

On the wallet side, integration can rely on a "DustEthic Wallet Connector" providing:

- dust detection APIs (or using an external indexer)
- simulation APIs:
  - amount estimation
  - gas and fee estimation
- construction APIs:
  - building ready to sign transactions or UserOperations
- monitoring APIs:
  - operation status
  - donation receipts (hash, NGO, timestamp)

In an ERC 4337 context:

- the DustEthic relayer may act as:
  - a specialized bundler, and or
  - a paymaster sponsoring certain DustEthic UserOperations  
    (for example above a minimum donation amount or during specific campaigns)

---

## 5. Commission model for wallets

### 5.1 Core principles

- Commissions are taken from the "DustEthic portion" of flows, never from the user s base balance.
- Percentages should remain modest (in line with typical wallet and exchange service fees).
- The commission breakdown must be transparent:
  - NGO share
  - relayer share
  - wallet share (if any)
  - optional DustEthic standard funding share (open source, audits, etc.)

### 5.2 Illustrative grid (work example)

Non binding example to show orders of magnitude:

- Gross donation amount: 100 units
- Global DustEthic fee: 3 % (3 units)
- Possible breakdown:
  - 1.5 % (1.5 units) to the relayer (infrastructure, gas, risk)
  - 1.0 % (1 unit) to the wallet (integration, UX, support)
  - 0.5 % (0.5 unit) to a DustEthic fund (standard maintenance, audits)

Net NGO amount: 97 units.

The DustEthic Standard does not hard code these values but enforces:

- clear fee disclosure
- forbidding hidden fees in obscure conversions
- a "full NGO" mode where the user can refuse any wallet side commission

### 5.3 Alternative scenarios

- "Zero wallet fee" mode:
  - 0 % wallet
  - relayer funded by other sources (grants, sponsorships, etc.)
- "Sponsored campaign" mode:
  - reduced or zero DustEthic fee covered by a third party sponsor
  - sponsor may be displayed in the UX
- "Premium wallet" mode:
  - slightly higher wallet commission in exchange for extra services  
    (richer reporting, NGO branding, etc.)  
  - must stay a minority option and be clearly labeled

---

## 6. Potential gains for a wallet (non binding examples)

These scenarios are only here to illustrate scale effects.  
They are not revenue promises.

### 6.1 Example 1 - Medium size wallet

Purely illustrative assumptions:

- 1 000 000 monthly active users (MAU)
- 10 % enable DustEthic
- 2 dust sweeps per user per year
- average donation per sweep: 5 USD
- wallet commission: 1 % of donated amount

Rough calculation:

- DustEthic users: 100 000
- Annual sweeps: 200 000
- Total donation volume: 1 000 000 USD
- 1 % wallet commission: about 10 000 USD per year

Not huge on its own, but:

- can help cover integration and maintenance costs
- brand value of a "pro social" feature may be significant
- upside grows if DustEthic becomes a general pattern

### 6.2 Example 2 - Very large scale wallet

Same logic with:

- 10 000 000 MAU
- 15 % DustEthic adoption
- 3 sweeps per user and year
- 7 USD per sweep
- 1 % wallet commission

Rough orders of magnitude:

- DustEthic users: 1 500 000
- Annual sweeps: 4 500 000
- Donation volume: about 31 500 000 USD
- 1 % wallet commission: about 315 000 USD per year

This shows DustEthic is a "long tail" flow:

- low per user amounts
- potentially meaningful at scale  
  while remaining very acceptable for donors

---

## 7. DustEthic wallet module mockup

### 7.1 UX principles

The DustEthic module inside a wallet should:

- be reassuring (no hidden automatic actions)
- be readable for non expert users
- clearly show:
  - estimated amounts
  - fees and commissions
  - global impact of donations

### 7.2 Screen 1 - Introduction

Typical content:

- Title: "Turn your dust into donations"
- Short explanation:
  - "DustEthic lets you turn your small unused balances into aggregated micro donations for NGOs."
- Three key bullets:
  - "You stay in control."
  - "Fees are transparent."
  - "Donations are traceable on chain."

Actions:

- Primary button: "Set up DustEthic"
- Secondary link: "Learn more" pointing to public DustEthic docs

### 7.3 Screen 2 - Token and threshold selection

Content:

- List of detected tokens and networks
- For each token:
  - total balance
  - estimated "dust" part (below threshold)
  - check box "include in DustEthic"
- Parameters:
  - minimal threshold per token (for example 2 USD)
  - sweep frequency (manual, monthly, quarterly)

### 7.4 Screen 3 - NGO selection

Options:

- Quick category selection:
  - environment, education, health, etc.
- Pre approved NGO partner list
- Future options:
  - custom lists, regional filters, etc.

### 7.5 Screen 4 - Summary and fees

Shows:

- estimated donation (gross)
- fee breakdown:
  - x % relayer
  - y % wallet
  - z % DustEthic fund
- estimated net to NGOs
- gas fee estimation and who pays it

Call to action:

- Primary button: "Sign and run DustEthic sweep"
- Secondary option: "Save configuration and exit"

### 7.6 Screen 5 - History and receipts

For each DustEthic operation:

- date and time
- network / token
- gross, fees, NGO net amount
- on chain tx id (link to explorer)
- NGO or NGO pool

---

## 8. Risks, constraints and open questions

### 8.1 UX and reputation risks

- Possible confusion between:
  - explicit donation and
  - "automatic deductions" user did not clearly understand  
  Strong opt in and reversibility are mandatory.

- Negative perception if:
  - wallet fees are seen as too high
  - fee transparency is weak

### 8.2 Technical constraints

- AA integration complexity (ERC 4337, EIP 7702) for teams new to it
- Cross chain management:
  - various token types
  - different gas patterns per network
- Need for robust logs and receipts:
  - audits
  - NGO reporting
  - user confidence

### 8.3 Regulatory constraints

- Service qualification may vary by jurisdiction:
  - donation service
  - payment service
- Need to avoid:
  - any resemblance to an investment product
  - any kind of yield language or tokenized financial incentive for donors

### 8.4 Open questions

- How far to standardize wallet side APIs:
  - minimal common core vs large flexibility
- Governance:
  - who validates standard changes
  - how wallets and NGOs feedback is integrated
- Standard funding:
  - small share of fees
  - external sponsorship
  - 100 % open source donations only

---

## 9. Integration roadmap for partner wallets (high level)

1. Phase 0 - Discovery
   - review DustEthic Standard
   - identify internal constraints (legal, UX, tech)

2. Phase 1 - Internal prototype
   - minimal integration:
     - manual sweep
     - single network
     - one or two NGOs
   - testnet deployment

3. Phase 2 - Limited pilot
   - restricted launch on a small opt in user segment
   - collect UX feedback
   - tune thresholds, messages and fees

4. Phase 3 - Progressive rollout
   - broader user rollout
   - adding more networks and NGOs
   - improved reporting

5. Phase 4 - Standardization and optimization
   - active participation in DustEthic Standard evolution
   - alignment with new versions
   - cost and flow optimization

---

## 10. Critical summary (devil s advocate)

Potential strengths for wallets:

- modest but recurring additional revenue stream
- pro social brand positioning
- open standard, no lock in

Weaknesses and risks:

- uncertain short term profitability
- UX misunderstandings if poorly implemented
- maintenance and compliance overhead

Provisional conclusion:

- DustEthic is unlikely to become a main revenue line for wallets.
- Yet, if well designed, it can:
  - strengthen brand perception
  - cover part of costs
  - make "Web3 for good" tangible without speculative tokens.

---

## 11. External sources (technical background)

These sources are provided as general technical background on Account Abstraction, paymasters and wallet revenue models.  
They are not affiliated with DustEthic.

- ERC 4337 - Account Abstraction (documentation and specification):  
  https://docs.erc4337.io  
  https://ethereum-magicians.org/t/erc-4337-account-abstraction-via-entry-point-contract-specification/7160
- Account abstraction and smart wallets:  
  https://www.alchemy.com/overviews/what-is-account-abstraction  
  https://docs.stackup.fi/docs/understanding-erc-4337
- Paymasters and gas sponsorship:  
  https://www.alchemy.com/overviews/what-is-a-paymaster
- EIP 7702 and smart EOAs:  
  https://www.quicknode.com/guides/ethereum-development/smart-contracts/eip-7702-smart-accounts  
  https://www.openfort.io/blog/eip-7702-with-erc-4337
- Wallet and exchange revenue models (fees, commissions):  
  https://streamflow.finance/blog/how-does-a-crypto-wallet-make-money  
  https://www.coinsclone.com/how-do-crypto-wallets-make-money  
  https://trusteeglobal.eu/academy/how-cryptocurrency-fees-are-calculated
