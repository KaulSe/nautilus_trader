# Architect

[Architect](https://sandbox.architect.exchange) is an institutional financial technology platform providing modern
infrastructure for regulated trading across multiple asset classes. It operates as an SEC-registered
broker-dealer and NFA-registered introducing broker. This integration supports live market data ingest
and order execution for equities, futures, and perpetual futures.

:::warning
This integration is currently under construction and not yet ready for use.
:::

## Overview

This adapter is implemented in Rust, with optional Python bindings for use in Python-based workflows.
Architect uses REST for market data and order management, with WebSocket for real-time data and
execution updates.

The adapter includes the following components:

- `ArchitectHttpClient`: Low-level HTTP API connectivity.
- `ArchitectWebSocketClient`: Low-level WebSocket API connectivity.
- `ArchitectInstrumentProvider`: Instrument parsing and loading functionality.
- `ArchitectDataClient`: Market data feed manager.
- `ArchitectExecutionClient`: Account management and trade execution gateway.

## Supported instruments

| Product Type        | Data Feed | Trading | Notes                                        |
|---------------------|-----------|---------|----------------------------------------------|
| Perpetual Futures   | ✓         | ✓       | FX, rates, metals, and traditional assets.   |
| Futures             | Planned   | Planned | Traditional futures contracts.               |
| Equities            | Planned   | Planned | US equities.                                 |

## API credentials

Architect requires API credentials for authentication. You can provide these via environment variables:

### Required credentials

| Environment Variable     | Description                                                    |
|--------------------------|----------------------------------------------------------------|
| `ARCHITECT_API_KEY`      | Your Architect API key (e.g., `ak_...`).                       |
| `ARCHITECT_API_SECRET`   | Your Architect API secret.                                     |

### Optional 2FA credentials

If your account has two-factor authentication (2FA) enabled:

| Environment Variable     | Description                                                    |
|--------------------------|----------------------------------------------------------------|
| `ARCHITECT_TOTP_SECRET`  | Base32 TOTP secret for auto-generating 2FA codes.              |

This is the base32 secret displayed when you set up 2FA (often shown as a QR code or text).

### Environment selection

| Environment Variable     | Description                                                    |
|--------------------------|----------------------------------------------------------------|
| `ARCHITECT_IS_SANDBOX`   | Set to `true` for sandbox environment (default), `false` for production. |

## Authentication

Architect uses **bearer token authentication** via HTTP headers:

1. API key and secret (with optional TOTP) are used to obtain a session token via `/authenticate`.
2. The session token is then used as a bearer token for subsequent REST and WebSocket requests.

Session tokens expire after a configurable period (default: 3600 seconds).

## Configuration

### API endpoints

| Environment | HTTP API                              | Market Data WebSocket              | Orders WebSocket                    |
|-------------|---------------------------------------|------------------------------------|-------------------------------------|
| Sandbox     | `https://gateway.sandbox.architect.exchange/api`  | `wss://gateway.sandbox.architect.exchange/md/ws` | `wss://gateway.sandbox.architect.exchange/orders/ws` |
| Production  | `https://gateway.architect.exchange/api`          | `wss://gateway.architect.exchange/md/ws`       | `wss://gateway.architect.exchange/orders/ws`    |

## API documentation

- **Main website**: <https://sandbox.architect.exchange/>
- **API reference**: <https://docs.sandbox.x.architect.co/api-reference/>
- **Sandbox API**: `https://gateway.sandbox.architect.exchange/api`
- **Production API**: `https://gateway.architect.exchange/api`
