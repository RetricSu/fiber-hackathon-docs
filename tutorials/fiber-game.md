# Tutorial: Build a Two-Player Game on Fiber

This tutorial walks through the [fiber-game](https://github.com/quake/fiber-demo/tree/main/fiber-game) demo: a decentralized rock-paper-scissors and guess-number game built on CKB Fiber. You will learn how to run the demo, understand the hold-invoice gaming patterns, and adapt it for your own idea.

---

## What You Will Build

A two-player game where:

- Both players stake CKB into a Fiber hold invoice.
- Each player commits to a move without revealing it.
- An oracle determines the winner.
- The winner claims the staked funds; the loser gets a refund.

The cryptography (commitments + adaptor signatures) enforces fair play without a trusted escrow.

---

## Concepts You Will Learn

- **Hold invoices:** Invoices that lock funds until a preimage is revealed.
- **Commitment scheme:** Players commit to moves before revealing them.
- **Adaptor signatures:** The oracle signs one of several possible outcomes; only the winning player can complete the signature.
- **Frontend-driven Fiber integration:** The backend never calls Fiber RPC; the browser talks directly to each player's own node.

---

## Prerequisites

- Rust 1.75+ ([rustup](https://rustup.rs/))
- `curl`, `tar`, `jq` (used by the setup script)
- Two CKB testnet accounts with some CKB from the [faucet](https://faucet.nervos.org/)
- Basic familiarity with running a Fiber node (see [Onboarding Workflow](../onboarding.md))

---

## Step 1: Clone the Demo

```bash
git clone https://github.com/quake/fiber-demo.git
cd fiber-demo
```

The demo has two projects:

- `fiber-game/` — two-player games
- `fiber-escrow/` — escrow trading (not covered here)

---

## Step 2: Start Two Fiber Testnet Nodes

The repo includes a setup script that automates the boring parts.

```bash
./scripts/setup-fiber-testnet.sh
```

The script does the following:

1. Downloads `fnn` v0.7.0 and `ckb-cli` v2.0.0.
2. Creates two CKB accounts.
3. Prints addresses for funding via the [CKB Faucet](https://faucet.nervos.org/).
4. Waits for funding.
5. Starts two local Fiber nodes:
   - Node A: `http://127.0.0.1:8227`
   - Node B: `http://127.0.0.1:8229`
6. Opens a 500 CKB channel between them.

Other useful commands:

```bash
./scripts/setup-fiber-testnet.sh status  # Check node and channel status
./scripts/setup-fiber-testnet.sh stop    # Stop running nodes
```

---

## Step 3: Run the Combined Demo

The easiest way to run the game is the combined service, which bundles the Oracle and both Player UIs on one port.

```bash
cd fiber-game/crates/fiber-game-demo
FIBER_PLAYER_A_RPC_URL=http://localhost:8227 \
FIBER_PLAYER_B_RPC_URL=http://localhost:8229 \
cargo run
```

Open `http://localhost:3000` in two browser windows. Use the **Player selector** dropdown to switch between Player A and Player B.

Try a game of rock-paper-scissors:

1. Player A creates a game and stakes some CKB.
2. Player B joins with the same stake.
3. Both players choose a move.
4. Both reveal.
5. The oracle declares a winner and reveals the loser's preimage to the winner.

---

## Step 4: Understand the Architecture

The most important design decision in this demo is that the **backend makes zero Fiber RPC calls**. All Fiber interactions happen in the browser.

```
Player A Browser ◄────► Backend (Oracle + Game State) ◄────► Player B Browser
       │                        (no Fiber RPC)                │
       │                                                      │
       ▼                                                      ▼
  Fiber Node A                                          Fiber Node B
```

### Backend responsibilities

- Game state management (create, join, reveal, result)
- Oracle logic (adaptor signatures, winner determination)
- Preimage/payment hash exchange between players

### Frontend responsibilities

- `new_invoice` — create a hold invoice on the player's own node
- `send_payment` — pay the opponent's invoice
- `settle_invoice` — claim funds using the opponent's preimage (winner)
- `cancel_invoice` — cancel the hold invoice and refund (loser)
- `list_channels` — check balance

---

## Step 5: Follow the Payment Flow

Here is what happens under the hood during a game.

### 1. Preimage submission

Each player generates a random preimage, computes `payment_hash = hash(preimage)`, and submits **both** to the Oracle. The preimage stays secret until the game ends.

### 2. Cross-invoice creation

Each player creates a hold invoice on **their own** Fiber node using the **opponent's** `payment_hash`. This means only the opponent's preimage can settle it.

### 3. Mutual payment

Both players pay each other's invoices. Funds are locked, not yet transferred.

### 4. Reveal and outcome

Both players reveal their moves. The Oracle determines the winner and reveals the **loser's preimage** to the winner.

### 5. Settlement

- The winner uses the opponent's preimage to settle their own invoice on their own node.
- The loser cancels their invoice on their own node and gets a refund.

```
Player A                  Oracle                  Player B
    │                       │                        │
    │  1. Create game       │                        │
    │  (stake + move hash)  │                        │
    │──────────────────────►│                        │
    │                       │                        │
    │                       │  2. Join game          │
    │                       │  (stake + move hash)   │
    │                       │◄───────────────────────│
    │                       │                        │
    │  3. Adaptor sigs for  │  all outcomes          │
    │◄──────────────────────│───────────────────────►│
    │                       │                        │
    │  4. Reveal move       │  4. Reveal move        │
    │──────────────────────►│◄───────────────────────│
    │                       │                        │
    │  5. Oracle picks winner                       │
    │  6. Winner gets loser's preimage              │
    │  7. Winner claims prize via hold invoice      │
```

---

## Step 6: Run the Separate Services (Optional)

For production-like setups, run each service independently:

```bash
# Terminal 1: Oracle service
cd fiber-game/crates/fiber-game-oracle
cargo run

# Terminal 2: Player A UI
cd fiber-game/crates/fiber-game-player
PORT=3001 cargo run

# Terminal 3: Player B UI
cd fiber-game/crates/fiber-game-player
PORT=3002 cargo run
```

This is useful when players run on different machines.

---

## Step 7: Read the Code

Key files to study:

| File | Purpose |
|------|---------|
| `fiber-game-core/src/games/` | Game definitions (RPS, Guess Number) |
| `fiber-game-core/src/protocol/` | Game protocol state machine |
| `fiber-game-core/src/crypto/` | Commitments and signature points |
| `fiber-game-oracle/src/` | Oracle HTTP service |
| `fiber-game-player/src/` | Player UI service |
| `fiber-game-demo/src/` | Combined service wiring |

Run the tests:

```bash
# All tests
cargo test

# E2E game flow
cargo test --test e2e_game_flow -- --nocapture
```

---

## Step 8: Adapt It for Your own idea

Ideas for modifications:

- **New game:** Add tic-tac-toe or a card game in `fiber-game-core/src/games/`.
- **Different stake token:** Use a UDT instead of CKB.
- **Decentralized oracle:** Replace the trusted oracle with a multi-party or ZK-based resolution.
- **Tournament mode:** One oracle, many players, bracketed matches.
- **Mobile UI:** Replace the server-rendered player UI with a React/Vue frontend.

---

## Production Considerations

The demo uses a **trusted oracle** for simplicity. Before going to production:

1. **Integrate adaptor signatures fully.** The demo pre-computes signature points but does not yet use them for settlement. Wire the `signature_point.rs` logic into the payout flow.
2. **Verify invoice strings.** Parse the opponent's BOLT11/BOLT12 invoice and confirm the `payment_hash` matches what the Oracle gave you before paying.
3. **Add timeout and dispute logic.** Handle players who never reveal.
4. **Run your own stable nodes.** Browser users should connect to nodes they trust.

---

## Next Steps

- Read the [fiber-l402 tutorial](./fiber-l402.md) to learn paywall patterns.
- Explore the [Resources page](../resources.md) for tooling and community projects.
- Join the [Nervos Discord](https://discord.com/invite/BF9AJ4fzs6) if you get stuck.

---

*Last updated: 2026-06-18*
