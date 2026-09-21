# Sol Pump — Solana Trading Desk

**Browser trading desk for Solana meme tokens — Telegram mint discovery, DexScreener watchlist with journey stats, native charts, manual Jupiter swaps, and optional Pump Catcher sniper.**

**Sol Pump** ingests new mints from Telegram alert channels, builds a personal watchlist with live prices and since-add / from-peak / off-low analytics, renders OHLC charts, and supports **manual Jupiter swaps** (quote → simulate → type **SWAP** to confirm) plus an optional **Pump Catcher** that can auto-buy on signals in mock or live mode with post-catch dump and take-profit monitoring.

Built for **authorized trading on chains you control** — pump.fun ecosystem tokens via Jupiter routing and DexScreener pools on a self-hosted desk. Keys and wallet material stay in local config.

**Made by [Logic Encoder](https://logicencoder.com)**

Private source: [logicencoder/sol-pump-app](https://github.com/logicencoder/sol-pump-app)

---

## Tech stack

| Layer | Technologies |
|-------|----------------|
| Backend | Python 3.10+, FastAPI, Uvicorn, Pydantic, orjson |
| Frontend | Vanilla HTML/CSS/JS, Clay UI, Lightweight Charts |
| Engine | Node.js child process — Jupiter quotes and swap execution |
| Chain | Solana mainnet via Helius or configured RPC |
| Market data | DexScreener API, Jupiter Quote/Swap API |
| Signals | Telegram public poller and optional Chrome realtime monitor |
| Real-time | WebSocket — status, signals, swaps, watchlist updates |
| Persistence | JSON under `data/` — watchlist, trade logs, settings |
| Quality | Playwright E2E, empirical test suite, CI smoke |

---

## Desk surfaces

| Area | In plain language |
|------|-------------------|
| **Charts tab** | Watchlist table, token hero KPIs, native OHLC chart, sortable coin detail table |
| **Tracker tab** | Portfolio median, win rate, brutal dumps, performance bar chart |
| **Trade tab** | Pump Catcher sniper, manual buy/sell ticket, swap blotter |
| **Telegram tab** | Feeder status, signal scanner, dev inject (when enabled) |
| **Ops tab** | Runtime, engine paths, safety caps, deploy hints |
| **Settings tab** | Channel list, poll intervals, auto-add signals, TG auto-start |
| **Jupiter routing** | Aggregator or pump.fun-direct routes via DexScreener pool pick |
| **Watchlist** | Up to 100 mints, manual add, FARTCOIN shortcut, auto-add from TG |
| **Live push** | WebSocket for prices, signals, swaps, pump catch events |

---

## Operator workflows

#### Telegram mint discovery
1. You start **Telegram catcher** in the sidebar — public channel poller parses Solana addresses from pump alerts every few seconds.
2. You enable Chrome mode — realtime monitor pushes lines; first login opens visible Chrome for Telegram Web auth.

#### Signal scanner and feed
1. A new mint appears in the **Telegram** tab scanner and Charts right-rail **Signals** pane with channel name.
2. Dev inject posts a test mint when inject mode is enabled — empirical tests without live TG.

#### Auto-add to watchlist
1. **Auto-add TG signals** on — new signal mint lands in watchlist with `source: telegram`.
2. Auto-add off — you paste mint manually in the watchlist form with optional symbol.

#### Watchlist price monitoring
1. Background poll refreshes all watchlist USD prices on your configured interval (default ~15s).
2. You click **Refresh watchlist prices** in the sidebar for immediate sync.

#### Token selection and context bar
1. You click a watchlist row — context bar shows symbol, mint, live price, since-add % on every tab.
2. You switch to **Trade** — the same token mint stays locked in the order ticket.

#### Price journey analytics
1. Token added at one price, now higher — **Since add** shows gain % in hero KPIs and summary table.
2. Token peaked then dumped — **From peak** and **Off low** update from stored price snapshots.

#### Native price chart
1. After enough watch snapshots, chart uses local 1m OHLC from your session data.
2. New token with few snapshots — synthetic chart built from DexScreener change anchors.

#### Coin detail table (Charts)
1. You sort by **Since add** descending to find best performers across watched coins.
2. You click **Detail table →** from Tracker to open the same table on Charts.

#### Tracker portfolio analytics
1. Tracker KPIs show median since-add %, win rate, count of coins down more than 50%.
2. You filter **Down** and sort **From peak** to review post-pump drawdowns.

#### Right rail — All coins bars
1. Horizontal bar chart colored by % since add; sort by **Pump** to compare peak runs.
2. You filter **Up** to hide losers during a quick visual scan.

#### Pair trade activity log
1. Select token — **Trades → Pair trades** lists recent on-chain txs for the DexScreener pair.
2. Trades accumulate in local logs since watchlist add time — not full chain history.

#### Manual Jupiter buy flow
1. Select token, enter SOL size, **Get route** → review quote → **Sim** → type **SWAP** → **Submit**.
2. Swap disabled in config — quote and sim work; execute panel shows gated state.

#### Manual Jupiter sell flow
1. Toggle **Sell**, enter token amount, preview token→SOL route, confirm with **SWAP**.
2. Position card shows wallet token balance — refresh before sizing sell.

#### Swap safety gates
1. Swap for mint not on watchlist — API returns safety error before engine call.
2. Live swap blocked when price impact exceeds your configured max percent.

#### Pump Catcher sniper
1. **ARM SNIPER** in Mock mode — TG signal on configured channel triggers mock buy and blotter entry.
2. **Catch** with pasted test mint while armed executes immediate sniper buy respecting Jupiter vs Pump.fun route.

#### Post-catch monitor and auto-exit
1. After catch, monitor polls price; **Dump** at % from peak triggers auto sell when enabled.
2. You enable **TP** at +50% — monitor sells when gain from entry hits target.

#### Trade blotter and history
1. Blotter filters **Buy** + **Ok** to review submitted live swaps with SOL amount and symbol.
2. **Your swaps** rail tab shows app swaps for selected token alongside pair trades.

#### Settings without env edits
1. You add a second Telegram channel in Settings — list persists and applies on next feeder start.
2. You set watchlist poll to 30s and activity poll to 10s — backend tasks pick up new intervals.

#### Ops and runtime visibility
1. **Ops → Runtime** shows uptime, WebSocket client count, and app version after deploy.
2. **Ops → Safety** displays max swap SOL, confirm phrase, and impact cap for audit.

#### UI layout persistence
1. You drag split gutters — watchlist width, chart vs table, blotter height persist in browser storage.
2. Reload page — last main tab and Charts right-rail pane restore from saved layout.

---

**Scope:** a local FastAPI desk on your machine — wallet, watchlist, and trade logs stay local. Routing goes through Jupiter aggregation and DexScreener pool picks. The desk is a desktop browser UI for research and execution you authorize.


## Quick start

```bash
cd sol-pump-app
bash setup-sol-pump-venv.sh
cp .env.example .env   # RPC, wallet, feature flags
bash start-sol-pump.sh
```

See the private repo README for engine setup, safety flags, and [REPOS.md](REPOS.md).

---

## Related repositories

| Repository | Role |
|------------|------|
| [sol-pump-app](https://github.com/logicencoder/sol-pump-app) | Private application code |
| [sol-pump-app-overview](https://github.com/logicencoder/sol-pump-app-overview) | This product overview |

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
