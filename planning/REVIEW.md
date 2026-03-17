# Review of `planning/PLAN.md`

## Overall Assessment

`planning/PLAN.md` is strong, comprehensive, and directionally clear. It gives a concrete product vision, defines major system choices, and provides enough implementation guidance for multiple contributors/agents to work in parallel.

The document is especially effective at:

- Framing user experience outcomes before implementation details.
- Defining architecture in practical, deployable terms (single container/port, static frontend, FastAPI backend).
- Capturing key interfaces and contracts (SSE endpoint, API route table, LLM structured schema).
- Anticipating testing and deployment concerns early.

## Content Feedback

### Strengths

- The scope is ambitious but still bounded (single-user, market orders only, no auth), which is a good project-shaping decision.
- The rationale table in Architecture is useful and helps future contributors preserve intent.
- Database schema is explicit and pragmatic for the current phase while leaving room for future multi-user support.
- LLM integration is concrete and actionable, especially around structured output and mock mode.
- Testing section covers unit and E2E concerns with realistic scenarios.

### Gaps / Ambiguities to Address

- **Ticker validation rules are unspecified**: define canonical ticker format, max length, casing behavior, and handling unknown symbols.
- **Trade validation details are incomplete**: specify minimum quantity, decimal precision rules, and whether quantity must be strictly positive.
- **Error contract is missing**: include a standard API error shape and representative status codes per endpoint.
- **Watchlist and stream relationship is slightly ambiguous**: the stream says "all tickers known to the system," but UX implies user watchlist; clarify authoritative source in single-user mode.
- **Portfolio valuation source is unspecified**: define behavior when live prices are missing/stale (e.g., last known price fallback, unknown valued at zero, stale marker).
- **Chat safety/guardrails are underdefined**: add explicit constraints for LLM actions (max trade size, rejected actions, idempotency expectations).
- **Operational defaults are partly implicit**: add concrete defaults for poll intervals, snapshot intervals, and reconnection retry timing in a centralized config table.

## Structure Feedback

### What Works

- The high-level flow from Vision → UX → Architecture → Data/API/LLM → Deployment/Testing is logical.
- Markdown headings are generally consistent and easy to navigate.
- Tables and bullet lists are used appropriately for quick scanning.

### Suggested Structural Improvements

- Add a short **"Scope / Non-goals"** section near the top (e.g., no auth, no real brokerage execution, no multi-user sessions yet).
- Add a **"Definition of Done"** section listing minimum acceptance criteria for MVP.
- Add an **"API Contracts"** subsection with request/response examples for each mutable endpoint (`POST /trade`, `POST /watchlist`, `POST /chat`).
- Add a **"State & Event Model"** subsection clarifying how prices, trades, snapshots, and chat actions propagate through the system.
- Consolidate configuration into a single **"Runtime Config Defaults"** table to reduce drift across sections.

## Clarity Feedback

### Clarity Strengths

- The prose is readable, practical, and mostly implementation-ready.
- Most sections avoid unnecessary abstraction and describe concrete behavior.
- The user journey is vivid, which helps align design and engineering choices.

### Clarity Issues to Fix

- There are recurring character encoding artifacts (e.g., corrupted em-dashes and arrow characters) that reduce readability; likely a UTF-8/Windows encoding mismatch.
- A few terms are overloaded (e.g., "known to the system" vs watchlist); tighten wording to avoid interpretation differences.
- Some requirements are "preferred" rather than "required" (e.g., chart library choice) without priority labels; add MUST/SHOULD language where needed.
- A few implementation assumptions are implicit (e.g., lazy DB init trigger timing, startup behavior if API keys are invalid); make these explicit.

## Recommended Next Edits (High Impact)

1. Fix encoding issues across the file to remove corrupted punctuation.
2. Add an MVP non-goals section and Definition of Done checklist.
3. Define standardized API success/error response envelopes with examples.
4. Clarify exact trade/ticker validation rules and edge-case behavior.
5. Add one centralized configuration table with explicit defaults.

## Suggested Quality Score

- **Content:** 8.5/10 (strong and thorough, with a few missing contracts)
- **Structure:** 8.5/10 (well organized, could use a few framing sections)
- **Clarity:** 7.5/10 (good writing, but encoding artifacts and a few ambiguities)
- **Overall:** **8.2/10**
