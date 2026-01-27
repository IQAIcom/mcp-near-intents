# 🔄 NEAR Intent Swaps MCP Server

[![npm version](https://img.shields.io/npm/v/@iqai/mcp-near-intents.svg)](https://www.npmjs.com/package/@iqai/mcp-near-intents)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## 📖 Overview

The NEAR Intent Swaps MCP Server enables AI agents to perform cross-chain token swaps using [NEAR's intent-based architecture](https://near.org) powered by the [Defuse Protocol one-click SDK](https://github.com/defuse-protocol/one-click-sdk-typescript). This server provides comprehensive access to swap quotes, execution, and status tracking.

By implementing the Model Context Protocol (MCP), this server allows Large Language Models (LLMs) to discover available tokens, get swap quotes, execute cross-chain swaps, and monitor transaction status directly through their context window, bridging the gap between AI and decentralized cross-chain trading.

## ✨ Features

*   **Token Discovery**: Discover all available tokens supported for cross-chain swaps with metadata including price, symbol, and decimals.
*   **Simple Quotes**: Get basic swap quotes without requiring wallet addresses for quick rate checking.
*   **Full Quotes**: Get complete swap quotes with deposit addresses for actual swap execution.
*   **Swap Execution**: Execute swaps by submitting deposit transaction hashes after sending funds.
*   **Status Tracking**: Monitor swap execution progress and transaction states in real-time.

## 📦 Installation

### 🚀 Using npx (Recommended)

To use this server without installing it globally:

```bash
npx @iqai/mcp-near-intents
```

### 🔧 Build from Source

```bash
git clone https://github.com/IQAIcom/mcp-near-intent-swaps.git
cd mcp-near-intent-swaps
pnpm install
pnpm run build
```

## ⚡ Running with an MCP Client

Add the following configuration to your MCP client settings (e.g., `claude_desktop_config.json`).

### 📋 Minimal Configuration

```json
{
  "mcpServers": {
    "near-intents": {
      "command": "npx",
      "args": ["-y", "@iqai/mcp-near-intents"],
      "env": {
        "NEAR_SWAP_JWT_TOKEN": "your_jwt_token_here"
      }
    }
  }
}
```

### ⚙️ Advanced Configuration (Local Build)

```json
{
  "mcpServers": {
    "near-intents": {
      "command": "node",
      "args": ["/absolute/path/to/mcp-near-intent-swaps/dist/index.js"],
      "env": {
        "NEAR_SWAP_JWT_TOKEN": "your_jwt_token_here",
        "NEAR_SWAP_API_URL": "https://1click.chaindefuser.com"
      }
    }
  }
}
```

## 🔐 Configuration (Environment Variables)

| Variable | Required | Description | Default |
| :--- | :--- | :--- | :--- |
| `NEAR_SWAP_JWT_TOKEN` | Yes | JWT token for authentication with Defuse Protocol API | - |
| `NEAR_SWAP_API_URL` | No | Custom API endpoint for the swap service | `https://1click.chaindefuser.com` |

## 💡 Usage Examples

### 🔍 Token Discovery
*   "What tokens are available for NEAR intent swaps?"
*   "Show me all supported tokens with their current prices."
*   "Find the token ID for USDC on Arbitrum."

### 💱 Getting Quotes
*   "Get a simple quote to swap 100 USDC from Arbitrum to SOL on Solana."
*   "What's the exchange rate for swapping ETH to NEAR?"
*   "Get a full quote for swapping 1 ETH to USDC with my wallet address 0x..."

### 🚀 Executing Swaps
*   "I've sent funds to the deposit address. Execute my swap with transaction hash 0x..."
*   "Complete my pending swap with deposit address 0xabc..."

### 📊 Status Tracking
*   "Check the status of my swap at deposit address 0x..."
*   "Is my cross-chain swap complete yet?"
*   "Show me the transaction details for my recent swap."

## 🛠️ MCP Tools

<!-- AUTO-GENERATED TOOLS START -->

### `GET_NEAR_SWAP_TOKENS`
Discover available tokens for swaps. Returns token metadata including blockchain, contract address, current USD price, symbol, decimals, and price update timestamp.

_No parameters_

### `GET_NEAR_SWAP_SIMPLE_QUOTE`
Get a basic quote for a cross-chain token swap without requiring addresses. Perfect for checking swap rates and fees before committing.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `originAsset` | string | ✅ | | Origin asset identifier |
| `destinationAsset` | string | ✅ | | Destination asset identifier |
| `amount` | string | ✅ | | Amount to swap (in base units) |
| `swapType` | string | | "EXACT_INPUT" | Type of swap: EXACT_INPUT or EXACT_OUTPUT |
| `slippageTolerance` | number | | 100 | Slippage tolerance in basis points (100 = 1%) |
| `quoteWaitingTimeMs` | number | | 3000 | Time to wait for quote in milliseconds |

### `GET_NEAR_SWAP_FULL_QUOTE`
Get a complete quote with deposit address for a cross-chain token swap. Requires recipient and optionally refund addresses.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `originAsset` | string | ✅ | | Origin asset identifier |
| `destinationAsset` | string | ✅ | | Destination asset identifier |
| `amount` | string | ✅ | | Amount to swap (in base units) |
| `recipient` | string | ✅ | | Recipient address |
| `swapType` | string | | "EXACT_INPUT" | Type of swap |
| `recipientType` | string | | "DESTINATION_CHAIN" | Recipient address type |
| `refundTo` | string | | | Refund address (optional) |
| `refundType` | string | | "ORIGIN_CHAIN" | Refund address type |
| `slippageTolerance` | number | | 100 | Slippage tolerance in basis points |
| `dry` | boolean | | false | Whether this is a dry run |
| `depositType` | string | | "ORIGIN_CHAIN" | Deposit type |
| `deadline` | string | | | Deadline in ISO format |
| `referral` | string | | | Referral identifier |
| `quoteWaitingTimeMs` | number | | 3000 | Time to wait for quote in milliseconds |

### `EXECUTE_NEAR_SWAP`
Execute a swap by submitting a deposit transaction hash after sending funds to the deposit address.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `txHash` | string | ✅ | Transaction hash of the deposit transaction |
| `depositAddress` | string | ✅ | Deposit address for the swap |

### `CHECK_NEAR_SWAP_STATUS`
Check the execution status of a swap. Returns swap state and detailed transaction information.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `depositAddress` | string | ✅ | Deposit address to check status for |

<!-- AUTO-GENERATED TOOLS END -->

## 👨‍💻 Development

### 🏗️ Build Project
```bash
pnpm run build
```

### 👁️ Development Mode (Watch)
```bash
pnpm run watch
```

### ✅ Linting & Formatting
```bash
pnpm run lint
pnpm run format
```

### 📁 Project Structure
*   `src/tools/`: Individual tool definitions
*   `src/services/`: API client and business logic
*   `src/lib/`: Shared utilities and configuration
*   `src/index.ts`: Server entry point

## 📚 Resources

*   [Defuse Protocol one-click SDK](https://github.com/defuse-protocol/one-click-sdk-typescript)
*   [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
*   [NEAR Protocol](https://near.org)

## ⚠️ Disclaimer

This project interacts with cross-chain swap protocols and decentralized finance (DeFi) infrastructure. Cross-chain swaps involve significant risk including price slippage, failed transactions, and potential loss of funds. Users should verify all swap parameters and understand the risks before executing swaps. The authors are not responsible for any financial losses incurred through the use of this software.

## 📄 License

[MIT](LICENSE)
