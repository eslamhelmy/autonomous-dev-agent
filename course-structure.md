# Build Your Own Dev Agent with Claude Code

> Create a persistent, safe, reporting autonomous agent stack for your own engineering workflow.

## Course Positioning (Validated by ChatGPT + Market Research)

**What this is:** Agent as infrastructure, not chatbot. Agent as operator, not pair programmer. Agent as system, not one-off prompt trick.

**What this is NOT:** Not AI app building. Not prompt engineering. Not "replace yourself with an autonomous coder."

**One-sentence promise:** Learn how to design a personal dev agent that can run recurring tasks, inspect codebases, execute bounded actions, produce reports, and operate safely over time using Claude Code.

**Before/After:**
- Before: manually check logs, issues, PRs, docs, tasks. Use AI in isolated chats. Do repetitive coordination yourself.
- After: personal agent that monitors, summarizes, proposes actions, executes approved workflows, and reports outcomes safely.

---

## Current Content Inventory

| Asset | Status |
|-------|--------|
| Full working agent stack (.claude/) | Done |
| 13 skills (scheduled + on-demand) | Done |
| Hooks lesson (V1 Arabic, V2 English, V3 DevRel) | Filmed |
| Teleprompter transcript for hooks | Pending (Apr 9 evening) |

---

## Course Structure (8 Modules, 30 Lessons)

### MODULE 0: What is a Dev Agent? (Intro Video — Free)
- Why Claude Code as an agent, not just a coding assistant
- The difference: assistant vs copilot vs script vs agent
- What you'll build by the end (demo of the working agent)
- **Fork-friendly:** Show YOUR agent doing YOUR daily tasks, then say "yours will look completely different"

---

### MODULE 1: Foundation — Your Agent's Brain
- **1.1** The .claude/ directory — file-based state architecture
  - Why files, not databases (portability, git-trackable, readable)
  - The directory layout and what each file does
- **1.2** CLAUDE.md — the master instruction file
  - Session startup ritual (what to read, in what order)
  - How to write rules the agent actually follows
- **1.3** Your first agent session
  - Setting up from scratch (clone template vs build from zero)
  - First interaction: agent reads state, acts, updates state

**Fork-friendly pattern:** Each lesson has a "Your Turn" section:
> "My CLAUDE.md has these sections because I'm a .NET dev who makes YouTube content. Yours might have: frontend deploy checks, PR review rules, or daily standup summaries. Here's the template — fill in YOUR workflow."

---

### MODULE 2: Making It Deterministic — Hooks (Already Filmed: 2.1)
- **2.1** The 9 events and mental model *(filmed)*
  - SessionStart, UserPromptSubmit, PreToolUse, PostToolUse, Notification, Stop, SubagentStop, PreCompact, SessionEnd
- **2.2** Hook types deep dive
  - command (primary), prompt, agent, http
  - Exit codes: 0 success, 1 warn, 2 BLOCK
  - Matchers: regex on tool names
- **2.3** Building safety rails
  - Block push to main (PreToolUse + exit 2)
  - Fix git identity (PreToolUse + auto-correct)
  - Custom rules for YOUR workflow
- **2.4** Notifications to your phone
  - Telegram bot setup
  - Stop hook → response preview to phone
  - Permission prompt → "check your terminal" alert
  - Alternative: Slack webhook (show both, viewer picks)

**Fork-friendly pattern:**
> "I use Telegram because I live in it. If you're a Slack team, here's the same hook with a Slack webhook. If you use Discord, swap the curl endpoint. The PATTERN is what matters — event → shell script → notification channel."

---

### MODULE 3: Memory & Learning
- **3.1** State files explained
  - tasks-active.md / tasks-completed.md (work tracking)
  - progress.txt (append-only action log)
  - learnings.md (accumulated patterns)
  - error-log.md (mistakes — read at startup, never repeat)
- **3.2** Inline learning — correct once, never repeat
  - When user corrects → immediately append to right file
  - Style correction → preferences.md
  - Bug you made → error-log.md
  - Good approach → learnings.md
- **3.3** The Learning Loop — daily consolidation
  - Review day's inline learnings
  - Pattern appears 3+ times → promote to CLAUDE.md
  - Consolidation, not replacement for inline learning
- **3.4** Preferences & don'ts — teaching your agent your style
  - Identity, calendars, communication style
  - The "Don'ts" section — hard boundaries

**Fork-friendly pattern:**
> "My preferences.md says I write in Egyptian Arabic for YouTube and use Teams for meetings. Yours might say you write in Japanese, use Linear for issues, and never touch the staging branch. The STRUCTURE is the same — the CONTENT is yours."

---

### MODULE 4: Skills Architecture
- **4.1** What is a skill? (anatomy of a SKILL.md file)
  - Input → Process → Output → State Update
  - Metadata: triggers, schedule, dependencies
- **4.2** Building your first skill from scratch
  - Pick ONE repetitive task you do weekly
  - Write the SKILL.md step by step
  - Test it, iterate, ship it
