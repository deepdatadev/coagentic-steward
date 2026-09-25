# CoAgentic · Steward — OKX Dev Day 2026 (Evidence)

> **This repository is evidence, not the engine.** It does not contain the product — it *proves* it. Every claim below links to something you can check yourself: a content hash you can recompute against a live feed, an on-chain transaction you can open, a running surface. The generator that produces them — the prompts, the gates, the research — stays private. We open-source the slice that proves we built it, not the playbook.

## Submission

| | |
|---|---|
| **Team** | CoAgentic Markets (solo) |
| **Primary track** | **Build a Market** — X Layer · tokenized stocks / RWA |
| **Participation route** | **In Person** (auto-entered to Remote Build if not selected — per OKX) |
| **Product** | **Steward** — a liquidity-aware tokenized-equity index operated by an autonomous agent on X Layer |
| **Live product** | https://coagentic.markets/research |
| **Demo video** | https://youtu.be/mmwe8yVtfok |
| **Deep commit trail** | private repo, review-team access on request |

## As submitted — OKX Dev Day 2026 build round

_The product description on file with OKX, verbatim from the application. Reproduced here so the dossier matches what the judges hold._

> **Steward — a liquidity-aware tokenized-equity index operated by an autonomous agent on X Layer.**
>
> Tokenized equities exist on X Layer, but a contract address is not a usable portfolio: liquidity is uneven, off-hours prices can drift, and naive target-weight rebalancing can buy a weakening asset. Steward will manage an equity-forward basket of SPCXx, NVDAx, TSLAx, CRCLx, AAPLx and MSTRx, with a small ETH/OKB satellite, from a dedicated OKX Agentic Wallet.
>
> Its differentiator is CoAgentic-aware rebalancing. The Tradability Oracle blocks untradable legs; deterministic macro, turnover and concentration controls can defer a rebalance; and a channel-phase model plus Bull/Bear/Risk Council challenges each proposed trade. In one live-data deliberation, the planner proposed buying approximately $26 of underweight OKB, but the phase model classified OKB as Breaking while SPCXx and TSLAx were in Expansion, prompting the Council to recommend staging the deployment rather than buying mechanically.
>
> This is a graduation, not a greenfield build. Already working are the derived index and paper NAV, public Oracle and Steward dashboards, multi-agent Council, macro gate, phase classifier, and a successful on-chain ETH execution path on X Layer through OKX OnchainOS. Markets, Council and Steward (#12856) are three QA-approved OKX.AI ASP identities. Equity-screen snapshots and qualifying-universe identities are already hashed and anchored through an X Layer testnet registry.
>
> During build week we will connect final guarded approval to the multi-leg executor, verify a small live basket rebalance, expose the Oracle through x402, and extend provenance from the source screen through membership, weights, verdict and rebalance.

## What Steward is

A **tokenized-equity index on X Layer** (xStocks + a small crypto satellite). A multi-agent **Council** deliberates a rebalance; a **deterministic gate** (`hold / defer / approve`) guards execution and defers net risk-add into scheduled macro catalysts. Every decision is **content-hashed** and every trade is **on-chain** — so the record is checkable, not asserted. In a dense-fork macro regime it runs as a **briefing, not an autopilot**.

## Show the claim, hold the machine

The submission pack is a **publication, not a locker.** So this repo carries the *proof* and the *interfaces* — the running product, the verifiable hashes, the on-chain trades, the build record. The **generator** — the LLM Council prompts, the gate tuning, the data-stitch, the unpublished research and the roadmap — stays in a private repo. If scoring required handing over the engine, we would skip the prize. Two names take 1st and 2nd anyway; we build like the work has to stand without the badge.

## Deliverables

| Deliverable | Status |
|---|---|
| Live product (research surfaces + `/api/public/*` feeds + Oracle API) | ✅ live |
| On-chain, attested track record (FIFO realized P/L, per-agent attribution) | ✅ live |
| Sentinel macro engine + **News tracker** (built in-window) | ✅ live |
| Safeguards threat model (metadata-injection / poisoning) | ✅ published |
| One live Council-gated multi-leg rebalance on X Layer (tx-linked) | ⬜ Sep 19–25 |
| Oracle API x402 metering | ⬜ Sep 19–25 |
| 2–4 min demo video | ✅ [published](https://youtu.be/mmwe8yVtfok) |
| This evidence dossier | 🟢 in progress |

## How to verify — *don't trust, verify*

- **Content hashes** — the published Sentinel hypothesis carries a canonical content hash (`sha256` over the sorted-key JSON of the hypothesis core). It proves *content identity*, not publication time. Recompute it from the live feed and compare — no key, no trust required:

  Current published hash: **`0xf08e9417cff1787f6e6db20e7f1fdfe0840dd555abd0e192cc5b42f5bf1b9763`**

  ```js
  // node verify.mjs — recomputes the hash from the live feed and compares.
  import { createHash } from 'crypto';
  const canonical = (v) =>
    Array.isArray(v) ? `[${v.map(canonical).join(',')}]`
    : (v && typeof v === 'object')
      ? `{${Object.keys(v).sort().map(k => `${JSON.stringify(k)}:${canonical(v[k])}`).join(',')}}`
      : JSON.stringify(v);
  const sha = (v) => `0x${createHash('sha256').update(canonical(v)).digest('hex')}`;
  const feed = await (await fetch('https://tv.coagentic.markets/api/public/sentinel')).json();
  const h = feed.hypothesis, version = Number(h?.schemaVersion ?? 1);
  const KEYS = version >= 2
    ? ['schemaVersion','posture','confidence','published','expires','expects','falsifier','author','watching','source']
    : ['posture','confidence','published','expires','expects','falsifier'];
  const core = Object.fromEntries(KEYS.map(k => [k, h?.[k] ?? (k === 'schemaVersion' ? version : null)]));
  console.log(sha(core) === feed.hypothesisHash ? 'MATCH ' + feed.hypothesisHash : 'MISMATCH');
  ```

  The daily Council `candidate.hash` is computed the same way over the candidate object. The hash changes when the hypothesis is re-published, so a future reader should compare the snippet's output against the feed's live `hypothesisHash`, not against the pinned value above.
- **On-chain track record** — the realized round-trips are real X Layer transactions on the Steward book. Open them:
  - ETH round-trip (Aug 11→18): [`0x30894b01…`](https://www.oklink.com/x-layer/evm/tx/0x30894b0199c8d0af386c5b45204e373f1c1590b6e809bd49c3464689d570f835)
  - ETH de-risk (Sep 10): [`0xdadfce08…`](https://www.oklink.com/x-layer/evm/tx/0xdadfce080f29b4e381455058aafc203977e250b0235e9efe42691d1b757e9edc)
- **Live surfaces** — open them and watch them update:
  - Steward / TEI rebalance + Council verdict — https://coagentic.markets/research/tei
  - Sentinel Macro Tape — https://coagentic.markets/research/macro
  - Tradability Oracle — https://coagentic.markets/research/oracle

## Build period — what shipped Sep 15–25, 2026

_Judged work is only what was built in the official window. Foundation (the index, the Trading Desk, the on-chain track record + attribution) predates it and is context, not build-week work. The in-window build is the **macro-intelligence + safety layer** on top._

- **Safeguards threat model** (Sep 15) — mapped address/token poisoning **and token-metadata/context injection** (the ZeroDrift/OnchainOS drain: a token description read as an instruction → unlimited approve → wallet drained) onto our surfaces, and documented why our pipeline resists it structurally (**the model never holds the pen** — the signer is deterministic, no untrusted chain text reaches a signing context).
- **Sentinel macro engine — hardened into a live, checkable tape** (Sep 16–18):
  - a **time-aware macro gate** — lifts a defer after the print + settle window, and a catalyst-**proximity lookahead** defers risk-add *approaching* a high-impact fork;
  - **deliberation framing rules** — separate an instrument's *level* from its *session* move, reconcile channel-phase vs posture, distinguish priced-in from pending, hold confidence in a catalyst's immediate aftermath;
  - **data-integrity fixes** — stale-by-age market data, a suppressed cross-session 2s10s, occurred-catalysts rendered as *"occurred · <actual>"*.
- **Sentinel News tracker** (Sep 18) — a **zero-key breaking-news signal** (SEC.gov + Fed + crypto wires, keyword-triaged) that the Council reads, **flags and classifies** as candidate catalysts, promotable to the verified calendar with one command — an *unverified signal* tier kept distinct from the verified calendar.
- **Global macro coverage** (Sep 17–18) — non-US watch themes (BOJ/yen carry, China, EU MiCA) fed to the Council; the **BOJ decision** added as a scheduled fork; **CLARITY Act** failure and the **SEC "Innovation Exemption"** recorded.
- **Live proving ground** — through the window the system read the **Fed hike (Sep 16)**, the **BOJ hike to a 31-yr high (Sep 18)**, and the **SEC tokenized-stock exemption (Sep 17)** — de-risking ahead of the forks and holding dry powder, on the record.

## The catalyst that landed mid-build

On **Sep 17** the SEC granted a **5-year "Innovation Exemption"** — a regulated US path to trade **tokenized stocks** via permissioned on-chain AMMs. That is *exactly* Steward's domain, handed down three days into the build window — and the Sentinel news tracker surfaced it. The "why now" for tokenized equities on X Layer wrote itself.

---

_CoAgentic Markets · not investment advice · the engine is private; the proof is here._
