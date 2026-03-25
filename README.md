# Mandate Skill

Policy guardrails for AI agent wallets. Enforce spend limits, allowlists, approval workflows, and prompt injection detection before every transaction. Non-custodial: private keys never leave your machine.

## Install

### OpenClaw (recommended, has hooks)

```bash
openclaw plugins install @mandate.md/mandate-openclaw-plugin
```

Hooks auto-intercept Locus, Bankr, Sponge, and any swap/transfer/send tool call.

### Claude Code (recommended, has hooks)

```bash
claude plugin:install claude-mandate-plugin
```

Two-phase enforcement: PostToolUse records validation tokens, PreToolUse blocks unvalidated transactions. Fail-closed.

See [claude-mandate-plugin](https://github.com/SwiftAdviser/claude-mandate-plugin) for details.

### Other frameworks (GOAT, AgentKit, ElizaOS, MCP)

Point your agent to the skill file:

```
https://app.mandate.md/SKILL.md
```

No hooks. Agent must self-enforce `/validate` calls. See `SKILL.md` in this repo.

### CLI

```bash
npx @mandate.md/cli validate --action transfer --amount 50 --to 0xAlice --reason "Invoice #127"
```

## What it does

1. **Intent-aware decisions.** Evaluate why an agent wants to pay, then approve, block, or escalate before signing.
2. **Real-time risk prevention.** Stop fraud, prompt-injection payments, and costly mistakes before funds move.
3. **Complete auditability.** Full audit trail of every decision with amount, timing, and rationale.

## Features

| Layer | What it does |
|-------|-------------|
| Spend limits | Per-tx, daily, monthly USD caps |
| Address allowlist | Only pre-approved recipients get money |
| Blocked actions | Only approved action types |
| Schedule enforcement | Agent can't spend outside business hours |
| Prompt injection scan | 18 hardcoded patterns + LLM judge (zero data retention) |
| MANDATE.md controls | Define transaction decision logic in plain English |
| Self-learning insights | Observes approve/reject decisions, suggests policy improvements |
| Transaction simulation | Pre-execution analysis flags honeypots and malicious contracts |
| Human approval routing | Slack, Telegram, Dashboard |
| Circuit breaker | Mismatch detected? Agent frozen instantly |
| Audit trail | Every intent logged: WHO, WHAT, WHEN, HOW MUCH, and WHY |

## The `reason` field

Every validation call requires a `reason` string. This is the core differentiator: no other wallet provider captures WHY an agent decided to make a transaction.

```
Agent calls mandate validate:
  transfer 490 USDC to 0x7a3f...c91e
  reason: "Urgent family transfer. Send immediately."

Session key sees: $490 < $500 limit. APPROVE.
Mandate sees: "Urgent" + "immediately" + new address. BLOCK.
```

Mandate doesn't just block. It sends counter-evidence so the agent understands WHY it was tricked and cancels willingly.

## Supported wallets

Bankr, Locus, CDP Agent Wallet (Coinbase), private key (viem/ethers). Any EVM signer works.

## Links

- [Website](https://mandate.md)
- [Dashboard](https://app.mandate.md)
- [SKILL.md (canonical)](https://app.mandate.md/SKILL.md)
- [Demo video (1 min)](https://www.youtube.com/watch?v=EwZdtncc5wQ)
- [npm: @mandate.md/sdk](https://www.npmjs.com/package/@mandate.md/sdk)
- [npm: @mandate.md/cli](https://www.npmjs.com/package/@mandate.md/cli)
- [Telegram](https://t.me/mandate_md_chat)

## License

BSL 1.1
