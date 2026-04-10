# Repo Overview

## Summary

This repository is a full algorithmic trading platform, not just an API service. It combines:

- A Flask backend that acts as the main runtime shell
- A React 19 + TypeScript frontend
- A large broker adapter/plugin system
- REST and WebSocket interfaces
- Multiple databases for isolated subsystems
- Trading-specific services for orders, analytics, automation, and monitoring

## Main Entry Points

- `app.py`: Main backend startup path. Boots Flask, Socket.IO, middleware, REST API, blueprints, background initialization, and React frontend serving.
- `restx_api/__init__.py`: Central registration point for the `/api/v1` REST namespaces.
- `frontend/src/main.tsx`: React app entrypoint.
- `frontend/src/App.tsx`: Main frontend router with all page definitions.
- `blueprints/react_app.py`: Flask blueprint that serves the built React SPA for migrated frontend routes.

## Backend Shape

The backend is organized as a platform host around Flask:

- `blueprints/`: Web-facing backend modules such as auth, dashboard, sandbox, analyzer, chartink, flow, telegram, historify, security, admin, and more.
- `restx_api/`: Versioned REST API namespaces for trading and market-data actions like place order, modify order, quotes, history, option chain, funds, positions, holdings, search, symbol lookup, and telegram.
- `services/`: Core business logic. This appears to be where most feature behavior is implemented.
- `utils/`: Shared helpers for config, security, logging, sessions, HTTP clients, plugin loading, rate limiting, and monitoring.
- `websocket_proxy/`: Unified WebSocket normalization and broker feed integration layer.
- `events/` and `subscribers/`: Internal event bus and subscription system.

## Broker System

Broker support is a major architecture layer in this repo.

- `broker/`: One directory per broker.
- Most broker folders follow a similar structure:
  - `api/`
  - `mapping/`
  - `streaming/`
  - `database/`

The broker discovery and lazy loading path is centered in `utils/plugin_loader.py`:

- Reads broker `plugin.json` files for capabilities
- Caches supported exchanges and broker type
- Lazy-loads broker auth functions on demand instead of importing every broker SDK at startup

This helps keep startup lighter while supporting many brokers.

## Frontend Shape

The frontend is a substantial application, not a small admin panel.

- `frontend/src/App.tsx`: Declares the main React Router structure
- `frontend/src/app/providers.tsx`: Sets up React Query, market data context, toaster notifications, and devtools
- `frontend/src/pages/`: Feature pages across trading, analytics, setup, broker auth, admin, logs, telegram, sandbox, chartink, and strategies
- `frontend/src/api/`: Client-side API wrappers
- `frontend/src/hooks/`: Reusable frontend logic for sockets, market data, polling, visibility, and live updates
- `frontend/src/stores/`: Zustand stores for auth, session, broker state, theme, alerts, and flow workflow state
- `frontend/src/components/flow/`: Node-based visual strategy builder powered by React Flow

## Data Layer

Data storage is split across many focused modules in `database/`, including:

- Auth
- User
- Strategy
- Flow
- Chartink
- Sandbox
- Traffic
- Latency
- Historify
- Leverage
- Action Center
- Market Calendar
- Telegram
- Master contract / symbols

This repo appears to prefer isolated storage modules for different product areas instead of one single database abstraction.

## Key Observations

- Flask is the integration hub for almost everything.
- React is the main user-facing UI and has broad feature coverage.
- The system is built around broker abstraction and normalized APIs.
- The codebase contains both REST and streaming pathways.
- Startup includes a lot of initialization work, including databases and background services.
- The repository has broad functionality: execution, analytics, sandbox testing, chart tools, Telegram integration, ChartInk integration, Python strategy hosting, and a flow-based strategy builder.

## Suggested Reading Order

If revisiting the repo later, these are strong starting points:

1. `README.md`
2. `app.py`
3. `restx_api/__init__.py`
4. `utils/plugin_loader.py`
5. `frontend/src/App.tsx`
6. `frontend/package.json`
7. `pyproject.toml`

## Testing

There is a sizable `test/` directory. The test suite appears to include a mix of:

- Unit-style tests
- Integration-style tests
- Feature-specific validation scripts
- Sandbox-specific tests

## Practical Orientation

Depending on what needs to be changed, a good next deep-dive path would be:

- Backend architecture and request flow
- Frontend architecture and state flow
- End-to-end order lifecycle
- Broker plugin architecture
- Flow builder architecture
