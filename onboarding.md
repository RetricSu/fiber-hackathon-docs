# Fiber Developer Onboarding

This guide is designed to help new developers go from “I have heard of Fiber” to “I can run a node, complete a real transfer, and start building on top of it.”

The goal is not to overwhelm you with protocol detail on day one. The goal is to help you reach the first meaningful milestone as quickly as possible: a working node, a successful payment, and a clear sense of where to go next.

---

## What you will learn

By the end of this path, you should be able to:

- understand what Fiber is and how it differs from Bitcoin Lightning
- run a Fiber node on Testnet and use its basic CLI/RPC commands to inspect basic information
- open a channel, create or receive an invoice, and complete a first transfer flow
- connect a simple app or script to a running node through the SDK and decide whether Fiber fits your own project or demo

---

## Stage 0: Concepts Alignment (10–15 minutes)

**Goal:** Build a simple mental model before touching the tooling.

Start with these references:

- [What is Fiber Network?](https://www.fiber.world/docs)
- [How Fiber Network Works](https://www.fiber.world/docs/how-it-works)
- [Fiber Network Light Paper](https://www.fiber.world/docs/res/light-paper)
- [Glossary](https://www.fiber.world/docs/res/glossary)

Think through these three questions:

1. **Why does Fiber exist?**
   Read the overview and light paper to understand why Fiber is useful for fast, low-cost, repeated transfers and why off-chain channels matter.

2. **How do payment channels and multi-hop routing work?**
   Read the “How Fiber Network Works” page and click the simulation to understand how channels are opened, updated, and settled, and how payments can travel through intermediate nodes.

3. **How is Fiber different from Bitcoin Lightning?**
   Read the light paper and the overview together. The key difference is that Fiber is built on CKB, designed for multi-asset, cross-chain and programmable payment flows, and aims to be more composable with CKB-native assets and smart contract logic.

### What to understand

- Fiber is not just “another wallet”; it is a network of payment channels.
- A channel is a shared state between two participants, not a permanent ledger entry for every payment.
- Routing depends on available liquidity, so a path may exist in theory but still fail in practice if the capacity is insufficient.
- The base-layer role of CKB is to anchor and enforce the channel state when channels open, update, or close.
- Compared with Bitcoin Lightning, Fiber is positioned as a CKB-native payment channel network with broader multi-asset, built-in cross-chain and programmability goals.

### Success checkpoint

You should be able to answer these three questions in your own words: why Fiber exists, how the channel/routing model works, and how it differs from Bitcoin Lightning.

---

## Stage 1: Quick Start (30–60 minutes)

**Goal:** Reach the “aha” moment quickly by getting a node running.

### Recommended path

For most developers, the fastest route is:

1. follow the official quick-start guide for [Run a Fiber Node](https://www.fiber.world/docs/quick-start/run-a-node). Notes: use a pre-built binary first; build from source only if you need full control or custom development.
2. You can also use [docker-compose](https://github.com/HappySonnyDev/fiber-demo-startup/tree/demo-0.8) to start 3 nodes on your local machine and use the built-in [frontend app](https://github.com/HappySonnyDev/fiber-demo-startup/tree/demo-0.8#2-start-the-demo-app) to inspect what a smallest Fiber network looks like.

### Basic setup checklist

Before you begin, make sure you have:

- Git
- a shell environment
- ckb-cli for key handling
- access to Testnet funds
- docker and docker-compose for the second path

### The minimum commands you should know

The fiber pre-built release contains two binaries `fnn` and `fnn-cli`. The `fnn` binary exposes HTTP RPC and node-maintenance tooling.  The `fnn-cli` is the official command-line tool for managing a Fiber node.

```bash
./fnn-cli info
./fnn-cli peer list_peers
./fnn-cli channel list_channels
./fnn-cli --help
```

These are the core commands you will come back to when inspecting your node, debugging issues, or checking basic state.

### Success checkpoint

You have a running node on Testnet, the node responds to basic inspection commands and RPC calls, and you have inspect the smallest local network for fiber in your machine.

---

## Stage 2: Your First Payment Flow (1–2 hours)

**Goal:** Move from “node is running” to “I understand the basic payment lifecycle.”

At this stage, you should practice the essentials:

- connect your node to another peer
- open a channel
- create or receive an invoice
- send a payment and verify the result

### Recommended Path

- starts with [Basic Transfer](https://www.fiber.world/docs/quick-start/basic-transfer)
- continue with [Transfer Stablecoin](https://www.fiber.world/docs/quick-start/transfer-stablecoin)
- Try [Connecting to Public testnet Nodes and do Multi-hop Transfer](https://www.fiber.world/docs/quick-start/multi-hop-transfer)
- Whenever needed, check the [Network Resources](https://www.fiber.world/docs/quick-start/network-resources)

### What to practice

1. confirm your node is connected and synced
2. open a channel with a peer
3. verify the channel state
4. send an invoice and complete a payment
5. inspect the payment result from the node

### Success checkpoint

You can describe, in your own words, how a payment moves from invoice to final settlement through a channel.

---

## Stage 3: SDK for Developing Apps (2–4 hours)

**Goal:** Move from CLI-based experimentation to building an application with Fiber.

At this stage, the main question is no longer “how do I run a node?” but “how do I connect an app to Fiber?”

### Recommended path

- quickly go through the [SDK overview](https://www.fiber.world/docs/build/sdk)
- learn the basics from the [JavaScript SDK guide](https://www.fiber.world/docs/build/sdk/fiber-js)
- Use the Javascript SDK to build a [simple game with Fiber micro-payments](https://www.fiber.world/docs/build/simple-game)

### What to learn

The official SDK docs describe a JavaScript/TypeScript client for connecting to an existing Fiber node over HTTP. In practice, this means:

- installing `@ckb-ccc/fiber`
- creating a `FiberSDK` instance with your node RPC endpoint
- checking node state with `getNodeInfo()`
- opening channels, creating invoices, and sending payments from your app code
- understanding the two-party model: one side opens, the other accepts; one side invoices, the other pays

A good mental model is:

- the node provides the protocol runtime
- the SDK gives your app a typed interface to that runtime
- your application logic sits above them both

### What to practice

1. connect a small app or script to a running node
2. create an invoice and pay it from code
3. inspect node state and channel data from the SDK
4. compare the SDK workflow with the CLI workflow to understand where each is useful

### Success checkpoint

You can write a small script or prototype that connects to a Fiber node and performs a basic invoice/payment flow through the SDK.

---

## Stage 4: Production Readiness (next steps)

**Goal:** Prepare for real usage, not just a local demo.

At this point, you should combine application design with operational planning. Think about:

- whether your app needs fast micro-payments, on-chain finality, or both
- whether users will run their own node or whether your app will operate one on their behalf
- whether this is a browser app, a backend service, or an agent-based workflow
- how to manage liquidity, channel lifecycle, [backups](https://www.fiber.world/docs/operate/backup) and key security

This is the phase where you move from “hello world” to something that could support a real prototype or a production-style integration.

### Success checkpoint

You can describe the operational risks and integration choices for running a node or embedding Fiber into an application.

---

## Where to go next

- [resources.md](./resources.md) for available tools and reference links
- [tutorials/fiber-l402.md](./tutorials/fiber-l402.md) for paywall-style integration ideas
- [tutorials/fiber-game.md](./tutorials/fiber-game.md) for a practical demo flow

If you get stuck, the official docs and the CKB builder community are the best places to continue from here.
