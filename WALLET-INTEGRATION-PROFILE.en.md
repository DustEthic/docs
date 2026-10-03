# DustEthic Wallet Integration Profile v0.1

Date: 2026-10-03. **DustEthic proposal, Phase 0, unaudited.**
This is not an ERC. The described adapters and Wallet Kit are not implemented. The website simulation uses fictional data only.

## Purpose

Let wallets identify, authorize, sponsor and verify donations using their own capabilities. Wallets retain keys and signing interfaces. DustEthic imposes no central operator, token or treasury.

## ZERO-OR-WAIT

`user_network_fee = 0`, `additional_user_payment = 0` and, for the external sponsorship profile, `gas_reimbursed_from_donation = 0`.

The sponsor must cover the whole required path, including setup, approvals, transfers, recipient account creation, storage or reserve requirements and failed attempts. No other donor asset can pay fees. A donation-funded gas reimbursement must not be called external sponsorship.

The signed donation is the maximum donation debit. Any batch commission or reserve must be disclosed, capped and accepted; it reduces the NGO net and is not an additional payment. Missing or insufficient funding means WAIT, followed by retry or expiry. No automatic paid fallback, native-token purchase, swap or bridge.

A sponsorship quote is not an inclusion guarantee. The adapter must enforce the absence of user charges during submission and failure, not merely show zero in the UI.

## Wallet decision

Use the wallet's local inventory, optionally ERC-7811 where available. Check the asset, chain, balance, dust policy, transfer behavior, authorization, verified recipient, NGO acceptance, sponsorship and minimum NGO net.

States: ELIGIBLE (proposal requires consent), WAIT (no execution), EXCLUDED (show reason), EXPIRED, CANCELLED, SUBMITTED, CONFIRMED and FAILED. Only verified receipt establishes a donation. Separate batches by asset, network and beneficiary. Fiat amounts are illustrative.

## Common intent, network-specific authorization

Proposed fields: profile version, intent ID, network, asset ID, atomic amount, beneficiary, recipient, expiry, nonce, zero user fee limits, batch deduction cap, minimum NGO amount, adapter ID and revocation policy.

Use decimal integer strings in smallest units with verified asset identifiers and decimals. Bind the real signature to the chain, recipient, amount, domain and expiry. Common business fields do not imply a universal signature or a trusted metadata-only JSON object.

## Initial qualification paths

| Proposed adapter | Preconditions | Wallet implementation work |
|---|---|---|
| `evm-wallet-paymaster` | Compatible EIP-5792 account and chain, effective sponsorship; ERC-7677 if supported. | Discover capabilities, build calls, enforce needed atomicity and zero user charges, track status and receipt. |
| `evm-token-authorization` | Qualified ERC-7758 token, or ERC-2612 with a bounded executor. | Display typed data, bind the destination to the intent, submit through a sponsoring relayer, check residual allowances and nonces. |
| `solana-fee-payer` | Qualified wallet, token, external sponsor and recipient. | Check all instructions and accounts, sponsor ancillary costs, gather signatures, verify receipt. |

ERC-2612's deadline limits permit submission, not an allowance's lifetime after creation. A permit alone does not bind the final recipient. An executor must enforce the donation intent. Sponsor any paid on-chain revocation or refuse that path and explain the limitation. UI expiry does not revoke on-chain permissions.

EIP-7702 is optional; do not impose unexplained persistent delegation. Permission proposals ERC-7715, ERC-7674 and ERC-8255 are separate from the ERC-7677 paymaster interface. Stellar, Aptos and Sui are research candidates, not implemented integrations.

## Proposed result envelope

Include intent IDs, versions, adapter, network, asset, beneficiary, gross, deductions, NGO net, user fees, actual sponsor, sponsor cost, network references and finality. Verify actual balance effects and fee payer; a transaction hash alone proves neither receipt nor zero user fees.

Keep sponsor costs in native units separate from donation-asset deductions. See [fictional examples](examples/wallet-profile-v0.1.json). No production-validated schema or deployed contract is claimed.

## Proposed Wallet Kit

Target API: `detectEligibleDust()`, `buildDonationIntent()`, `findSponsoredExecution()`, `requestAuthorization()`, `submit()` and `verifyDonationProof()`.
Future work: reviewed JSON Schemas, signed test vectors, adapters and network fixtures. The kit holds no keys or funds. An open sponsor registry is a later idea, not an existing service.

## Acceptance cases

- Missing, rejected, expired or insufficient sponsor: WAIT, no debit and no paid fallback.
- Sponsor withdrawn after consent: recheck funding, no user-paid failure.
- Wrong token, chain, recipient, amount, nonce or expiry: reject.
- Residual approvals or uncovered setup/native-token costs: reject the path.
- No implicit swaps, bridges or asset/beneficiary mixing; no duplicate or replayed donation.
- Equivalent fictional donations share an envelope, with network-specific references.
- Never label simulations, intents or submitted operations as confirmed donations.

## Sources

ZERO-OR-WAIT, the state model and kit API are DustEthic proposals dated 03.10.2026. External capabilities and statuses are linked in [TECHNICAL-WATCH.md](TECHNICAL-WATCH.md) and [SOURCES.md](SOURCES.md), checked 03.10.2026. Final status does not establish any particular wallet or token's compatibility.
