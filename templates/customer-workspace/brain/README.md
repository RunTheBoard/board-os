# Brain

The Brain is your board's structured knowledge base. While `memory/` files store running context and learnings, the Brain provides queryable, structured data.

## How It Works

The Brain is a SQLite database (`brain.db`) that stores:
- **Facts** — Verified business facts (revenue numbers, dates, contacts)
- **Decisions** — Recorded decisions with rationale and date
- **Insights** — Patterns and observations the board has identified over time
- **Relationships** — Connections between entities (people, companies, projects)

## Schema

```sql
CREATE TABLE facts (
  id INTEGER PRIMARY KEY,
  category TEXT NOT NULL,
  key TEXT NOT NULL,
  value TEXT NOT NULL,
  source TEXT,
  confidence REAL DEFAULT 1.0,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE decisions (
  id INTEGER PRIMARY KEY,
  topic TEXT NOT NULL,
  decision TEXT NOT NULL,
  rationale TEXT,
  decided_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  revisit_at DATETIME
);

CREATE TABLE insights (
  id INTEGER PRIMARY KEY,
  category TEXT NOT NULL,
  insight TEXT NOT NULL,
  evidence TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

## Usage

The board reads from and writes to the Brain automatically during conversations. You don't need to interact with it directly — it works behind the scenes to make your board smarter over time.

## Data Safety

- The Brain is local to your board instance
- No data leaves your environment
- Backups are taken daily
