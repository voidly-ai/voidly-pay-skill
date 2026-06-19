# Voidly Pay — Claude Code Skill

A Claude [Agent Skill](https://support.claude.com/en/articles/12512176-what-are-skills) for [Voidly Pay](https://voidly.ai/pay) — agent-to-agent payments over HTTP 402 (x402), USDC-backed, live on Base mainnet.

## What this skill does

Teaches Claude how to use the Voidly Pay rail to:

- **Charge for an API** without writing payment code (universal reverse proxy)
- **Monetize an AI service** — wrap inference in a 402 paywall
- **Hire another agent** — capability-search, escrow, signed receipt
- **Pay another agent** — auto-pay any HTTP 402 quote
- Set up subscriptions / streams for per-token or per-second metering

Skill format: a single `SKILL.md` with YAML frontmatter, per the [Anthropic Agent Skills spec](https://github.com/anthropics/skills/tree/main/spec).

## Install

### Option 1 — drop into your `~/.claude/skills/`

```bash
cd ~/.claude/skills/
git clone https://github.com/voidly-ai/voidly-pay-skill.git voidly-pay
```

Restart Claude Code. The skill loads automatically.

### Option 2 — clone into a project

```bash
mkdir -p .claude/skills
cd .claude/skills
git clone https://github.com/voidly-ai/voidly-pay-skill.git voidly-pay
```

Commit `.claude/skills/voidly-pay/` to your repo so anyone who clones gets the skill.

### Option 3 — also install the MCP server

For the full surface (28 dedicated Pay tools), add to your MCP host config (`.mcp.json`, `~/.cursor/mcp.json`, etc.):

```json
{
  "mcpServers": {
    "voidly-pay": {
      "command": "npx",
      "args": ["-y", "@voidly/pay-mcp"]
    }
  }
}
```

The MCP server mints an Ed25519 keypair on first run (`~/.voidly-pay/keypair.json`, mode 0600). That keypair is the agent's identity. **Private keys never leave your machine.**

## Try it without installing

Browser demo, no setup: https://huggingface.co/spaces/emperor-mew/voidly-pay

Claim a DID + 10 starter credits, paywall any URL, watch a real settlement land on the rail.

## Trust posture

- **USDC-backed:** Stage 2 vault on Base mainnet at [`0xd25d3c6f32886b65356cc5c700382a8a02d84df5`](https://basescan.org/address/0xd25d3c6f32886b65356cc5c700382a8a02d84df5). [Sourcify-verified](https://repo.sourcify.dev/contracts/full_match/8453/0xd25d3c6f32886b65356cc5c700382a8a02d84df5/), non-upgradable.
- **Public reserves:** https://voidly.ai/pay/proof (vault USDC ≥ Σ(stage2_credits), refreshed every 15s)
- **Caps:** $100 daily, $10 per-tx
- **OpenAPI 3.1:** https://api.voidly.ai/v1/pay/openapi.json

## Other ways to integrate

- **Cookbook (runnable recipes):** https://github.com/voidly-ai/voidly-pay-cookbook
- **Scaffolder:** `npx create-voidly-agent my-agent` (4 templates)
- **Drop-in middleware:** Hono, Express, FastAPI, Flask
- **Framework toolkits:** LangChain, Vercel AI SDK, CrewAI, AutoGen, LlamaIndex
- **Comparison vs alternatives:** https://voidly.ai/pay/compare

## License

MIT. See [LICENSE.txt](./voidly-pay/LICENSE.txt).
