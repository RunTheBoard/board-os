# BoardOS

**RTB's secure agent runtime for Run The Board.** Forked from [NanoClaw](https://github.com/nanoclaw/nanoclaw).

BoardOS powers the board — a team of virtual executives (COS, CMO, CFO, Developer, Secretary) that run business operations for RTB customers. Each customer gets their own isolated board instance with persistent memory that compounds over time.

## How It Works

1. **Customer sends a message** via Telegram (or WhatsApp/Slack/Discord)
2. **The COS (Chief of Staff) receives it** — reads business memory, loads the right skill, identifies gaps
3. **The right board member handles it** — marketing goes to CMO logic, finance to CFO, etc.
4. **Results are delivered** — directly in the conversation, ready to use
5. **Memory is updated** — every interaction makes the board smarter

The customer never sees infrastructure. They see one conversation with their Chief of Staff, and work gets done.

## Architecture

```
┌─────────────────────────────────────────────┐
│                  BoardOS                     │
│                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │ Customer  │  │ Customer  │  │ Operator │  │
│  │ Board A   │  │ Board B   │  │ Console  │  │
│  │ (Docker)  │  │ (Docker)  │  │ (Docker) │  │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  │
│       │              │              │        │
│  ┌────┴──────────────┴──────────────┴────┐  │
│  │           IPC / Host Runtime           │  │
│  │     (message routing, scheduling)      │  │
│  └────────────────┬──────────────────────┘  │
│                   │                          │
│  ┌────────────────┴──────────────────────┐  │
│  │          SQLite Store                  │  │
│  │   (messages, sessions, groups)         │  │
│  └───────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

### Per-Customer Isolation

Each customer board runs in its own Docker container with:
- **Own workspace** — memory files, brain database, conversation history
- **Read-only skill access** — skills are loaded but not modifiable by the container
- **No cross-board access** — customer A cannot see customer B's data
- **Resource limits** — CPU and memory caps per container

### The Harness (CLAUDE.md Templates)

The intelligence layer lives in `groups/global/CLAUDE.md` — this is the COS instruction set that makes every board smart about business operations. It defines:
- The Brain query → skill load → gap check → execute → capture loop
- Board member narration (COS, CMO, CFO, etc.)
- Memory management and compounding knowledge
- Brand voice and communication standards

### Skills

Skills live in `container/skills/rtb/` and encode best practices for specific business operations:
- `morning-brief/` — Daily operational briefing
- `ad-copy/` — Brand-aware advertising copy
- `brand-research/` — Market and competitive research
- `weekly-report/` — Performance summary and planning

Skills are RTB-controlled. No community or third-party skills. This is a security boundary and a quality boundary.

## Security Model

- **Container isolation** — each board is sandboxed via Docker
- **No community skills** — only RTB-authored skill files are deployed
- **No code execution from customers** — the board runs pre-approved operations only
- **Workspace separation** — customer data never crosses board boundaries
- **Credential isolation** — API keys and tokens are per-instance

## Deployment (Hetzner VPS)

### Prerequisites

- Hetzner VPS (CX22 or higher recommended)
- Ubuntu 22.04+
- Docker installed
- Node.js 20+
- Anthropic API key

### Setup

```bash
# Clone BoardOS
git clone https://github.com/RunTheBoard/board-os.git /opt/board-os
cd /opt/board-os

# Install dependencies
npm install

# Build
npm run build

# Provision a customer board
./scripts/provision-board.sh acme-corp "Acme Corporation" "telegram-bot-token"

# Configure .env with your Anthropic API key
# Then start
npm start
```

### Provisioning a New Customer

```bash
./scripts/provision-board.sh <slug> "<name>" "<telegram-token>"
```

This creates the customer workspace from template, initializes the brain database, and outputs next steps for DNS and Telegram bot setup.

## Development

```bash
# Run in dev mode
npm run dev

# Type checking
npm run typecheck

# Tests
npm run test

# Format
npm run format
```

## Project Structure

```
board-os/
├── src/                    # NanoClaw core (DO NOT MODIFY — keep mergeable)
├── container/
│   ├── agent-runner/       # Container agent runner
│   └── skills/rtb/        # RTB skill library
├── groups/
│   ├── global/CLAUDE.md    # COS harness (the core product)
│   └── main/CLAUDE.md      # Operator console
├── templates/
│   └── customer-workspace/ # New customer template
├── scripts/
│   └── provision-board.sh  # Customer provisioning
├── setup/                  # NanoClaw setup utilities
├── docs/                   # Documentation
└── store/                  # SQLite database (runtime)
```

## Attribution

BoardOS is a fork of [NanoClaw](https://github.com/nanoclaw/nanoclaw), an open-source personal assistant runtime. All original NanoClaw code retains its license and attribution. See [LICENSE](LICENSE) for details.

## License

See [LICENSE](LICENSE).
