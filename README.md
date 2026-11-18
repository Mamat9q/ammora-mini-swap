# Ammora Mini Swap

Minimal swap UI for **$tAMM (tAmm Coin)** on **GIWA Sepolia (chain 91342)**, built on the
Ammora Season 2 contracts — the same graduated pool and native router the official
Ammora app uses. Built for the Ammora Season 2 **Builder** mission.

Live site: https://mamat9q.github.io/ammora-mini-swap/

## What it does

- Connect wallet (MetaMask / any injected wallet) — auto-switches to / adds GIWA Sepolia
- Shows GIWA ETH + $tAMM balances
- Live quote both directions from the pool (`quoteExactIn`)
  - ETH → $tAMM: `swapPoolExactInNative(pool, minOut, recipient, deadline, 0x0)` (payable)
  - $tAMM → ETH: approve router first, then `swapPoolExactInForNative(...)`
- 1% slippage (min-out = quote × 99/100), deadline = now + 10 min
- Shows tx hash + GIWA explorer link after every swap

## Contracts (GIWA Sepolia, chain 91342)

| Role | Address |
|---|---|
| $tAMM token (6 dec) | `0x10B3Cc2fc981277Ada576891CF649D0ABA768B06` |
| $tAMM pool (graduated) | `0x1ef92a6a9bb4bbed2b385dd5e688f88e7965bcf8` |
| Native router | `0x4FC91b90087288520A7bB4aA7Ab5441bb4E9C496` |
| WETH | `0x4200000000000000000000000000000000000006` |

The bonding-curve path is retired (curve status = 3, graduated) — this app uses the
pool path only, exactly like the official app does today.

## Tech

Static site, no backend, no API keys. Vanilla JS + ethers.js 5.7.2 via CDN.
Free test ETH: https://faucet.giwa.io/ or https://faucet.nodit.io/giwa-sepolia

## Testnet only

Nothing here touches mainnet or real money. All swaps are on GIWA Sepolia testnet.
