# Tutorial: Build an L402 Paywall with Fiber

This tutorial walks through the [fiber-l402](https://github.com/RetricSu/fiber-l402) demo: a pay-per-view blog that uses Fiber Network for HTTP 402 (Payment Required) payments. You will learn how L402 works, how to run the demo, and how to adapt it for your own API or content paywall.

---

## What You Will Build

A blog where each article has a price in CKB. When a visitor requests paid content, the server responds with `402 Payment Required` and an L402 challenge. The visitor pays the invoice via Fiber, sends the proof back, and receives the content.

---

## What is L402?

[L402](https://www.x402.org/) is a protocol for "paid HTTP." It turns the HTTP `402 Payment Required` status code into a machine-readable payment challenge:

1. Client requests a paid resource.
2. Server responds with `402` and a `WWW-Authenticate: L402` header containing:
   - A **macaroon** — a verifiable token describing access rights.
   - An **invoice** — a Fiber payment request.
3. Client pays the invoice and obtains a **preimage** (proof of payment).
4. Client retries the request with `Authorization: L402 <macaroon>:<preimage>`.
5. Server verifies the preimage and returns the content.

Fiber's low-cost, instant payments make L402 practical for micropayments as small as fractions of a cent.

---

## Two Ways to Build This

There are two reference implementations in the ecosystem:

| Approach | Demo | Complexity | When to Use |
|----------|------|------------|-------------|
| **Application-level L402** | `fiber-l402` (this tutorial) | Higher — custom middleware, macaroons, invoice/verify/settle logic | You want full control or need to support older Fiber nodes |
| **Native x402 module** | `fiber-x402-blog` | Lower — delegates to Fiber node's built-in endpoints | You can run the [fiber#1301](https://github.com/nervosnetwork/fiber/pull/1301) branch |

This tutorial covers the application-level approach because it teaches the mechanics. If you are starting a new project and can use the x402 branch, prefer [fiber-x402-blog](https://github.com/RetricSu/fiber-x402-blog).

---

## Prerequisites

- Node.js 20+
- pnpm 9+
- A running Fiber node (testnet is fine)
- Some CKB in your node's wallet to create and pay invoices

---

## Step 1: Clone and Install

```bash
git clone https://github.com/RetricSu/fiber-l402.git
cd fiber-l402
pnpm install
```

Project layout:

```
fiber-l402/
├── apps/
│   ├── proxy/           # Express API + custom L402 middleware
│   └── web/             # Astro + React frontend
├── docs/
│   └── fiber-sdk.md     # Fiber SDK usage notes
└── e2e/                 # Playwright E2E tests
```

---

## Step 2: Configure Environment

Copy the example environment file:

```bash
cp .env.example .env
```

Edit `.env`:

```env
PORT=3001
L402_ROOT_KEY=<64-char-hex>
ARTICLE_PRICE_CKB=0.1
L402_EXPIRY_SECONDS=3600
FIBER_RPC_URL=http://127.0.0.1:8229

WEB_URL=http://localhost:4321
PROXY_URL=http://localhost:3001
```

Generate a random root key for macaroon signing:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

`ARTICLE_PRICE_CKB` sets the price per article. `L402_EXPIRY_SECONDS` controls how long a paid session remains valid.

---

## Step 3: Run the App

```bash
pnpm dev
```

This starts:

- **Web frontend** at `http://localhost:4321`
- **Proxy API** at `http://localhost:3001`

Open the web app, pick an article, and click **Unlock**. The frontend will guide you through paying the invoice.

---

## Step 4: Understand the L402 Flow

The payment flow is implemented in `apps/proxy/`:

```
GET /api/articles/:id/content
  → 402 WWW-Authenticate: L402 macaroon=..., invoice=...
  → User pays invoice with Fiber (in browser or external wallet)
  → retry with Authorization: L402 <macaroon>:<preimage>
  → 200 + full article content
```

### Backend responsibilities

- `MacaroonService` — mint and verify L402 macaroons.
- `createL402Middleware()` — Express middleware that intercepts paid routes.
- `FiberRpcClient` — talk to the merchant Fiber node to generate invoices and verify/settle payments.

### Frontend responsibilities

- Request paid content.
- Parse the `402` challenge.
- Pay the invoice via the user's Fiber node.
- Retry with the preimage.
- Cache unlocked content locally.

---

## Step 5: Read the Key Code

| File | Purpose |
|------|---------|
| `apps/proxy/src/` | Express server, L402 middleware, article routes |
| `apps/proxy/src/l402/` | Macaroon service and payment verification |
| `apps/web/src/` | Astro pages, React components, article content |
| `apps/web/src/pages/api/` | Astro API routes that forward to the proxy |
| `docs/fiber-sdk.md` | Notes on using `@fiber-pay/sdk` |

Key primitives from `@fiber-pay/sdk/node`:

```typescript
import {
  createL402Middleware,
  FiberRpcClient,
  MacaroonService,
} from '@fiber-pay/sdk/node';
```

---

## Step 6: Test It

Run unit and integration tests:

```bash
pnpm test
```

For end-to-end browser tests, use Playwright:

```bash
cd e2e
pnpm exec playwright test
```

Make sure you have a funded Fiber node running before running E2E tests.

---

## Step 7: Adapt It for Your Hackathon

Ideas for customizations:

- **API paywall:** Protect any REST endpoint instead of articles.
- **Metered billing:** Charge per API call or per token generated.
- **Subscriptions:** Issue macaroons with longer expiry and recurring payments.
- **Agent payments:** Let an AI agent pay for data access automatically.
- **Dynamic pricing:** Adjust price based on demand or content length.

---

## Native x402 Alternative

If you want a simpler implementation and can run the [fiber#1301](https://github.com/nervosnetwork/fiber/pull/1301) branch, use [fiber-x402-blog](https://github.com/RetricSu/fiber-x402-blog).

Comparison:

| | `fiber-l402` | `fiber-x402-blog` |
|---|--------------|-------------------|
| L402 implementation | Application-level via SDK | Native x402 module in Fiber node |
| Backend complexity | Express proxy with custom middleware | Astro API routes only |
| Fiber node requirement | Standard node `0.8.x` | Node with x402 PR |
| Code size | Larger | Smaller |
| Best for | Learning / full control | New projects / rapid prototyping |

The x402 demo uses Astro API routes to forward invoice, verify, and settle requests to the Fiber node's built-in x402 endpoints. The merchant node's RPC URL stays server-side, and the payer node can be any standard Fiber node.

---

## Production Considerations

Before deploying a paywall to production:

1. **Secure your root key.** `L402_ROOT_KEY` is used to sign macaroons. Store it in a secret manager, not in `.env` in version control.
2. **Validate preimages server-side.** Never trust the client; always ask the Fiber node whether the invoice was paid.
3. **Rate-limit the 402 endpoint.** Prevent abuse and invoice spam.
4. **Handle invoice expiry.** If a user does not pay before the invoice expires, they must request a new challenge.
5. **Use HTTPS.** L402 credentials and preimages should not travel over plaintext.

---

## Common Issues

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| "402" keeps appearing | Preimage not sent or invalid | Check the browser console for the retry request |
| "Invoice creation failed" | Node has no outbound liquidity | Open a channel and fund both sides |
| "Macaroon verification failed" | Wrong `L402_ROOT_KEY` or expired macaroon | Regenerate key, clear cookies/localStorage, retry |
| Frontend cannot pay | CORS or wrong `FIBER_RPC_URL` | Ensure the browser can reach the node RPC and COOP/COEP headers are set if using WASM |

---

## Next Steps

- Read the [fiber-game tutorial](./fiber-game.md) to learn hold-invoice gaming patterns.
- Review the [Resources page](../resources.md) for SDKs and example projects.
- Experiment with the [fiber-x402-blog](https://github.com/RetricSu/fiber-x402-blog) native x402 demo.

---

*Last updated: 2026-06-18*