- **4.3** Scheduled skills (cron) vs on-demand skills
  - Cron setup with cron-jobs.json
  - Self-healing via heartbeat (recreates expired jobs)
  - On-demand: invoke manually when needed
- **4.4** Skill composition — skills that call other skills
  - browser-verify called by other skills after changes
  - Content pipeline: content-creator → shorts-generator → script-push

**Fork-friendly pattern:**
> "I'll build the daily-planner skill live. But I want you to pause the video here and write down: what's ONE thing you do every day that's repetitive? That's your first skill. Not mine — yours."

**Skill Starter Menu (viewer picks 2-3):**
| If you... | Build this skill |
|-----------|-----------------|
| Review PRs daily | pr-reviewer |
| Check deploy status | deploy-monitor |
| Write standups | standup-generator |
| Track dependencies | dependency-checker |
| Manage releases | release-notes |
| Monitor logs | log-watcher |

---

### MODULE 5: Autonomy & Safety
- **5.1** The auto-resolver — what to decide alone vs escalate
  - Green: generate drafts, query data, update state files
  - Yellow: push code, send messages, create events → needs approval
  - Red: delete data, modify prod, spend money → never alone
- **5.2** Priority map — teaching urgency levels
  - 4 levels (Critical → Low)
  - Decision rules: what gets interrupted, what waits
- **5.3** Error logging & recovery patterns
  - When things go wrong: log, don't crash
  - failed-jobs.log as dead-letter queue
  - How the agent recovers from its own mistakes
- **5.4** The heartbeat — self-monitoring agent health
  - Every 2h: check state freshness, cron renewal, context usage
  - Self-healing: recreate expired jobs
  - Alert if something looks wrong

**Fork-friendly pattern:**
> "My auto-resolver says 'never push to main without approval.' Yours might say 'never deploy to prod' or 'never send a Slack message to #general.' The FRAMEWORK is: define your green/yellow/red zones. The BOUNDARIES are yours."

---

### MODULE 6: Real-World Skills (Build Along — Pick Your Path)

> "I'm building all 5 of these live. You watch all of them to learn the PATTERNS, then build the 2-3 that match YOUR workflow."

- **6.1** Daily Planner — calendar + tasks + scoring
  - Pull from calendar APIs, score the day, report
- **6.2** Email Triage — categorize & flag urgent
  - Connect to email, apply rules, surface what matters
- **6.3** Git Reviewer — monitor commits across repos
  - Watch multiple repos, summarize changes, flag risks
- **6.4** Meeting Ingest — transcripts to action items
  - Pull transcript, extract actions, assign to tasks
- **6.5** Content Pipeline — V1 → V2 → V3 creation workflow
  - Idea → Draft → Optimized → Production package

**Fork-friendly:** Each lesson ends with "adaptation prompts":
> "You just watched me build email-triage for Gmail. If you use Outlook, here's what changes. If you use Slack DMs instead of email, here's how to adapt the same pattern."

---

### MODULE 7: Advanced Patterns
- **7.1** Ralph Loop — iterate until acceptance criteria pass
  - Define acceptance criteria, loop until they pass
  - When to stop, when to escalate
- **7.2** Browser verification — verify your agent's work
  - Agent makes changes → opens browser → validates
  - Screenshot evidence, pass/fail reporting
- **7.3** Multi-project agent (same agent, different repos)
  - Shared preferences, per-project skills
  - Context switching patterns
- **7.4** Connecting external tools (MCP servers, APIs)
  - Telegram bot for mobile reporting
  - MCP servers for tool access
  - Notion, Linear, Jira — pick your integration

---

### MODULE 8: Ship It — Your Personal Agent
- **8.1** Fork the template, customize for your workflow
  - The starter repo: what's included, what to change
  - First 30 minutes: customize CLAUDE.md + preferences + 1 skill
- **8.2** What to keep vs what to replace
  - Keep: state architecture, hooks system, learning loop, heartbeat
  - Replace: specific skills, notification channel, calendar APIs, content pipeline
- **8.3** Growing your agent over time
  - Week 1: foundation + 1 hook + 1 skill
  - Week 2: add learning + 2 more skills
  - Week 4: full autonomy boundaries + scheduled skills
  - Month 2: advanced patterns, ralph loop, multi-project
- **8.4** Community patterns & sharing skills
  - How to package a skill for others
  - Skill registry concept (share SKILL.md files)
  - "Show your agent" — community showcase format

---

## How to Make It Fork-Friendly (Structural Patterns)

### 1. Template Repo with Blank Slots
```
.claude/
├── preferences.md        ← FILL IN: your identity, tools, style
├── priority-map.md       ← FILL IN: your urgency levels
├── auto-resolver.md      ← FILL IN: your green/yellow/red zones
├── skills/
│   ├── _template/SKILL.md  ← COPY THIS for each new skill
│   └── heartbeat/          ← KEEP AS-IS (universal)
```

### 2. "My Version vs Your Version" Side-by-Side
Every lesson shows:
- Left: YOUR actual working config
- Right: blank template with comments explaining each field
- Bottom: 3 example adaptations (frontend dev, DevOps eng, data scientist)

