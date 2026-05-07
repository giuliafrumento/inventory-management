# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Factory Inventory Management System Demo with GitHub integration - Full-stack application with Vue 3 frontend, Python FastAPI backend, and in-memory mock data (no database).

## Critical Tool Usage Rules

### Subagents
Use the Task tool with these specialized subagents for appropriate tasks:

- **vue-expert**: Use for Vue 3 frontend features, UI components, styling, and client-side functionality
  - Examples: Creating components, fixing reactivity issues, performance optimization, complex state management
  - **MANDATORY RULE: ANY time you need to create or significantly modify a .vue file, you MUST delegate to vue-expert**
- **code-reviewer**: Use after writing significant code to review quality and best practices
- **Explore**: Use for understanding codebase structure, searching for patterns, or answering questions about how components work
- **general-purpose**: Use for complex multi-step tasks or when other agents don't fit

### Skills
- **backend-api-test** skill: Use when writing or modifying tests in `tests/backend` directory with pytest and FastAPI TestClient

### MCP Tools
- **ALWAYS use GitHub MCP tools** (`mcp__github__*`) for ALL GitHub operations
  - Exception: Local branches only - use `git checkout -b` instead of `mcp__github__create_branch`
- **ALWAYS use Playwright MCP tools** (`mcp__playwright__*`) for browser testing
  - Test against: `http://localhost:3000` (frontend), `http://localhost:8001` (API)

## Stack
- **Frontend**: Vue 3 + Composition API + Vite (port 3000)
- **Backend**: Python FastAPI (port 8001)
- **Data**: JSON files in `server/data/` loaded via `server/mock_data.py`

## Quick Start

```bash
# First-time backend setup
cd server && uv sync

# Backend
cd server
uv run python main.py

# Frontend
cd client
npm install && npm run dev

# Run backend tests
cd server && uv run pytest ../tests/backend/ -v
```

> Note: `scripts/start.sh` / `stop.sh` are macOS/Linux only. Windows users must start servers manually.

## Key Patterns

**Filter System**: 4 filters (Time Period, Warehouse, Category, Order Status) apply to all data via query params. Quarter values (Q1–Q4) are mapped to month ranges server-side in `server/main.py`.
**Data Flow**: Vue filters → `client/src/api.js` → FastAPI → In-memory filtering → Pydantic validation → Computed properties
**Reactivity**: Raw data in refs (`allOrders`, `inventoryItems`), derived data in computed properties
**Global Filter State**: `useFilters.js` is a singleton composable — all components share the same reactive filter refs
**i18n**: EN/JP support via `useI18n.js` composable + `client/src/locales/en.js|ja.js`. Currency uses fixed 1:150 USD→JPY rate (`client/src/utils/currency.js`)

## API Endpoints
- `GET /api/inventory` - Filters: warehouse, category
- `GET /api/inventory/{item_id}` - Single item by ID
- `GET /api/orders` - Filters: warehouse, category, status, month
- `GET /api/orders/{order_id}` - Single order by ID
- `GET /api/dashboard/summary` - All filters
- `GET /api/demand`, `/api/backlog` - No filters
- `GET /api/spending/*` - Summary, monthly, categories, transactions
- `GET /api/reports/quarterly`, `/api/reports/monthly-trends` - Reports page

> Note: `client/src/api.js` also defines task and purchase-order methods (`getTasks`, `createTask`, `createPurchaseOrder`, etc.) that are **not yet implemented** in the backend.

## Coding Guidelines
- Always document non-obvious logic changes with comments

## Common Issues
1. Use unique keys in v-for (not `index`) - use `sku`, `month`, etc.
2. Validate dates before `.getMonth()` calls
3. Update Pydantic models when changing JSON data structure
4. Inventory filters don't support month (no time dimension)
5. Revenue goals: $800K/month single, $9.6M YTD all months

## File Locations
- Views: `client/src/views/*.vue`
- Components (modals, filter bar): `client/src/components/*.vue`
- Composables: `client/src/composables/` (`useFilters.js`, `useAuth.js`, `useI18n.js`)
- Locales: `client/src/locales/en.js`, `client/src/locales/ja.js`
- Currency util: `client/src/utils/currency.js`
- API Client: `client/src/api.js`
- Backend: `server/main.py`, `server/mock_data.py`
- Data: `server/data/*.json`
- Tests: `tests/backend/` (conftest.py + 3 test modules)
- Styles: `client/src/App.vue`

## Design System
- Colors: Slate/gray (#0f172a, #64748b, #e2e8f0)
- Status: green/blue/yellow/red
- Charts: Custom SVG, CSS Grid for layouts
- No emojis in UI
- 5 inventory categories: Circuit Boards, Sensors, Actuators, Controllers, Power Supplies
- 4 warehouses: San Francisco, London, Tokyo, Mexico City
