# JPool assets

Static assets and data for JPool.

Base URL for published assets: `https://assets.jpool.one/`

## Contents

- `logo.svg` — JPool logo
- `community-good/` — Community Good validators list ([details](./community-good/README.md))
- `data/token-list.json` — token metadata used by JPool integrations
- `data/img/logo/` — logo assets
- `data/img/token/` — token icons

## New

- Added a token list in `data/token-list.json` with token ids, names, symbols, decimals, icon paths, and Solana mint addresses.
- Added structured image directories under `data/img/` for logo and token assets.

## Public Endpoints

- Token list: `https://assets.jpool.one/data/token-list.json`
- Community Good validators: `https://assets.jpool.one/community-good/validators.json`

## Token List Schema

```json
{
	"id": "<token id>",
	"name": "<display name>",
	"symbol": "<ticker>",
	"decimals": 9,
	"icon": "/img/<icon file>",
	"token_address": "<solana mint address>"
}
```

Current list includes `SOL`, `WSOL`, `JSOL`, `USDC`, and `USDT`.
