# Fiber Developer Resources

A curated collection of tools, documentation, SDKs, and community projects for building on the CKB Fiber Network. Use this page as a starting point for hackathon projects and production prototypes.

> **Curation note:** The Fiber ecosystem is young and moving fast. Community hackathon projects are included because they show patterns you can reuse, but many are experimental or no longer maintained. Treat them as reference implementations rather than production dependencies.

---

## What is Fiber?

Fiber Network is a peer-to-peer payment and swap network on CKB, similar in concept to Bitcoin Lightning. It enables:

- Extremely low-cost micropayments
- Instant swaps between assets where channel paths exist
- Cross-network payments (e.g., Lightning ↔ Fiber)
- Privacy-preserving payments visible only to involved peers
- Programmable conditional payments and L402-style paywalls

The reference implementation is [nervosnetwork/fiber](https://github.com/nervosnetwork/fiber).

---

## Official Documentation

| Resource | Description | Link |
|----------|-------------|------|
| **Fiber Network Node (FNN)** | Source code, testnet config, build instructions, and protocol docs. | [github.com/nervosnetwork/fiber](https://github.com/nervosnetwork/fiber) |
| **Official Fiber Docs** | Fumadocs-based documentation site source. | [github.com/nervosnetwork/fiber-docs](https://github.com/nervosnetwork/fiber-docs) |
| **Run a Fiber Node** | Step-by-step guide for testnet setup using v0.8.0. | [fiber.world/docs/quick-start/run-a-node](https://www.fiber.world/docs/quick-start/run-a-node) |
| **Basic Transfer Example** | Send and receive payments after your node is running. | [fiber.world/docs/quick-start/basic-transfer](https://www.fiber.world/docs/quick-start/basic-transfer) |
| **Light Paper** | Protocol design and motivation. | [github.com/nervosnetwork/fiber/blob/develop/docs/light-paper.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/light-paper.md) |
| **Glossary** | Key terms (PTLC, HTLC, hold invoice, etc.). | [github.com/nervosnetwork/fiber/blob/develop/docs/glossary.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/glossary.md) |
| **RPC Documentation** | All JSON-RPC methods and parameters. | [github.com/nervosnetwork/fiber/blob/develop/crates/fiber-lib/src/rpc/README.md](https://github.com/nervosnetwork/fiber/blob/develop/crates/fiber-lib/src/rpc/README.md) |
| **Invoice Protocol** | Payment invoice format and semantics. | [github.com/nervosnetwork/fiber/blob/develop/docs/specs/payment-invoice.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/specs/payment-invoice.md) |
| **P2P Message Protocol** | Low-level peer messaging spec. | [github.com/nervosnetwork/fiber/blob/develop/docs/specs/p2p-message.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/specs/p2p-message.md) |
| **Dev Log Archive** | Pre-mainnet development updates and discussions. | [github.com/nervosnetwork/fiber/discussions/601](https://github.com/nervosnetwork/fiber/discussions/601) |

---

## SDKs and Libraries

### JavaScript / TypeScript

| Package | Purpose | Link |
|---------|---------|------|
| **`@fiber-pay/sdk`** | Core SDK for Fiber apps. Includes `FiberRpcClient`, browser WASM helpers, L402 middleware, and CCC funding resolver. | [npmjs.com/package/@fiber-pay/sdk](https://www.npmjs.com/package/@fiber-pay/sdk) |
| **`@nervosnetwork/fiber-js`** | JavaScript wrapper over the Fiber WASM runtime. Required peer dependency for browser apps. | [npmjs.com/package/@nervosnetwork/fiber-js](https://www.npmjs.com/package/@nervosnetwork/fiber-js) |
| **`@ckb-ccc/ccc`** | CKBers' Codebase — one-stop TypeScript SDK for CKB transactions, wallets, and assets. Useful when your app needs on-chain CKB logic alongside Fiber. | [docs.ckbccc.com](https://docs.ckbccc.com/) |
| **`@ckb-ccc/connector-react`** | React connector UI for CCC wallets. | [npmjs.com/package/@ckb-ccc/connector-react](https://www.npmjs.com/package/@ckb-ccc/connector-react) |
| **`@agentpay-dev/sdk`** | SDK for AI agents paying via CKB Fiber. | [npmjs.com/package/@agentpay-dev/sdk](https://www.npmjs.com/package/@agentpay-dev/sdk) |

### Other Languages

| Package | Purpose | Link |
|---------|---------|------|
| **`nervosnetwork/fiber-py-integration-test`** | Python integration tests — useful for understanding protocol flows. | [github.com/nervosnetwork/fiber-py-integration-test](https://github.com/nervosnetwork/fiber-py-integration-test) |
| **`ckb-sdk-go` / `ckb-sdk-java`** | Go and Java SDKs for CKB base layer (not Fiber-specific). | [github.com/nervosnetwork/ckb-sdk-go](https://github.com/nervosnetwork/ckb-sdk-go), [github.com/nervosnetwork/ckb-sdk-java](https://github.com/nervosnetwork/ckb-sdk-java) |

---

## Developer Tools

| Tool | Description | Link |
|------|-------------|------|
| **`fnn-cli`** | Official CLI bundled with FNN v0.8.0+. Manage peers, channels, and payments without raw JSON-RPC. | Included in [Fiber releases](https://github.com/nervosnetwork/fiber/releases) |
| **`ckb-cli`** | CKB key management and account tool. Required to export keys for FNN. | [github.com/nervosnetwork/ckb-cli](https://github.com/nervosnetwork/ckb-cli) |
| **CKB Faucet** | Get free CKB testnet tokens to fund channels. | [faucet.nervos.org](https://faucet.nervos.org/) |
| **Fiber Node Installer** | Community automated installer for Windows/Linux with dashboard. | [github.com/tecmeup123/fiber-node-installer](https://github.com/tecmeup123/fiber-node-installer) |
| **Fiber Scripts** | On-chain scripts used by Fiber channels. | [github.com/nervosnetwork/fiber-scripts](https://github.com/nervosnetwork/fiber-scripts) |
| **Fiber WSS Config Manual** | WebSocket secure configuration for nodes. | [github.com/nervosnetwork/fiber/blob/develop/docs/fiber-node-wss.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/fiber-node-wss.md) |
| **Public Nodes Manual** | How to run a public, discoverable node. | [github.com/nervosnetwork/fiber/blob/develop/docs/public-nodes.md](https://github.com/nervosnetwork/fiber/blob/develop/docs/public-nodes.md) |

---

## Demos and Starter Templates

| Demo | Stack | What It Shows | Link |
|------|-------|---------------|------|
| **`fiber-demo` (official-ish)** | Rust + browser JS | Two reference demos: a two-player game and an escrow trading system. Both use a frontend-driven architecture where the backend makes zero Fiber RPC calls. | [github.com/quake/fiber-demo](https://github.com/quake/fiber-demo) |
| **`fiber-game`** | Rust (Axum) + browser JS | Rock-paper-scissors and guess-number with adaptor signatures and hold invoices. | [github.com/quake/fiber-demo/tree/main/fiber-game](https://github.com/quake/fiber-demo/tree/main/fiber-game) |
| **`fiber-escrow`** | Rust (Axum) + browser JS | Escrow trading with hold invoices. | [github.com/quake/fiber-demo/tree/main/fiber-escrow](https://github.com/quake/fiber-demo/tree/main/fiber-escrow) |
| **`fiber-l402`** | Express + Astro + React | Application-level L402 paywall using `@fiber-pay/sdk`. Reference implementation for custom middleware. | [github.com/RetricSu/fiber-l402](https://github.com/RetricSu/fiber-l402) |
| **`fiber-x402-blog`** | Astro + React | Simplified paywall using Fiber's native x402 module (requires [fiber#1301](https://github.com/nervosnetwork/fiber/pull/1301)). Recommended starting point for new paywall apps. | [github.com/RetricSu/fiber-x402-blog](https://github.com/RetricSu/fiber-x402-blog) |

---

## Community Projects (Curated from Claw & Order Hackathon)

The [Claw & Order: CKB AI Agent Hackathon](https://talk.nervos.org/t/claw-order-hackathon-roundup/10154) produced 22 open-source projects. Many are prototypes, but several illustrate reusable patterns for Fiber-based agents, payments, and games.

> **Quality disclaimer:** Hackathon code is often unaudited and unmaintained. Use these for inspiration and pattern reference, not as dependencies.

### Payment, Commerce, and Agent Economies

| Project | Why It Matters | Link |
|---------|----------------|------|
| **`Fiber-pilot`** | AI agent that manages Fiber nodes via a 17-tool MCP server. Useful if you want to automate channel/liquidity operations. | [github.com/RaheemJnr/fiber-pilot](https://github.com/RaheemJnr/fiber-pilot) |
| **`Fiber Agent Pay`** | SDK + demo of a self-sustaining agent economy where bots buy/sell services. | [github.com/Jeremicarose/FiberAgentPay](https://github.com/Jeremicarose/FiberAgentPay) |
| **`Fiber402`** | HTTP 402 implementation for pay-per-use APIs paid instantly via Fiber. | [github.com/David-Pjs/fiber402](https://github.com/David-Pjs/fiber402) |
| **`1 Tok`** | Marketplace matching agent runtime buyers/providers with token-based billing and streaming payments. | [github.com/Keith-CY/1-tok](https://github.com/Keith-CY/1-tok) |
| **`Swift`** | Invoice-to-payment agent for freelancer payments. | [github.com/adisuyash/swift](https://github.com/adisuyash/swift) |

### Agent Coordination and Safety

| Project | Why It Matters | Link |
|---------|----------------|------|
| **`NERVE agent marketplace`** | Coordination layer with on-chain identities, reputation, and payment rails. | [github.com/RobaireTH/NERVE](https://github.com/RobaireTH/NERVE) |
| **`Remit`** | Permissioned execution runtime with user-defined spend limits and approved recipients. | [github.com/Ticoworld/remit](https://github.com/Ticoworld/remit) |
| **`Omniflow`** | Visual drag-and-drop builder for CKB/Fiber agent workflows. | [github.com/FadhilMulinya/Omniflow](https://github.com/FadhilMulinya/Omniflow) |
| **`CKB Alpha Agent`** | Oracle agent providing token analysis paid via Fiber micropayments. | [github.com/JouahriAli/Ckb-alpha-agent](https://github.com/JouahriAli/Ckb-alpha-agent) |

### Gaming and Social

| Project | Why It Matters | Link |
|---------|----------------|------|
| **`FiberQuest`** | Tournament agent for retro games that scores players and pays winners via Fiber. | [github.com/toastmanAu/fiberquest](https://github.com/toastmanAu/fiberquest) |
| **`CKB Arcade Agent Engine`** | Autonomous player for CKB Arcade games. | [github.com/GxNaitik/ckb-arcade](https://github.com/GxNaitik/ckb-arcade) |
| **`CKB Agent Forum`** | "Twitter for AI Agents" using CKB addresses as identity. | [github.com/ivanwopg-boop/CKB-forum](https://github.com/ivanwopg-boop/CKB-forum) |

### Financial and Trading Agents

| Project | Why It Matters | Link |
|---------|----------------|------|
| **`CAIT`** | AI trading agent with real CKB settlement. | [github.com/Victor-Okenwa/cait-bot](https://github.com/Victor-Okenwa/cait-bot) |
| **`CKB DAO Yield Agent`** | Natural-language interface for Nervos DAO operations. | [github.com/chetanchauhan64/CKB-AI-DAO-AGENT](https://github.com/chetanchauhan64/CKB-AI-DAO-AGENT) |
| **`CKB Position Guardian`** | DeFi collateral monitoring with custom lock scripts. | [github.com/anihdev/CKB_DEFI_GUARDIAN](https://github.com/anihdev/CKB_DEFI_GUARDIAN) |
| **`Vibe4Trading`** | Strategy benchmarking engine for AI trading agents. | [github.com/vibe4trading/vibe4trading](https://github.com/vibe4trading/vibe4trading) |

---

## Learning and Reference

| Resource | Description | Link |
|----------|-------------|------|
| **Fiber: A Payment Channel Network on CKB** | High-level blog post explaining the protocol. | [blog.cryptape.com/fiber-a-payment-channel-network-on-ckb](https://blog.cryptape.com/fiber-a-payment-channel-network-on-ckb) |
| **Nervos CKB Docs** | General CKB developer documentation. | [docs.nervos.org](https://docs.nervos.org) |
| **CKB Developer Resource** | Community-curated list of CKB resources. | [github.com/ckb-ecofund/CKB-Developer-Resource](https://github.com/ckb-ecofund/CKB-Developer-Resource) |
| **Nervos Talk — English** | Community forum for announcements and discussions. | [talk.nervos.org/c/english/49](https://talk.nervos.org/c/english/49) |
| **Nervos Discord** | Real-time help from the Nervos dev community. | [discord.gg/BF9AJ4fzs6](https://discord.gg/BF9AJ4fzs6) |

---

## Quick Checklist Before You Build

- [ ] You have CKB testnet CKB from the [faucet](https://faucet.nervos.org/).
- [ ] You can run a local FNN node or connect to an existing one.
- [ ] You understand the difference between **base-layer CKB transactions** (on-chain, slow, final) and **Fiber payments** (off-channel, fast, requires liquidity).
- [ ] You know whether your app needs **direct JSON-RPC**, **`@fiber-pay/sdk`**, or a **higher-level framework like CCC**.
- [ ] For browser apps, you have a plan for the WASM runtime size and required COOP/COEP headers.

---

*Last updated: 2026-06-18*
