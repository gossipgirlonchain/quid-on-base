# Quid on Base

**A personal money agent, on Base.** Quid watches your cash flow, advances you stablecoin against verified income before payday, and repays itself the moment you're paid — building an on-chain reputation that makes every advance cheaper.

This is the Base build of [Quid](https://github.com/gossipgirlonchain/quid), originally built on Casper for the Casper Agentic Buildathon 2026. Same agent, same loop — rebuilt for Base's rails: native USDC, Solidity, and Coinbase's onchain stack.

| | |
|---|---|
| 🧬 **Origin** | https://github.com/gossipgirlonchain/quid (Casper build, live on Testnet) |
| 🌐 **Landing** | https://quid.fund |
| 🎬 **Demo video (Casper build)** | https://www.youtube.com/shorts/FGolL0duPOk |

---

## The loop

Most "money before payday" apps are centralized black boxes. Quid is an **autonomous agent** that runs a tight loop on your behalf:

```
perceive → verify → decide → act → settle
```

1. **Perceive** — reads your balance, recurring bills, and income via Plaid; projects whether you'll go short before payday.
2. **Verify** — pays per-call over x402 to verify the income it would lend against.
3. **Decide** — computes a safe advance = min(plan cap, what your next paycheck can repay). It **declines** rather than over-lend.
4. **Act** — signs `issueAdvance` from its own wallet; USDC moves from the pool to your wallet.
5. **Settle** — when wages land, it `repayAdvance`s itself automatically and your on-chain **Quid score** ticks up, so the next advance is cheaper.

The agent is the protagonist. It holds its own wallet, decides, signs, and acts — the person just benefits.

---

## Why Base

- **Native USDC** — no test-token stand-in; the advance rail is the real dollar rail.
- **x402 is home turf** — Coinbase's x402 pay-per-call protocol is first-class on Base, so the income-verification step uses the network's own machinery.
- **Agent wallets** — Coinbase Developer Platform (CDP) server wallets / smart wallets give the agent a proper key-management story.
- **Cheap, fast blocks** — advance issuance and repayment are sub-cent, which is what a micro-advance product needs.

## Porting map (Casper → Base)

| Casper build | Base build |
|---|---|
| QuidPool contract (Odra / Rust, CEP-18) | `QuidPool.sol` (Solidity / Foundry, ERC-20 USDC) |
| CES events, read over JSON-RPC | EVM events, read over viem |
| casper-js-sdk v5 agent signer | viem + CDP server wallet |
| dUSDC test token | Base Sepolia USDC (Circle faucet) |
| cspr.live explorer links | BaseScan links |

## Planned layout

```
contracts/   QuidPool.sol + Foundry tests
agent/       the money agent: Plaid signals → decide → sign on Base
app/         the consumer app (Activity feed reads QuidPool events)
site/        landing
```

## Status

🚧 Fresh repo — porting from the Casper build. First milestone: `QuidPool.sol` deployed to **Base Sepolia** with the full issue → repay → reputation lifecycle covered by Foundry tests.
