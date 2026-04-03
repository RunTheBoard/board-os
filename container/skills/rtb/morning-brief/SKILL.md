# Morning Brief

Generate a daily morning brief that gets the operator up to speed in 60 seconds.

## Context Needed

Before generating, read:
- `memory/MEMORY.md` — business overview, key metrics, active projects
- `memory/projects/` — all active project files
- Latest dated memory file (`memory/YYYY-MM-DD.md`) — yesterday's activity
- Any calendar or scheduling data available

## Steps

1. **Scan memory** for active projects, pending decisions, upcoming deadlines
2. **Identify what changed** since the last brief (new data, completed tasks, blockers)
3. **Prioritize** — lead with what needs attention TODAY, not a general status dump
4. **Flag risks** — anything off-track, overdue, or about to become urgent
5. **Recommend actions** — specific next moves, not vague suggestions

## Output Format

```
☀️ *Morning Brief — [Date]*

*Needs Your Attention*
• [Most urgent item — what it is, why it matters, recommended action]
• [Second priority if applicable]

*In Progress*
• [Project/task] — [status, who's on it, ETA]
• [Project/task] — [status]

*Pipeline*
• [Upcoming items, scheduled deliverables]

*Numbers* (if available)
• [Key metric] — [value, trend]

*Today's Play*
[1-2 sentence recommendation for how to spend the day]
```

Keep it scannable. No fluff. If there's nothing urgent, say so — "Clean board today. Here's what's moving."

## Capture to Memory

After generating:
- Log the brief date to memory so you don't repeat items
- If the operator responds with updates or corrections, capture those immediately
- Track which items persist across briefs (potential stuck items)
