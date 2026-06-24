# Fiber Developer Resources

A curated list of entry points for learning, building, and experimenting with CKB Fiber. This page is intentionally lightweight: it focuses on official references first and keeps community projects clearly labeled as experimental.

> **Curation note:** The Fiber ecosystem is still evolving. Community projects are useful for inspiration and pattern reference, but they may be incomplete, unmaintained, or not suitable for production use.

---

## What is Fiber?

Fiber Network is a peer-to-peer payment and swap network on CKB, similar in concept to Bitcoin Lightning. It enables:

- Extremely low-cost micro-payments
- Instant swaps between assets where channel paths exist
- Cross-network payments (e.g., Lightning ↔ Fiber)
- Privacy-preserving payments visible only to involved peers
- Programmable conditional payments and x402-style paywalls

---

## Protocol Implementation

| Resource | Description | Link |
|----------|-------------|------|
| **Fiber Node (FNN)** | Reference implementation, node source, build instructions, and protocol docs. | [github.com/nervosnetwork/fiber](https://github.com/nervosnetwork/fiber) |
| **Fiber Scripts** | On-chain scripts used by Fiber Node. | [github.com/nervosnetwork/fiber-scripts](https://github.com/nervosnetwork/fiber-scripts) |

## Official Documentation

| Resource | Description | Link |
|----------|-------------|------|
| **Fiber Docs** | Primary official documentation site for getting started and building on Fiber. | [fiber.world/docs](https://www.fiber.world/docs) |
| **Light Paper** | Protocol motivation and high-level design. | [fiber.world/docs/res/light-paper](https://www.fiber.world/docs/res/light-paper) |
| **Glossary** | Key terms such as HTLC, PTLC, invoices, and routing concepts. | [fiber.world/docs/res/glossary](https://www.fiber.world/docs/res/glossary) |
| **RPC Docs** | JSON-RPC methods and parameters for node integration. | [fiber.world/docs/api-reference](https://www.fiber.world/docs/api-reference) |
| **SDK Docs** | Official SDK entry point for app development. | [fiber.world/docs/build/sdk](https://www.fiber.world/docs/build/sdk) |

---

## SDKs and Libraries

### Official / Recommended

| Package | Purpose | Link |
|---------|---------|------|
| **`@ckb-ccc/fiber`** | Official JavaScript/TypeScript SDK for building apps on top of an existing Fiber node. Best starting point for most app integrations. | [fiber.world/docs/build/sdk/fiber-js](https://www.fiber.world/docs/build/sdk/fiber-js) |
| **`@nervosnetwork/fiber-js`** | Browser-oriented wrapper around the Fiber runtime. Keep this as a lower-level library for running fiber wasm node. | [npmjs.com/package/@nervosnetwork/fiber-js](https://www.npmjs.com/package/@nervosnetwork/fiber-js) |
| **`@ckb-ccc/ccc`** | CKBers' Codebase — useful when your app needs wallet, transaction, or asset handling alongside Fiber. | [docs.ckbccc.com](https://docs.ckbccc.com/) |
| **`@ckb-ccc/connector-react`** | React-based connector UI for CCC wallets. | [npmjs.com/package/@ckb-ccc/connector-react](https://www.npmjs.com/package/@ckb-ccc/connector-react) |

### Community / Experimental

| Package | Purpose | Link |
|---------|---------|------|
| **`@fiber-pay/sdk`** | Community Fiber SDK for application-facing typed APIs (universal + node + browser entrypoints). | [github.com/RetricSu/fiber-pay/tree/master/packages/sdk](https://github.com/RetricSu/fiber-pay/tree/master/packages/sdk) |
| **`@fiber-pay/react`** | Community high-level React hooks and components for integrating browser payment flows on Fiber. | [github.com/RetricSu/fiber-pay/tree/master/packages/sdk](https://github.com/RetricSu/fiber-pay/tree/master/packages/react) |

---

## Developer Tools

| Tool | Description | Link |
|------|-------------|------|
| **`fnn-cli`** | Official CLI bundled with FNN v0.8.0+. Manage peers, channels, and payments without raw JSON-RPC. | Included in [Fiber releases](https://github.com/nervosnetwork/fiber/releases) |
| **`ckb-cli`** | Official CKB CLI. Required to export keys for FNN. | [github.com/nervosnetwork/ckb-cli](https://github.com/nervosnetwork/ckb-cli) |
| **CKB Faucet** | Get free CKB testnet tokens to fund channels. | [faucet.nervos.org](https://faucet.nervos.org/) |
| **`fiber-ffi`** | Community FFI bindings for integrating Fiber capabilities from native runtimes. | [github.com/joii2020/fiber-ffi](https://github.com/joii2020/fiber-ffi) |
| **@fiber-pay/cli** | Community Fiber CLI operations and automation for humans and agents | [github.com/RetricSu/fiber-pay/tree/master/packages/cli](https://github.com/RetricSu/fiber-pay/tree/master/packages/cli) |
| **Fiber Node Installer** | Community automated installer for Windows/Linux with dashboard. | [github.com/tecmeup123/fiber-node-installer](https://github.com/tecmeup123/fiber-node-installer) |
| **Fiber Studio** | Community native desktop app for running Fiber nodes | [github.com/chukwuma619/fiber-studio](https://github.com/chukwuma619/fiber-studio) |
| **Fiber WSS Config Manual** | WebSocket secure configuration for nodes. | [github.com/nervosnetwork/fiber/blob/develop/docs/fiber-node-wss.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/fiber-node-wss.md) |
| **Public Nodes Manual** | How to run a public, discoverable node. | [fiber.world/docs/operate/connect-nodes](https://www.fiber.world/docs/operate/connect-nodes) |

---

## Demos and Starter Templates

| Demo | Stack | What It Shows | Link |
|------|-------|---------------|------|
| **`fiber-game`** | Rust (Axum) + browser JS | Rock-paper-scissors and guess-number with adaptor signatures and hold invoices. | [github.com/quake/fiber-demo/tree/main/fiber-game](https://github.com/quake/fiber-demo/tree/main/fiber-game) |
| **`fiber-escrow`** | Rust (Axum) + browser JS | Escrow trading with hold invoices. | [github.com/quake/fiber-demo/tree/main/fiber-escrow](https://github.com/quake/fiber-demo/tree/main/fiber-escrow) |
| **`fiber-l402`** | Express + Astro + React | Application-level L402 paywall using `@fiber-pay/sdk`. Reference implementation for custom middleware. | [github.com/RetricSu/fiber-l402](https://github.com/RetricSu/fiber-l402) |
| **`fiber-audio-player`** | React + TypeScript | Audio player demo that explores Fiber-based micro-payment flows. | [github.com/RetricSu/fiber-audio-player](https://github.com/RetricSu/fiber-audio-player) |
| **`fiber-charge-sim`** | Browser-based demo | Interactive charging simulation demo showcasing Fiber payment interactions. | [github.com/HappySonnyDev/fiber-charge-sim](https://github.com/HappySonnyDev/fiber-charge-sim) |
| **`fiber-browser-extension-demo`** | Browser extension | Demo showcasing a browser extension integrating Fiber-based payments. | [github.com/joii2020/fiber-browser-extension-demo](https://github.com/joii2020/fiber-browser-extension-demo) |
| **`fiber-android-demo`** | Android | Demo showcasing a mobile Android integration with Fiber-based payments. | [github.com/joii2020/fiber-android-demo](https://github.com/joii2020/fiber-android-demo) |

---

## Quick Checklist Before You Build

- [ ] You have CKB testnet CKB from the [faucet](https://faucet.nervos.org/).
- [ ] You can run a local FNN node or connect to an existing one.
- [ ] You understand the difference between **base-layer CKB transactions** (on-chain, slow, final) and **Fiber payments** (off-channel, fast, requires liquidity).
- [ ] You know whether your app needs **direct JSON-RPC**, the official **`@ckb-ccc/fiber`** SDK, or a **higher-level framework like CCC**.
- [ ] For browser apps, you have a plan for the WASM runtime size and required COOP/COEP headers.

---

*Last updated: 2026-06-18*
