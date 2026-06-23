# Fiber Developer Onboarding

Welcome to CKB Fiber. This guide is designed to help new developers go from “I have heard of Fiber” to “I can run a node, complete a real transfer, and start building on top of it.”

The goal is not to overwhelm you with protocol detail on day one. The goal is to help you reach the first meaningful milestone as quickly as possible: a working node, a successful payment, and a clear sense of where to go next.

> Primary reference: the official Fiber 2.0 documentation. This repository is a practical companion for orientation, demos, and hackathon-style learning.

---

## What you will learn

By the end of this path, you should be able to:

- explain what Fiber is and how it differs from traditional payment channels
- run a Fiber node on Testnet
- inspect node state and basic channel information
- complete a basic transfer flow
- decide how Fiber fits into your own project or demo

---

## Stage 0: Concepts Alignment (10–15 minutes)

**Goal:** Build a simple mental model before touching the tooling.

Start with these references:

- [What is Fiber Network?](https://www.fiber.world/docs)
- [How Fiber Network Works](https://www.fiber.world/docs/how-it-works)
- [Fiber Network Light Paper](https://github.com/nervosnetwork/fiber/blob/develop/docs/light-paper.md)
- [Glossary](https://github.com/nervosnetwork/fiber/blob/develop/docs/glossary.md)

Think through these three questions:

1. **Why does Fiber exist?**
   Read the overview and light paper to understand why Fiber is useful for fast, low-cost, repeated transfers and why off-chain channels matter.

2. **How do payment channels and multi-hop routing work?**
   Read the “How Fiber Network Works” page to understand how channels are opened, updated, and settled, and how payments can travel through intermediate nodes.

3. **How is Fiber different from Bitcoin Lightning?**
   Read the light paper and the overview together. The key difference is that Fiber is built on CKB, designed for multi-asset, cross chain hub and programmable payment flows, and aims to be more composable with CKB-native assets and smart contract logic.

### What to understand

- Fiber is not just “another wallet”; it is a network of payment channels.
- A channel is a shared state between two participants, not a permanent ledger entry for every payment.
- Routing depends on available liquidity, so a path may exist in theory but still fail in practice if the capacity is insufficient.
- The base-layer role of CKB is to anchor and enforce the channel state when channels open, update, or close.
- Compared with Bitcoin Lightning, Fiber is positioned as a CKB-native payment channel network with broader multi-asset, built-in cross chain hub and programmability goals.

### Success checkpoint

You should be able to answer these three questions in your own words: why Fiber exists, how the channel/routing model works, and how it differs from Bitcoin Lightning.

---

## Stage 1: Quick Start (30–60 minutes)

**Goal:** Reach the “aha” moment quickly by getting a node running and completing a first transfer.

### Recommended path

For most developers, the fastest route is:

1. follow the official quick-start guide for [Run a Fiber Node](https://www.fiber.world/docs/quick-start/run-a-node). Notes: use a pre-built binary first; build from source only if you need full control or custom development.
2. You can also use [docker-compose](https://github.com/Officeyutong/fiber-demo-startup) to start up 3 nodes on your local machine and use this frontend to give it a inspect what a smallest fiber network looks like.

### Basic setup checklist

Before you begin, make sure you have:

- Git
- a shell environment
- ckb-cli for key handling
- access to Testnet funds
- docker and docker-compose for the second path

### The minimum commands you should know



```bash
./fnn-cli info
./fnn-cli peer list_peers
./fnn-cli channel list_channels
./fnn-cli --help
```

These are the core commands you will come back to when inspecting your node, debugging issues, or checking basic state.

### Success checkpoint

You have a running node on Testnet, the node responds to basic inspection commands, and you have inspect the smallest local network for fiber in your machine.

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

## Stage 3: Core Features for Real Apps (2–4 hours)

**Goal:** Start thinking like an application developer rather than a protocol explorer.

Once the basic flow works, go deeper into the pieces that matter for actual products:

- invoices and payment lifecycle
- channel management and liquidity
- direct and routed payments
- stablecoin or UDT-based flows
- how Fiber fits into a browser app, backend service, or agent workflow

This is the point where you should connect the protocol to your own idea. Ask:

- Does my app need fast micropayments or on-chain finality?
- Will users run their own node, or will my app operate one on their behalf?
- Is this a browser app, a backend service, or an agent-based workflow?

### Success checkpoint

You can explain how invoices, channels, and routing fit into a simple user story or demo.

---

## Stage 4: Production Readiness (next steps)

**Goal:** Prepare for real usage, not just a local demo.

When your basic flow is stable, start thinking about operations:

- backup your node state and keys
- understand liquidity and channel management
- plan for security and key rotation
- decide whether your app should target Testnet or Mainnet
- think about deployment for a real service or routing node

This is the phase where you move from “hello world” to something that could support a real prototype or a production-style integration.

### Success checkpoint

You can describe the operational risks and requirements for running a node or integrating Fiber into an application.

---

## Suggested Learning Path for This Repo

If you are using this repository as a hackathon or workshop companion, we recommend this order:

1. read this onboarding guide
2. follow the official quick-start docs to get a working node
3. use the tutorials in this repository for a concrete demo pattern
4. choose one production topic to study before shipping anything

---

## Common Pitfalls

- forgetting that amounts are expressed in shannons
- using the wrong network for funding
- assuming a payment will work without enough liquidity
- treating the backend as the place where Fiber RPC should live in a browser app without a clear reason
- upgrading node software without understanding storage and channel-state implications

---

## Where to go next

- [README](./README.md) for the repo overview
- [resources.md](./resources.md) for tools and reference links
- [tutorials/fiber-game.md](./tutorials/fiber-game.md) for a practical demo flow
- [tutorials/fiber-l402.md](./tutorials/fiber-l402.md) for paywall-style integration ideas

If you get stuck, the official docs and the Nervos community are the best places to continue from here.
