# Fiber Developer Onboarding Workflow

A progressive, task-based path for developers new to CKB Fiber. Complete each step before moving to the next. Most steps are designed to be short; the whole path should take a focused weekend.

---

## Milestone 0: Understand the Big Picture (1–2 hours)

**Goal:** Know what Fiber is, why it exists, and when to use it.

### Task 0.1 — Read the Light Paper
Read the [Fiber Light Paper](https://github.com/nervosnetwork/fiber/blob/develop/docs/light-paper.md) and note:

- How Fiber compares to Lightning Network.
- The role of payment channels, PTLC, and multi-hop payments.
- Why CKB's cell model makes Fiber "composable" with other scripts.

### Task 0.2 — Learn the Key Terms
Read the [Glossary](https://github.com/nervosnetwork/fiber/blob/develop/docs/glossary.md) and make sure you can explain:

- Channel, peer, funding transaction
- Invoice, hold invoice, payment hash, preimage
- PTLC vs HTLC
- Adaptor signature
- Liquidity and routing

### Task 0.3 — Decide Your Stack
Answer these questions for your hackathon idea:

1. Does your app need **fast micropayments** or **on-chain finality**?
2. Will users run their own Fiber node, or will you operate one on their behalf?
3. Is your frontend a browser app, a backend service, or an agent/MCP tool?

> **Checkpoint:** Write a one-paragraph project pitch that includes the word "channel" correctly.

---

## Milestone 1: Set Up Your Development Environment (2–3 hours)

**Goal:** Have a working Fiber testnet node and tooling.

### Task 1.1 — Install Dependencies
Install the following:

- [Rust](https://rustup.rs/) and Cargo (to build FNN from source)
- [Git](https://git-scm.com/)
- [ckb-cli](https://github.com/nervosnetwork/ckb-cli) for key management
- (Optional) [Node.js 20+](https://nodejs.org/) and [pnpm](https://pnpm.io/) if building JS/TS apps

### Task 1.2 — Build or Download FNN
Choose one:

**Option A: Build from source**
```bash
git clone https://github.com/nervosnetwork/fiber.git
cd fiber
cargo build --release
```

**Option B: Use a pre-built release**
Download the latest release from [github.com/nervosnetwork/fiber/releases](https://github.com/nervosnetwork/fiber/releases).

### Task 1.3 — Create a Node Directory
```bash
mkdir /folder-to/my-fnn
cp target/release/fnn /folder-to/my-fnn         # or downloaded binary
cp target/release/fnn-migrate /folder-to/my-fnn
cp target/release/fnn-cli /folder-to/my-fnn
cp config/testnet/config.yml /folder-to/my-fnn
cd /folder-to/my-fnn
```

### Task 1.4 — Create or Import a CKB Key
```bash
ckb-cli account new
mkdir ckb
ckb-cli account export --lock-arg <lock_arg> --extended-privkey-path ./ckb/exported-key
head -n 1 ./ckb/exported-key > ./ckb/key
rm ./ckb/exported-key
```

### Task 1.5 — Start the Node
```bash
FIBER_SECRET_KEY_PASSWORD='YOUR_PASSWORD' RUST_LOG='info' ./fnn -c config.yml -d .
```

Leave it running and watch the logs. You should see it sync with the testnet.

> **Checkpoint:** Run `./fnn-cli info` and confirm you see a `pubkey` and sync status.

---

## Milestone 2: Fund and Connect (2–3 hours)

**Goal:** Get testnet CKB, open a channel, and make a basic transfer.

### Task 2.1 — Get Testnet CKB
1. Copy your testnet address from `ckb-cli` or `./fnn-cli info`.
2. Request CKB from the [testnet faucet](https://faucet.nervos.org/).
3. Wait for the transaction to confirm on testnet.

### Task 2.2 — Connect to a Peer
Find a public testnet node or ask in the [Nervos Discord](https://discord.gg/BF9AJ4fzs6). Then connect:

```bash
./fnn-cli peer connect_peer \
  --peer-id <peer_pubkey> \
  --address /ip4/<ip>/tcp/<port>
```

Or via JSON-RPC:
```bash
curl -X POST http://127.0.0.1:8227 \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "connect_peer",
    "params": [{
      "address": "/ip4/<ip>/tcp/<port>/p2p/<peer_pubkey>"
    }]
  }'
```

### Task 2.3 — Open a Channel
```bash
./fnn-cli channel open_channel \
  --peer-id <peer_pubkey> \
  --funding-amount <shannons>
```

> Amounts are in **shannons**: `1 CKB = 10^8 shannons`.

### Task 2.4 — Make a Payment
Follow the [Basic Transfer Example](https://www.fiber.world/docs/quick-start/basic-transfer):

1. Create an invoice on the receiving node.
2. Pay the invoice from the sending node.
3. Verify the payment succeeded with `list_payments` or `get_payment`.

> **Checkpoint:** Successfully send at least one payment over a channel and confirm it with `list_payments`.

---

## Milestone 3: Use the SDK (3–4 hours)

**Goal:** Build a small script or web page that talks to your node.

### Task 3.1 — Install the SDK
```bash
pnpm add @fiber-pay/sdk
# For browser apps also add:
pnpm add @nervosnetwork/fiber-js
```

### Task 3.2 — Query Node Info from a Script
Create `node-info.ts`:

```typescript
import { FiberRpcClient } from '@fiber-pay/sdk';

const client = new FiberRpcClient({
  url: 'http://127.0.0.1:8227',
  biscuitToken: process.env.FIBER_RPC_BISCUIT_TOKEN,
});

async function main() {
  const info = await client.nodeInfo();
  console.log('pubkey:', info.pubkey);
}

main();
```

Run it:
```bash
npx tsx node-info.ts
```

### Task 3.3 — List Peers and Channels
Extend your script to print:

- Connected peers
- Open channels and their balances
- Recent payments

### Task 3.4 — Send a Payment from Code
Use the SDK to:

1. Create an invoice on a second node (or the same node).
2. Pay it programmatically.
3. Poll `get_payment` until it succeeds or fails.

> **Checkpoint:** Your script can print node info, list channels, and send a payment without using `fnn-cli`.

---

## Milestone 4: Run a Demo (4–6 hours)

**Goal:** Understand how a real Fiber app is architected.

### Task 4.1 — Set Up Two Local Nodes
Use the `fiber-demo` setup script:

```bash
git clone https://github.com/quake/fiber-demo.git
cd fiber-demo
./scripts/setup-fiber-testnet.sh
```

This starts two connected nodes (ports `8227` and `8229`) with a channel.

### Task 4.2 — Run the Game Demo
```bash
cd fiber-game/crates/fiber-game-demo
FIBER_PLAYER_A_RPC_URL=http://localhost:8227 \
FIBER_PLAYER_B_RPC_URL=http://localhost:8229 \
cargo run
```

Open two browser windows at `http://localhost:3000`, switch players, and play rock-paper-scissors.

### Task 4.3 — Answer These Architecture Questions
1. Why does the backend make **zero** Fiber RPC calls?
2. Where is each player's invoice created?
3. How does the oracle decide who receives the funds?
4. What prevents a player from refusing to reveal their move?

> **Checkpoint:** You can explain the frontend-driven Fiber integration pattern to another developer.

---

## Milestone 5: Build Your First Fiber Feature (1–2 days)

**Goal:** Ship a minimal but complete feature for your hackathon idea.

### Task 5.1 — Define the User Flow
Draw or write:

1. Who pays whom?
2. When is an invoice created?
3. What happens after payment succeeds?
4. What happens if payment fails or times out?

### Task 5.2 — Implement the Backend (if needed)
If your app needs state management, create a small HTTP service. Remember: **keep Fiber RPC calls out of the backend** unless you have a specific reason not to.

### Task 5.3 — Implement the Frontend
Use `@fiber-pay/sdk` or `@fiber-pay/sdk/browser` to:

- Query the user's node
- Create or pay invoices
- Handle success/failure states

### Task 5.4 — Test Edge Cases
- Node offline
- Insufficient channel balance
- Invoice expired
- User cancels payment

### Task 5.5 — Document Your Project
Write a short `README.md` with:

- What it does
- How to run it
- How to fund the testnet wallet
- Known limitations

> **Checkpoint:** A teammate can clone your repo, fund a wallet, and use your feature end-to-end.

---

## Optional Deep Dives

Pick one based on your project direction:

| Direction | Next Step |
|-----------|-----------|
| **AI agents / MCP** | Study `fiber-pilot` and `Fiber Agent Pay` from the hackathon. |
| **Paywalls / L402** | Follow the [fiber-l402 tutorial](./tutorials/fiber-l402.md) and then try [fiber-x402-blog](https://github.com/RetricSu/fiber-x402-blog). |
| **Gaming / betting** | Follow the [fiber-game tutorial](./tutorials/fiber-game.md) and adapt the protocol. |
| **Cross-chain swaps** | Read the cross-network sections of the Fiber repo and experiment with Lightning-compatible features. |
| **Production node ops** | Set up `fnn-cli` automation, backups, watchtower, and WSS configuration. |

---

## Common Pitfalls

1. **Forgetting shannons.** All Fiber amounts are in shannons. `100000000` = 1 CKB.
2. **Funding from the wrong network.** Make sure your faucet request targets testnet, not mainnet.
3. **Backend calling Fiber RPC.** In browser apps, the user's browser should talk to their own node. The backend should only manage app state.
4. **Ignoring channel liquidity.** A payment only succeeds if there is enough outbound capacity along the route.
5. **Upgrading without closing channels.** FNN storage format may change between versions; close channels before upgrading.

---

## Need Help?

- [Nervos Discord — Fiber channel](https://discord.gg/BF9AJ4fzs6)
- [Nervos Talk — English category](https://talk.nervos.org/c/english/49)
- [Fiber GitHub Issues](https://github.com/nervosnetwork/fiber/issues)

---

*Last updated: 2026-06-18*
