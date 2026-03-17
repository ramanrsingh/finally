# FinAlly — AI Trading Workstation

A visually stunning, AI-powered trading workstation that streams live market data, supports simulated portfolio trading, and includes an LLM chat assistant that can analyze positions and execute trades on your behalf. Inspired by Bloomberg terminals with an AI copilot.

## Features

- **Live price streaming** — SSE-powered real-time price updates with green/red flash animations
- **Simulated portfolio** — start with $10,000 virtual cash, execute instant market orders
- **Watchlist** — track 10 default tickers (AAPL, GOOGL, MSFT, AMZN, TSLA, NVDA, META, JPM, V, NFLX); add/remove freely
- **Sparkline charts** — mini price charts per ticker, accumulated live from the SSE stream
- **Portfolio heatmap** — treemap showing positions sized by weight and colored by P&L
- **P&L chart** — total portfolio value tracked over time
- **AI chat assistant** — natural language interface to analyze your portfolio, suggest trades, and auto-execute them

## Quick Start

```bash
# macOS/Linux
./scripts/start_mac.sh

# Windows
./scripts/start_windows.ps1
```

Then open [http://localhost:8000](http://localhost:8000).

## Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
OPENROUTER_API_KEY=your-key-here   # Required for AI chat
MASSIVE_API_KEY=                    # Optional: real market data (default: simulator)
LLM_MOCK=false                      # Set true for testing without an API key
```

## Architecture

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js (TypeScript, static export) |
| Backend | FastAPI (Python, uv) |
| Database | SQLite (lazy-initialized, volume-mounted) |
| Real-time | Server-Sent Events (SSE) |
| AI | LiteLLM → OpenRouter (Cerebras) |
| Deployment | Single Docker container, port 8000 |

## Project Structure

```
finally/
├── frontend/        # Next.js TypeScript app
├── backend/         # FastAPI Python app
├── planning/        # Project documentation
├── test/            # Playwright E2E tests
├── scripts/         # Start/stop scripts
├── db/              # SQLite volume mount target
└── Dockerfile       # Multi-stage build
```

## Testing

```bash
# E2E tests (requires Docker)
cd test && docker compose -f docker-compose.test.yml up
```

Unit tests live in `frontend/` and `backend/` following each framework's conventions.
