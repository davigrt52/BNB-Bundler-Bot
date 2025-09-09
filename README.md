# BNB Sniper / Bundler Volume Bot

Hardhat project for BNB Smart Chain (BSC) that builds signed transaction bundles and submits them through [bloXroute](https://bloxroute.com/). It targets PancakeSwap V3–style flows (Uniswap V3 SDK, BSC addresses in `constants/`).

## Requirements

- Node.js **≥ 16** and npm **≥ 8**
- A BSC JSON-RPC endpoint (public or your own)
- A bloXroute account and API **Authorization** header
- A wallet with enough **BNB** for gas (the bot warns below **0.1 BNB**)

## Setup

```bash
git clone https://github.com/QuantyraLab/BNB-Bundler-Bot
cd BNB-Bundler-Bot
npm install
npm run dev
```

Copy `.env.example` to `.env` and fill in secrets (e.g. `cp .env.example .env` on Unix, or copy the file in Explorer on Windows).

Never commit `.env` (it is gitignored).

### Required variables

| Variable | Purpose |
|----------|---------|
| `PRIVATE_KEY` | Hex private key of the signing wallet (with or without `0x`) |
| `AUTHORIZATION_HEADER` | bloXroute API authorization value from your dashboard |

### Common optional variables

| Variable | Purpose |
|----------|---------|
| `RPC_URL` | BSC HTTP RPC (see `.env.example` for a default seed URL) |
| `BLOXROUTE_ENDPOINT` | bloXroute API base URL (default `https://api.blxrbdn.com`) |
| `TOKEN_NAME`, `TOKEN_SYMBOL`, `TOKEN_SUPPLY` | ERC20 deployment parameters |
| `GAS_PRICE_MULTIPLIER`, `FUTURE_BLOCK_OFFSET`, `FEE_TIER`, `INITIAL_PRICE` | Bundle / pool tuning |

Hardhat-specific keys (`BSC_RPC_URL`, `BSC_FORK_URL`, `BSCSCAN_API_KEY`, `FORK_BLOCK_NUMBER`, gas reporter keys) are documented in `.env.example`.

## Scripts

| Command | Description |
|---------|-------------|
| `npm start` / `npm run dev` | Run the bloXroute bundle bot (`scripts/bloxroute.js`) |
| `npm run compile` | Compile Solidity contracts |
| `npm run clean` | Remove Hardhat cache and artifacts |
| `npm run sn` | Start Hardhat node with BSC fork (`hardhat` network) |
| `npm test` | Run tests against `http://127.0.0.1:8545` (start `npm run sn` first) |
| `npm run deploy` | Run `scripts/deploy.js` (default Hardhat **Lock** sample; pass `--network` as needed) |

## Bot behavior

`scripts/bloxroute.js` loads `MyToken` from compiled artifacts, validates env, connects to BSC, estimates gas, signs a **token deployment** transaction, and POSTs the bundle to bloXroute (`/api/v1/bundle`). Additional steps (approvals, pool creation, liquidity, swaps) are marked as TODO in the script.

## Security

- Treat `PRIVATE_KEY` and `AUTHORIZATION_HEADER` as secrets.
- Review bundle logic and RPC providers before mainnet use.
- This software is provided as-is; you are responsible for compliance, funds, and operational risk.

## License

ISC (see `package.json`).
