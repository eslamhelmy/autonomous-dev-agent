# Build Your Own Dev Agent with Claude Code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-blueviolet)](https://claude.ai/claude-code)
[![Lessons](https://img.shields.io/badge/Lessons-11-green)]()

A hands-on course for building a **persistent, autonomous developer agent** that runs on your machine using [Claude Code](https://claude.ai/claude-code). No frameworks. No cloud services. Just files, cron jobs, and a feedback loop that makes the agent smarter every day.

---

## What You'll Build

By the end of this course, you'll have a fully working dev agent with:

- **CLAUDE.md + state files** -- the agent's brain and memory
- **Hooks** -- deterministic guardrails (block dangerous operations, notify your phone)
- **Inline learning** -- the agent corrects itself in real-time, not on a schedule
- **6 skills** -- daily planner, PR reviewer, git reviewer, standup generator, meeting ingest, heartbeat
- **Cron scheduling** -- skills run on autopilot
- **Self-healing heartbeat** -- the agent monitors itself and recovers from failures
- **Remote access** -- control your agent from your phone via tmux + Tailscale

```
.claude/
  CLAUDE.md                    # Agent identity + rules
  preferences.md               # Who you are, how you work
  tasks-active.md              # Current work
  progress.txt                 # Action log
  learnings.md                 # What the agent learned
  cron-jobs.json               # 6 scheduled skills
  hooks/
    stop-telegram.sh           # Phone notifications
  skills/
    daily-planner/SKILL.md     # Score your day
    pr-reviewer/SKILL.md       # Review PRs across repos
    git-reviewer/SKILL.md      # Monitor commits
    standup-generator/SKILL.md # Generate standups
    meeting-ingest/SKILL.md    # Extract action items
    heartbeat/SKILL.md         # Self-monitoring
```

## Course Structure

| # | Lesson | What You Build |
|---|--------|----------------|
| 01 | [What Is a Dev Agent?](lessons/01-what-is-a-dev-agent.md) | Mental model -- see a real agent in action |
| 02 | [Foundation: CLAUDE.md + State Files](lessons/02-foundation-claudemd-state-files.md) | CLAUDE.md, preferences, tasks, progress files |
| 03 | [Hooks: Making It Deterministic](lessons/03-hooks-making-it-deterministic.md) | Stop hook (Telegram), permission gate |
| 04 | [Memory: Inline Learning](lessons/04-memory-inline-learning.md) | Error log, learnings, auto-resolver |
| 05 | [Skills and Scheduling](lessons/05-skills-and-scheduling.md) | Daily planner skill, cron-jobs.json |
| 06 | [PR Review Agent](lessons/06-pr-review-agent.md) | PR reviewer skill |
| 07 | [Git Reviewer + Standup Generator](lessons/07-git-reviewer-standup-generator.md) | 2 skills that share state |
| 08 | [Meeting Ingest + Failure Handling](lessons/08-meeting-ingest-failure-handling.md) | Meeting ingest, failed-jobs.log |
| 09 | [Heartbeat: Self-Healing](lessons/09-heartbeat-self-healing.md) | Heartbeat skill, cron renewal |
| 10 | [Access Your Agent Anywhere](lessons/10-access-your-agent-anywhere.md) | tmux + Tailscale + Termius |
| 11 | [Ship It](lessons/11-ship-it.md) | Full system review, MCP, what's next |

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) CLI installed
- A terminal (macOS/Linux recommended)
- Basic familiarity with git and the command line
- A Telegram bot token (for notifications -- optional but recommended)

## How to Use This Course

**Option 1: Follow along lesson by lesson**
Start at Lesson 01 and work through each lesson in order. Each lesson builds on the previous one.

**Option 2: Jump to what you need**
Each lesson is self-contained enough to read independently, but the skills build progressively.

**Option 3: Fork and customize**
The architecture is a pattern, not a product. Fork it, swap components, add your own skills. The [Component Swap Table](lessons/11-ship-it.md) in Lesson 11 shows what's replaceable.

## Key Principles

1. **Everything is a file.** No database. No cloud service. Git is the audit trail.
2. **Explain, then build.** Every concept is demonstrated before you implement it.
3. **One skill per lesson.** No overwhelming config dumps.
4. **The agent gets smarter over time.** Inline learning + daily consolidation = compound improvement.

## Diagrams

All 5 architecture diagrams are embedded directly in the lessons as Mermaid blocks -- they render natively on GitHub, no images needed.

## Who Made This

Built by [Eslam Helmy](https://github.com/eslamhelmy) -- Lead Engineer at Edenred UAE, YouTube [@arabdevsimplified](https://youtube.com/@arabdevsimplified).

This course was designed, written, and tested using the exact agent system it teaches.

## License

[MIT](LICENSE) -- fork it, modify it, make it yours.
