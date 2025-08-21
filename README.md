# MCP Trading Agents

A simulation of AI agents trading stocks on your behalf, with a local market, accounts, and trader behaviors orchestrated behind a simple app entrypoint. :contentReference[oaicite:0]{index=0}

**Live demo:** https://huggingface.co/spaces/Anindhya/MCP_TradingAgents

---

## Overview

This project models a miniature trading ecosystem:
- **Market services** provide prices and order matching.
- **Accounts services** hold cash/positions.
- **Trader agents** decide what to buy/sell and submit orders to the market.
- A lightweight **UI** (via Gradio) lets you launch and observe the system. The repository metadata on GitHub shows the Space-style config for Gradio with `app.py` and `sdk_version: 5.42.0`. :contentReference[oaicite:2]{index=2}

---

## Features

- **Multiple services**: market, accounts, and a push/notification layer
- **Agent-based traders** with pluggable strategies
- **Local persistence**: lightweight SQLite (`accounts.db`) for state
- **Composable modules**: clean separation between data, services, and agents
- **Gradio UI**: quick local run via `app.py` to simulate and observe trades :contentReference[oaicite:3]{index=3}

---

## Repository Structure (high level)

- `app.py` — App entrypoint (Gradio-based UI wrapper) :contentReference[oaicite:4]{index=4}
- `trading_floor.py` — Orchestrates traders, market, and accounts (simulation loop)
- `traders.py` — Trader agent behaviors/strategies
- `market.py`, `market_server.py` — Market model and service endpoints
- `accounts.py`, `accounts_server.py`, `accounts_client.py` — Account model/service and client
- `push_server.py` — Push/notification mechanism between components
- `database.py` — DB helpers (SQLite)
- `templates.py` — Reusable message/order templates
- `mcp_params.py` — Tunable parameters for the simulation
- `tracers.py` — Logging/tracing utilities
- `util.py` — Shared helpers
- `reset.py` — Utilities to reset data/state
- `requirements.txt` — Python dependencies :contentReference[oaicite:5]{index=5}

> The file list above is derived from the repo’s “Code” tab file tree. :contentReference[oaicite:6]{index=6}

---

## Tech Stack

- **Python 3.10+**
- **Gradio** for the local UI launched by `app.py` :contentReference[oaicite:7]{index=7}
- **SQLite** for lightweight persistence (`accounts.db`)
- **Requests / HTTP** for service-style modules (market/accounts/push)
- **Structured modules** for clear separation of concerns

---

## Installation

Clone the repository:
    git clone https://github.com/anindhya1/mcp_trading_agents.git
    cd mcp_trading_agents

Install dependencies:
    pip install -r requirements.txt  # see requirements.txt in the repo :contentReference[oaicite:8]{index=8}

(Optional) If you run into platform-specific issues, create and activate a virtual environment before installing.

---

## Quickstart

Start the app locally:
    python app.py

Then open the printed local URL in your browser (typically `http://127.0.0.1:7860/`) to interact with the simulation UI. The GitHub metadata indicates the app is Gradio-based and uses `app.py` as the entrypoint. :contentReference[oaicite:9]{index=9}

---

## Suggested Run Order (Service Mode)

If you prefer running services explicitly (instead of letting the app orchestrate):

1) Start Accounts service:
    python accounts_server.py

2) Start Market service:
    python market_server.py

3) Start Push/notification service:
    python push_server.py

4) Run the Trading Floor (agents and strategies):
    python trading_floor.py

You can then watch logs (via `tracers.py` integration) and inspect `accounts.db` for balances and positions.

---

## Configuration

Most knobs live in:
- `mcp_params.py` — simulation parameters (e.g., tick interval, symbols, spreads)
- `templates.py` — message/order templates
- `util.py` — shared helpers

For a clean slate:
    python reset.py

---

## How It Works (Conceptual)

1) **Market Loop**
   - The market service produces prices/ticks, accepts orders, and fills them based on simple microstructure assumptions.

2) **Accounts & Ledger**
   - The accounts service tracks cash, positions, realized P&L, and updates the SQLite database (`accounts.db`).

3) **Trader Agents**
   - Each trader agent (in `traders.py`) implements a strategy (e.g., trend-following, mean-reversion, random baseline).
   - The `trading_floor.py` orchestrator coordinates agent decisions and sends orders to the market.

4) **UI & Control**
   - `app.py` (Gradio) provides a minimal control surface to start/stop a simulation run and inspect status. The repo’s GitHub page explicitly lists `app.py` as the app file for a Gradio setup. :contentReference[oaicite:10]{index=10}

---

## Extending the System

- **Add a new strategy**: implement a class/function in `traders.py`, wire it into `trading_floor.py`.
- **Change market dynamics**: adjust logic in `market.py` (spreads, fills, tick generation).
- **Augment account rules**: customize `accounts.py` (margin rules, fees/commissions).
- **Observability**: add spans/events in `tracers.py` for richer logs or metrics.
- **Parameters**: centralize tunables in `mcp_params.py`.

---

## Notes, Safety, and Limitations

- This is a **simulation** for experimentation and learning—**not** financial advice and **not** connected to live brokerage APIs.
- Market/strategy logic is intentionally simplified to keep the code approachable.
- SQLite is used for convenience; consider Postgres for heavier workloads.
- Real-time concurrency is limited in the basic run mode; service mode can be extended with async or multiprocessing.

---

## Roadmap Ideas

- Richer price processes and order book simulation
- Portfolio-level risk constraints and position limits
- Async I/O and streaming tick pipelines
- Strategy plug-ins with config-driven selection
- Web dashboard for historical P&L, drawdowns, and exposure

---

## License

MIT (or your preferred license). Add a `LICENSE` file if not present.

---

## Acknowledgments

- Gradio for the local app wrapper (as indicated by repo metadata) :contentReference[oaicite:11]{index=11}
- Python ecosystem for batteries-included simulation building blocks

