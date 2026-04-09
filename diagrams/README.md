# Course Diagrams

## Mermaid Diagrams (embedded in lessons)
These render automatically on GitHub, Notion, and most blog platforms.
To convert to PNG: use https://mermaid.live or `mmdc` CLI.

## AI-Generated Images
Prompts for Gemini/DALL-E are in `../diagram-prompts.md`.
One image generated (autonomy loop) — available in Gemini chat history.

## Diagram 1: Autonomy Loop (Lesson 01)

```mermaid
graph LR
    T[TRIGGER] -->|cron/webhook| D[DECIDE]
    D -->|read state| A[ACT]
    A -->|execute| V[VERIFY]
    V -->|check result| R[REPORT]
    R -->|next cycle| T
    style T fill:#e67e22,stroke:#f39c12,color:#fff
    style D fill:#e67e22,stroke:#f39c12,color:#fff
    style A fill:#e67e22,stroke:#f39c12,color:#fff
    style V fill:#27ae60,stroke:#2ecc71,color:#fff
    style R fill:#e67e22,stroke:#f39c12,color:#fff
```

## Diagram 2: Hooks and Events (Lesson 03)

```mermaid
graph TD
    subgraph "Claude Code Session Events"
        SS[SessionStart] --> UPS[UserPromptSubmit]
        UPS --> PreTU[PreToolUse]
        PreTU --> PostTU[PostToolUse]
        PostTU --> N[Notification]
        N --> ST[Stop]
        ST --> SAS[SubagentStop]
        SAS --> PC[PreCompact]
        PC --> SE[SessionEnd]
    end

    subgraph "Your Hooks"
        PreTU -.->|matcher: Bash| BPM["block-push-main.sh\nEXIT 2 = BLOCK"]
        ST -.->|always| STG["stop-telegram.sh\nNotify phone"]
        N -.->|matcher: permission_prompt| PG["permission-gate.sh\nAlert: check terminal"]
    end

    style BPM fill:#e74c3c,stroke:#c0392b,color:#fff
    style STG fill:#27ae60,stroke:#2ecc71,color:#fff
    style PG fill:#f39c12,stroke:#e67e22,color:#fff
```

## Diagram 3: Inline Learning Flow (Lesson 04)

```mermaid
graph TD
    UC["User corrects agent"] --> R{"Route by type"}
    R -->|style/format| P["preferences.md"]
    R -->|bug/error| E["error-log.md"]
    R -->|good approach| LP["learnings.md\n(Patterns)"]
    R -->|bad approach| LM["learnings.md\n(Mistakes)"]

    P --> CL["CLAUDE.md"]
    E --> CL
    LP --> CL
    LM --> CL

    CL -.->|"promoted after 3+ occurrences"| PERM["Permanent rule"]

    style UC fill:#3498db,stroke:#2980b9,color:#fff
    style R fill:#f39c12,stroke:#e67e22,color:#fff
    style P fill:#27ae60,stroke:#2ecc71,color:#fff
    style E fill:#e74c3c,stroke:#c0392b,color:#fff
    style LP fill:#27ae60,stroke:#2ecc71,color:#fff
    style LM fill:#e74c3c,stroke:#c0392b,color:#fff
    style PERM fill:#9b59b6,stroke:#8e44ad,color:#fff
```

## Diagram 4: Skills and Scheduling (Lesson 05)

```mermaid
graph LR
    subgraph "Scheduling"
        CJ["cron-jobs.json"] -->|"8:30 AM"| SG
        CJ -->|"12:00 PM"| GR
        CJ -->|"5:33 PM"| DP
        CJ -->|"every 2h"| HB
    end

    subgraph "Skills"
        DP["daily-planner\nSKILL.md"]
        GR["git-reviewer\nSKILL.md"]
        SG["standup-generator\nSKILL.md"]
        HB["heartbeat\nSKILL.md"]
    end

    subgraph "Shared State"
        TA["tasks-active.md"]
        PT["progress.txt"]
        TC["tasks-completed.md"]
    end

    DP --> TA
    DP --> PT
    GR --> PT
    SG -->|"reads"| PT
    SG -->|"reads"| TC
    HB -.->|"recreates expired"| CJ

    style CJ fill:#3498db,stroke:#2980b9,color:#fff
    style HB fill:#e74c3c,stroke:#c0392b,color:#fff
```

## Diagram 5: Heartbeat Self-Healing (Lesson 09)

```mermaid
graph TD
    HB["HEARTBEAT\nevery 2 hours"] --> C1{"progress.txt\nfresh?"}
    HB --> C2{"Tasks past\ndeadline?"}
    HB --> C3{"failed-jobs.log\nempty?"}
    HB --> C4{"pipeline.json\nvalid?"}
    HB --> C5{"Cron jobs\nalive?"}
    HB --> C6{"Context\n< 60%?"}

    C5 -->|"missing"| FIX["Recreate via\nCronCreate"]
    C6 -->|"> 60%"| WARN["Warn user:\nrun /compact"]
    C3 -->|"has entries"| ALERT["Flag for\nreview"]

    style HB fill:#e74c3c,stroke:#c0392b,color:#fff
    style FIX fill:#27ae60,stroke:#2ecc71,color:#fff
    style WARN fill:#f39c12,stroke:#e67e22,color:#fff
    style ALERT fill:#f39c12,stroke:#e67e22,color:#fff
```
