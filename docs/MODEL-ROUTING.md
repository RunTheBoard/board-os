# Model Routing — BoardOS Cost Management

## How It Works

BoardOS uses two models via the Claude Agent SDK's built-in model routing:

| Model | Env Variable | Used For | Cost |
|-------|-------------|----------|------|
| **Sonnet** | `CLAUDE_MODEL` | Primary reasoning, creative work, strategy, customer conversations | ~$3/$15 per 1M tokens (in/out) |
| **Haiku** | `CLAUDE_SMALL_FAST_MODEL` | Tool calls, file edits, quick lookups, lightweight operations | ~$0.80/$4 per 1M tokens (in/out) |

The Claude Agent SDK automatically routes to the small/fast model for operations that don't need full reasoning (file reads, bash commands, simple edits). The primary model handles customer-facing responses and complex tasks.

## Typical Customer Cost Breakdown

Based on ~20 interactions/day with a mix of quick questions and real work:

```
Sonnet (conversations, strategy, deliverables):  ~$25-45/mo
Haiku (tool calls, file ops, quick lookups):      ~$5-15/mo
─────────────────────────────────────────────────
Total per customer:                                ~$30-60/mo
```

### By Tier Margin

| Tier | Price | Est. API Cost | Infra ($5) | Margin |
|------|-------|--------------|------------|--------|
| The Core ($149) | $149/mo | ~$40/mo | $5/mo | $104 (70%) |
| The Full Board ($299) | $299/mo | ~$60/mo | $5/mo | $234 (78%) |
| The Boardroom ($999) | $999/mo | ~$80/mo | $5/mo | $914 (92%) |

## What Goes to Haiku Automatically

The Claude Agent SDK routes these to `CLAUDE_SMALL_FAST_MODEL`:
- File read/write/edit operations
- Bash command execution
- Glob/grep searches
- Simple tool invocations
- Internal reasoning steps that don't produce customer-facing output

## What Stays on Sonnet

Everything that requires reasoning or customer-facing quality:
- Responding to customer messages
- Strategy and analysis
- Creative work (ad copy, content)
- Multi-step task planning
- Decision recommendations

## When to Consider Opus

For alpha, Sonnet handles 95%+ of work well. Opus ($15/$75 per 1M tokens) should only be considered for:
- Complex multi-document analysis
- High-stakes strategic recommendations
- Tasks where Sonnet consistently underperforms

To use Opus for a specific customer, change `CLAUDE_MODEL` in their `.env`:
```
CLAUDE_MODEL=claude-opus-4-20250514
```

This is a per-VPS setting — one customer can run Opus while others stay on Sonnet.

## Scheduled Task Cost Optimization

Scheduled tasks (morning briefs, sweeps) are the biggest cost risk because they fire whether or not there's useful work to do.

**Always use script gates for recurring tasks:**

```bash
# Good: script checks if there's anything to report
script: |
  # Check if any new leads came in
  node -e "
    const count = checkNewLeads();
    console.log(JSON.stringify({ wakeAgent: count > 0, data: { newLeads: count } }));
  "
prompt: "Generate morning brief with the new lead data."

# Bad: agent wakes up every morning regardless
prompt: "Check if there are new leads and generate a brief."
```

The script runs in milliseconds at zero token cost. The agent only wakes (and spends tokens) when there's real work.

## Monitoring

Track per-customer API spend by checking Anthropic dashboard usage and correlating with customer activity logs in `groups/{customer}/logs/`.

Future: Add token counting middleware to the agent-runner for per-customer usage tracking.

## Configuration

Set in each customer's `.env`:

```bash
# Default routing (recommended for most customers)
CLAUDE_MODEL=claude-sonnet-4-20250514
CLAUDE_SMALL_FAST_MODEL=claude-haiku-3-5-20241022

# For high-value Boardroom customers who need max quality
# CLAUDE_MODEL=claude-opus-4-20250514

# API key — RTB managed, one key per VPS
ANTHROPIC_API_KEY=sk-ant-...
```
