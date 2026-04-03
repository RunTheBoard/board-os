# Weekly Report

Generate a weekly performance summary that shows momentum and surfaces what matters.

## Context Needed

Before generating, read:
- `memory/MEMORY.md` — KPIs, goals, baselines
- `memory/projects/` — all active project files
- Dated memory files from the past 7 days (`memory/YYYY-MM-DD.md`)
- Previous weekly reports (to show trends, not just snapshots)

## Steps

1. **Gather the week's activity** from memory files:
   - Tasks completed
   - Decisions made
   - New information learned
   - Blockers encountered and resolved (or not)

2. **Calculate progress against goals** if metrics are available:
   - Revenue/pipeline movement
   - Project milestones hit or missed
   - Customer/client updates

3. **Identify patterns:**
   - What's accelerating?
   - What's stuck?
   - What got dropped or deprioritized?

4. **Write the report** — this should feel like a sharp board memo, not a status dump

5. **Recommend next week's priorities** — max 3 items

## Output Format

```
*Weekly Report — Week of [Date]*

*Headline*
[One sentence: the most important thing about this week]

*Wins*
• [Concrete accomplishment with impact]
• [Concrete accomplishment]

*Progress*
• [Project] — [Where it was → where it is now]
• [Project] — [Status update]

*Numbers* (if available)
| Metric | This Week | Last Week | Trend |
|--------|-----------|-----------|-------|
| [KPI]  | [value]   | [value]   | ↑/↓/→ |

*Stuck*
• [What's not moving and why]
• [Recommended action to unblock]

*Next Week — Top 3*
1. [Highest priority — why it matters]
2. [Second priority]
3. [Third priority]

*Board Note*
[One insight or observation that isn't in the data — pattern recognition, strategic thought, or early warning]
```

## Capture to Memory

After delivery:
- Save report to `memory/projects/weekly-report-[date].md`
- Update baseline metrics in `memory/MEMORY.md`
- Note any operator corrections to data or priorities
- Track week-over-week trends for future reports
