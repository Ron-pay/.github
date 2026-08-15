<div align="center">

# RolloPay

**Naira in, dollars held, out to any destination — on time, every time.**

A non-custodial payment rail on Stellar. Buy USDC with naira, hold it in a wallet only you control, and send it anywhere — a bank account, or a recurring bill like DSTV — automatically.

</div>

---

## What we're building

RolloPay does three things, deliberately, and nothing else:

1. **On-ramp** — naira in, USDC out, via a licensed anchor.
2. **Self-custody** — USDC sits in the user's own Stellar wallet. We orchestrate the flow; we never hold the balance.
3. **Payout** — USDC converts back to naira, landing in any Nigerian bank account, or paying a recurring bill directly.

The recipient never sees any of this. No wallet, no app, no seed phrase — just naira arriving on time. The dollar leg is invisible infrastructure, not a product feature.

A bank account is just one *destination*. A recurring bill — DSTV, electricity, data — is another. Same rail, different endpoint. That's what turns RolloPay from an app people open occasionally into one that quietly runs in the background of someone's life, making sure a family member never gets logged out because a bill was forgotten.

| Leg | What happens | Who handles it |
| --- | --- | --- |
| **On-ramp** | User pays naira; USDC arrives in their wallet | Licensed anchor |
| **Hold** | USDC rests in the user's own Stellar wallet | The user (self-custody) |
| **Payout** | USDC settles as naira, to a bank or a bill | Anchor + bank / biller rails |
| **Repeat** *(optional)* | A saved schedule fires again on the due date | RolloPay scheduler |

We rent the regulated fiat rail from a licensed anchor rather than building it from scratch — that's what lets a small team ship a working loop in months, prove volume, and pursue our own licence later.

---

## How the org is organized

One core monorepo, a few satellites — each isolated because it has a different lifecycle, toolchain, or access level.

| Repository | What it handles | Stack | Status |
| --- | --- | --- | --- |
| `rollopay` | The product: mobile app, orchestration API, recurring-payments scheduler, shared packages (`wallet`, `anchor`, `billers`, `core`, `ui`) | Turborepo · TypeScript | Planned — `M1` |
| `rollopay-contracts` | Soroban smart contracts — fee collection first, delegated-limit auto-pay later. Separate audit cycle. | Rust · Soroban SDK | Planned |
| [`rollopay-infra`](https://github.com/Ron-pay/rollopay-infra) | IaC, Docker, CI/CD, environment config, secrets policy, observability. Bootstraps the org itself as code. | Terraform · Docker · Actions | **Active** |
| `rollopay-docs` | Internal memory: ADRs, incident runbooks, compliance policy, partner integration notes | Markdown | Planned |
| [`Rollopay-landing`](https://github.com/Ron-pay/Rollopay-landing) | The public marketing site | Next.js | **Active** |

`rollopay-infra` also declares the org itself — repositories, teams, branch protection, and required checks are Terraform, not clicks, so "no direct pushes to `main`" doesn't quietly stop being true six months from now.

---

## How we build

- **Testnet first, always.** The full loop — KYC → on-ramp → payout → bill → schedule → alert — runs end-to-end on Stellar testnet before any pubnet configuration is touched. No real money moves until that's proven.
- **Non-custodial by design.** User funds never sit in an account we control. This is a security property and a regulatory one — it's the line that keeps auto-pay a deliberate, later decision rather than a launch feature.
- **Never hardcode a country or a currency.** `naira`, `NGN`, and "Nigerian bank" are adapter parameters, not constants — anywhere in the stack. It's what makes a second corridor an adapter, not a rewrite.
- **A destination is a destination.** A bank account, a bill, a future country's payout rail — all the same interface. Adding one is configuration, not a new code path.
- **Secrets are referenced, never committed.** Every credential is a name and an owner in an inventory, resolved at boot from a secret store. Nothing sensitive is ever "temporarily" in git.
- **An alert without a runbook is an incomplete alert.** If it can page someone, someone who didn't write it can already act on it.

---

## Where things stand

We're pre-launch and building in public within the org, not yet with users. Infra work is sequenced `I0`→`I6`; product work is sequenced `M0`→`M4`; infra leads product by one milestone so nothing gets built against a rail that doesn't exist yet.

The bar for our first real milestone: **ten real users, one corridor, one live recurring bill, every month** — end to end, on rails that have already survived testnet.

---

<div align="center">

**RolloPay** — a focused rail, not a wallet with everything in it.

</div>
