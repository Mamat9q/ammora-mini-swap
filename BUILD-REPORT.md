# Ammora Mini Swap — Build Report

Date: 18 Nov 2026 ~10:40 WIB. Builder: subagent (parent: Ammora Season 2 side chat).
Project name for the Ammora builder submit: **Ammora Mini Swap** (16 chars, ≤120).

## What was built

Static swap UI for $tAMM on GIWA Sepolia (chain 91342), in `~/workspace/ammora-builder/`:
- `index.html` — the app (vanilla JS + ethers.js 5.7.2 via cdnjs)
- `README.md` — project readme (contracts, how it works, faucet links)
- `.nojekyll` — for GitHub Pages

Features: connect wallet (MetaMask/injected, auto add/switch to GIWA Sepolia),
live ETH + $tAMM balances, live quote both directions from the pool, swap
ETH→$tAMM and $tAMM→ETH (approve-first on sells), 1% slippage, deadline now+600s,
tx hash + GIWA explorer link after each swap. No backend, no API keys.

Contracts (all verified live in ammora-probe2-2026-11-18.md):
- $tAMM `0x10B3Cc2fc981277Ada576891CF649D0ABA768B06` (6 dec)
- Pool `0x1ef92a6a9bb4bbed2b385dd5e688f88e7965bcf8` (graduated, status 3 — pool path only, curve dead)
- Native router `0x4FC91b90087288520A7bB4aA7Ab5441bb4E9C496`
- WETH `0x4200000000000000000000000000000000000006`

## Verification (all done 18 Nov 2026, read-only, no signing, no keystore)

- Live `quoteExactIn` via GIWA Sepolia RPC (curl, machine-built calldata):
  - buy 0.0001 ETH → **0.138915 tAMM**
  - buy 0.001 ETH → **1.389129 tAMM**
  - sell 1 tAMM → **0.0007055 ETH**
  - sell 0.139761 tAMM → 0.0000986 ETH
  (matches probe2 values within normal price movement)
- Swap calldata encoding cross-checked with eth_abi against the exact example in
  ammora-probe2 §4b: `swapPoolExactInNative` selector `0xdb73c039`, full calldata
  **byte-exact match**; sell selector `0xdfecc67d` confirmed.
- Inline JS: `node --check` OK (8,621 chars, 1 script block).
- ethers.js CDN (cdnjs 5.7.2): HTTP 200.
- All contract addresses, chain id 91342 (`0x164ce`), RPC and explorer URLs present in the page.

## NOT tested

- No live swap was broadcast from any wallet (per task: read-only verification only).
  The swap path is the same calldata the parent already executed successfully
  on 18 Nov (vault/swap/stake txs), and the encoding matches byte-exact, but an
  end-to-end click-through in a real browser (MetaMask connect + swap) has not
  been done — needs the parent's browser check on the live URL.
- Page load in a real browser (no console errors) — not testable from this
  subagent (no live browser). Needs the parent's browser check.

## BLOCKED: repo creation + GitHub Pages (needs parent/Boss)

- `gh` CLI is **not logged in** in this VM (`gh auth status`: not logged into any
  GitHub hosts; no `~/.config/gh`, no git credentials).
- GitHub MCP connector **is connected** as Mamat9q (verified via `get_me`), and can
  see the account's repos — but `create_repository` fails:
  `403 Resource not accessible by integration` (POST /user/repos). GitHub App
  tokens cannot create repositories under a user account via API.
- Earlier Mamat9q repos also have no Pages enabled (their `mamat9q.github.io/...`
  URLs all 404), so there is no existing Pages mechanism to reuse.

**To unblock (one-time, parent/Boss):**
1. Create the empty public repo **Mamat9q/ammora-mini-swap** at
   https://github.com/new (public, no README needed — or with README, either works).
2. Then push the files from `~/workspace/ammora-builder/` via the GitHub MCP
   `push_files` (owner Mamat9q, repo ammora-mini-swap, branch main) — or `git push`.
3. Enable Pages: repo Settings → Pages → Deploy from branch → `main` / root.
   Expected live URL: **https://mamat9q.github.io/ammora-mini-swap/**
4. Verify the live URL loads (parent browser check), then do the X post +
   Ammora builder submit (`submit {kind:"builder", url:<X post>, project:"Ammora Mini Swap",
   website:"https://mamat9q.github.io/ammora-mini-swap/"}`).

## Files

- `~/workspace/ammora-builder/index.html`
- `~/workspace/ammora-builder/README.md`
- `~/workspace/ammora-builder/BUILD-REPORT.md` (this file)
- `~/workspace/ammora-builder/.nojekyll`