### 3. Skill Starter Menu
Instead of "build these 5 skills," give a menu of 15+ skill ideas organized by role. Viewer picks the 3 that match their workflow.

### 4. "Agent Contract" Worksheet
Module 5 includes a downloadable worksheet:
- What should your agent do automatically?
- What needs your approval?
- What should it NEVER do?
- Who does it report to? (Telegram, Slack, email, desktop)

---

## ChatGPT Validation (Full Answers)

### 1. Is the structure logical and progressive?
**Yes.** Moves in the right order: mental model → control layer → memory → reusable capabilities → autonomy → real workflows → advanced patterns → shipping.

- Strongest part: **safety before autonomy** — exactly right for this topic
- Escalates well from concepts → infrastructure → build-along
- One weakness: **Module 7 feels partly foundational** — browser verification and external tools often matter earlier

### 2. What's missing that developers would expect?
- **System architecture overview**: what runs where, what files matter, what the event loop looks like, how all pieces connect
- **Testing/debugging the agent itself**: dry runs, replaying failures, tracing decisions, validating hooks safely
- **Permissions and threat model**: secrets, repo scope, shell access, destructive actions, auditability
- **Installation/bootstrap for different environments**: local machine vs VPS vs always-on box
- **Template/repo tour very early**: so students know the moving parts before they start editing

### 3. How to make each module fork-friendly?
- End every module with **"Now fork it" checkpoint**: "Here are the 3 files to edit, 2 values to change, and 1 thing to delete if it doesn't fit your workflow."
- Teach every feature as a **pattern with interchangeable examples**, not as your personal setup
- Add a **decision matrix** in each module: "Keep this if you need X, replace this if you need Y, skip this if you don't do Z."
- Include **minimal starter defaults** AND then an "opinionated Eslam version" separately
- Give each build-along skill a **customization exercise**: rename it, change one input source, change one escalation rule, change one output format

### 4. Should I reorder anything?
- **Move browser verification earlier** → Module 5 or at least introduce it there as part of safety/validation
- **Move connecting external tools earlier** if later modules depend on Telegram, Slack, calendar, or MCP — otherwise students feel they're using magic before it's explained
- **Place template architecture / repo anatomy** at the end of Module 1, before hooks, memory, and skills
- **Keep real-world skills before advanced loops** — students need concrete wins before deeper abstractions

### 5. Any modules to split or merge?
- **Module 2**: may be too dense → split into: **Control Surface** (events, hooks, execution model) + **Guardrails & Alerts** (safety rails, notifications)
- **Module 3 + 4**: tightly connected → could be reframed as one larger system block: **Memory + Skills = learned behavior**
- **Module 5 + parts of Module 7**: overlap conceptually → browser verification, recovery, heartbeat, escalation, Ralph Loop all belong under **Autonomy, Validation, and Recovery**
- **Module 6**: five examples may be too many → better to keep **3 deep examples than 5 shallow ones**

### Strongest recommendation from ChatGPT:
> Add one early lesson called **"Agent Architecture Tour"** and move **verification/tooling prerequisites** earlier. That will make the whole course feel more rigorous and easier to fork.

---

## Suggested Reordering (Incorporating ChatGPT Feedback)

| Change | Reason |
|--------|--------|
| Add Module 0 (free intro) | Hook viewers, show the end result upfront |
| Add "Agent Architecture Tour" to Module 1 (1.3) | ChatGPT's top recommendation — show the system before editing it |
| Move external tools/Telegram setup to Module 2.4 | Students need this before later modules use it |
| Move browser-verify to Module 5 | Part of autonomy/validation, not "advanced" |
| Module 6 → 3 deep skills, not 5 shallow | ChatGPT: 3 deep > 5 shallow |
| Module 6 → "Pick Your Path" | Reduces "copy my setup" feel |
| Add "Skill Starter Menu" in Module 4 | Give viewers agency to choose THEIR skills early |

## What's Missing (Combined: Market Research + ChatGPT)

| Gap | Where to Add |
|-----|-------------|
| Agent Architecture Tour (system overview) | Module 1.3 (new lesson) |
| Debugging/testing your agent | Module 5.3 (expand) |
| Permissions and threat model | Module 5 (new 5.5) |
| Cost/token awareness | Module 5 (new 5.6) |
| Installation/bootstrap for different envs | Module 1.3 or 8.1 |
| Context window management (compaction) | Module 3.2 (add section) |
| Template/repo tour early | Module 1 (end of module) |
| Team use (shared agent for a team) | Module 8.4 (mention) |
| Testing skills before deploying | Module 4.2 (add section) |

---

## Recording Priority

1. **Module 0** — Free intro video (best for YouTube discovery)
2. **Module 1** — Foundation (people need this to start)
3. **Module 4** — Skills Architecture (the "aha" moment)
4. **Module 5** — Autonomy & Safety (the trust moment)
5. **Module 3** — Memory & Learning (the persistence moment)
6. **Module 6** — Build Along (the practical value)
7. **Module 7** — Advanced (for returning viewers)
8. **Module 8** — Ship It (graduation)

Module 2 (Hooks) is already filmed — slot it in.
