# 🔗 NEAR Intents MCP Server

[![npm version](https://img.shields.io/npm/v/@iqai/mcp-near-intents.svg)](https://www.npmjs.com/package/@iqai/mcp-near-intents)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## 📖 Overview

The NEAR Intents MCP Server enables AI agents to perform cross-chain token swaps through NEAR's intent-based architecture using the [Defuse Protocol one-click SDK](https://github.com/defuse-protocol/one-click-sdk-typescript). This server provides a comprehensive 5-step flow for discovering tokens, getting quotes, executing swaps, and tracking their status.

By implementing the Model Context Protocol (MCP), this server allows Large Language Models (LLMs) to facilitate cross-chain swaps seamlessly, bridging the gap between AI assistants and decentralized cross-chain infrastructure.

## ✨ Features

*   **Token Discovery**: Discover available tokens across multiple blockchains supported by the Defuse Protocol.
*   **Simple Quotes**: Get basic swap quotes without requiring addresses - perfect for rate checking.
*   **Full Quotes**: Get complete quotes with deposit addresses for executing swaps.
*   **Swap Execution**: Submit deposit transaction hashes to execute cross-chain swaps.
*   **Status Tracking**: Monitor swap progress and completion status in real-time.

## 📦 Installation

### 🚀 Using npx (Recommended)

To use this server without installing it globally:

```bash
npx @iqai/mcp-near-intents
```

### 🔧 Build from Source

```bash
git clone https://github.com/IQAIcom/mcp-near-intents.git
cd mcp-near-intents
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
        "NEAR_SWAP_JWT_TOKEN": "your-jwt-token-here"
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
      "args": ["/absolute/path/to/mcp-near-intents/dist/index.js"],
      "env": {
        "NEAR_SWAP_JWT_TOKEN": "your-jwt-token-here",
        "NEAR_SWAP_API_URL": "https://1click.chaindefuser.com"
      }
    }
  }
}
```

## 🔐 Configuration (Environment Variables)

| Variable | Required | Description | Default |
| :--- | :--- | :--- | :--- |
| `NEAR_SWAP_JWT_TOKEN` | Yes | JWT token for Defuse Protocol API authentication | - |
| `NEAR_SWAP_API_URL` | No | Custom API endpoint | `https://1click.chaindefuser.com` |

## 💡 Usage Examples

### 🔍 Token Discovery
*   "What tokens are available for cross-chain swaps on NEAR?"
*   "Show me all supported tokens in the Defuse Protocol."
*   "List available tokens I can swap from Arbitrum to Solana."

### 📊 Getting Quotes
*   "What's the rate to swap 100 USDC from Arbitrum to SOL?"
*   "Get me a quote for swapping ETH to USDC on NEAR."
*   "How much SOL would I get for 1000 USDC?"

### 💱 Executing Swaps
*   "I want to swap 500 USDC from Arbitrum to Solana."
*   "Help me execute a cross-chain swap with this deposit address."
*   "I've sent the funds to the deposit address, here's my transaction hash."

### 📈 Tracking Status
*   "Check the status of my swap with deposit address 0xabc..."
*   "Is my cross-chain swap complete?"
*   "What's the current state of my pending swap?"

## 🛠️ MCP Tools

<!-- AUTO-GENERATED TOOLS START -->

### `CHECK_NEAR_SWAP_STATUS`
[STEP 5] Check the current execution status of a NEAR intent swap. Returns the swap state (PENDING_DEPOSIT, PROCESSING, SUCCESS, REFUNDED, FAILED, etc.) along with detailed transaction information. Use this to monitor swap progress after initiating the swap, and continue polling until the swap is complete.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `depositAddress` | string | ✅ | The unique deposit address from the quote response - used to track and retrieve the current status of the swap |

### `EXECUTE_NEAR_SWAP`
[STEP 4] Submit a deposit transaction hash to initiate the swap after sending funds to the deposit address. This notifies the 1Click service that funds have been sent and triggers the swap execution process. Use this after users have sent their funds to the deposit address from the full quote response.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `txHash` | string | ✅ | Transaction hash of your deposit transaction that was sent to the deposit address from the quote |
| `depositAddress` | string | ✅ | The deposit address that was provided in the quote response and to which the deposit transaction was sent |

### `GET_NEAR_SWAP_FULL_QUOTE`
[STEP 2] Get a full quote with deposit address for a NEAR intent swap. This requires recipient and refund addresses and returns a unique deposit address where users can send their funds to initiate the swap. Use this when users are ready to proceed with the swap after checking the simple quote. NOTE: If users provide simple token names (e.g., 'ETH', 'USDC'), first use GET_NEAR_SWAP_TOKENS to discover the exact token IDs required for this API.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `swapType` | string |  | "EXACT_INPUT" | (Optional, defaults to EXACT_INPUT) Whether to use the amount as the output or the input for the basis of the swap: EXACT_INPUT - request output amount for exact input, EXACT_OUTPUT - request output amount for exact output. The refundTo address will always receive excess tokens back even after the swap is complete. |
| `originAsset` | string | ✅ |  | ID of the origin asset (e.g. 'nep141:arb-0xaf88d065e77c8cc2239327c5edb3a432268e5831.omft.near') |
| `destinationAsset` | string | ✅ |  | ID of the destination asset (e.g. 'nep141:sol-5ce3bf3a31af18be40ba30f721101b4341690186.omft.near') |
| `amount` | string | ✅ |  | Amount to swap as the base amount (can be switched to exact input/output using the dedicated flag), denoted in the smallest unit of the specified currency (e.g., wei for ETH) |
| `recipient` | string | ✅ |  | Recipient address. The format should match recipientType. |
| `recipientType` | string |  | "DESTINATION_CHAIN" | (Optional, defaults to DESTINATION_CHAIN) Type of recipient address: DESTINATION_CHAIN - assets will be transferred to chain of destinationAsset, INTENTS - assets will be transferred to account inside intents |
| `refundTo` | string |  |  | (Optional) Address for user refund |
| `refundType` | string |  | "ORIGIN_CHAIN" | (Optional, defaults to ORIGIN_CHAIN) Type of refund address: ORIGIN_CHAIN - assets will be refunded to refundTo address on the origin chain, INTENTS - assets will be refunded to refundTo intents account |
| `slippageTolerance` | number |  | 100 | (Optional, defaults to 100) Slippage tolerance for the swap. This value is in basis points (1/100th of a percent), e.g. 100 for 1% slippage. |
| `dry` | boolean |  | false | (Optional, defaults to false) Flag indicating whether this is a dry run request. If true, the response will NOT contain the following fields: depositAddress, timeWhenInactive, deadline. |
| `depositType` | string |  | "ORIGIN_CHAIN" | (Optional, defaults to ORIGIN_CHAIN) Type of the deposit address: ORIGIN_CHAIN - deposit address on the origin chain, INTENTS - account ID inside near intents to which you should transfer assets inside intents |
| `deadline` | string |  | "2026-01-27T22:45:37.613Z" | (Optional, defaults to 1 hour from now) Timestamp in ISO format, that identifies when user refund will begin if the swap isn't completed by then. It needs to exceed the time required for the deposit tx to be minted, e.g. for Bitcoin it might require ~1h depending on the gas fees paid. |
| `referral` | string |  |  | (Optional) Referral identifier (lower case only). It will be reflected in the on-chain data and displayed on public analytics platforms. |
| `quoteWaitingTimeMs` | number |  | 3000 | (Optional, defaults to 3000) Time in milliseconds user is willing to wait for quote from relay |

### `GET_NEAR_SWAP_SIMPLE_QUOTE`
[STEP 1] Get a simple quote for a NEAR intent swap between different chains and assets. This is a dry run that doesn't require any addresses - perfect for users who want to check swap rates and fees before committing to a swap. Use this when users want to explore swap options without providing recipient addresses. NOTE: If users provide simple token names (e.g., 'ETH', 'USDC'), first use GET_NEAR_SWAP_TOKENS to discover the exact token IDs required for this API.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `originAsset` | string | ✅ |  | ID of the origin asset (e.g. 'nep141:arb-0xaf88d065e77c8cc2239327c5edb3a432268e5831.omft.near') |
| `destinationAsset` | string | ✅ |  | ID of the destination asset (e.g. 'nep141:sol-5ce3bf3a31af18be40ba30f721101b4341690186.omft.near') |
| `amount` | string | ✅ |  | Amount to swap as the base amount, denoted in the smallest unit of the specified currency (e.g., wei for ETH) |
| `swapType` | string |  | "EXACT_INPUT" | (Optional, defaults to EXACT_INPUT) Whether to use the amount as input or output for the swap calculation |
| `slippageTolerance` | number |  | 100 | (Optional, defaults to 100) Slippage tolerance in basis points (100 = 1%) |
| `quoteWaitingTimeMs` | number |  | 3000 | (Optional, defaults to 3000) Time in milliseconds to wait for quote from relay |

### `GET_NEAR_SWAP_TOKENS`
[DISCOVERY] Get a list of tokens currently supported by the 1Click API for NEAR Intents. Returns token metadata including blockchain, contract address, current USD price, symbol, decimals, and price update timestamp. Use this to help users discover available tokens before requesting quotes.

_No parameters_

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
*   `src/lib/`: Shared utilities
*   `src/index.ts`: Server entry point

## 📚 Resources

*   [Defuse Protocol one-click SDK](https://github.com/defuse-protocol/one-click-sdk-typescript)
*   [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
*   [NEAR Protocol](https://near.org)

## ⚠️ Disclaimer

This project interacts with cross-chain swap infrastructure. Users should exercise caution and verify all transaction details before executing swaps. Cross-chain transactions involve multiple blockchains and carry inherent risks.

## 📄 License

[MIT](LICENSE)
