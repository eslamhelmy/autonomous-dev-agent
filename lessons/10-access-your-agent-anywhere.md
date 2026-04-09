# Lesson 10 — Access Your Agent from Anywhere

Your agent runs on your machine. But you're not always at your machine. This lesson sets up phone access so you can check on your agent, approve permissions, and read reports from anywhere.

We'll also cover what NOT to use — tools that sound great but break in practice.

## Where You Are

```
.claude/
├── CLAUDE.md
├── preferences.md
├── error-log.md
├── learnings.md
├── auto-resolver.md
├── priority-map.md
├── tasks-active.md
├── tasks-completed.md
├── progress.txt
├── cron-jobs.json
├── failed-jobs.log
├── hooks/
│   ├── stop-telegram.sh
│   └── (settings.local.json configured)
└── skills/
    ├── daily-planner/
    ├── pr-reviewer/
    ├── git-reviewer/
    ├── email-triage/
    ├── meeting-ingest/
    └── heartbeat/
```

You have a full agent with 6 skills, hooks that notify your phone, and a heartbeat that self-heals. But if your terminal closes, the agent dies. And if you're away from your desk, you can't interact with it.

## What You Are Adding

Phone access to your agent via SSH — plus understanding why some alternatives don't work.

---

## Why Not Claude Channels / Cowork Dispatch?

Before we set up the reliable solution, let's address two tools you might have seen:

**Claude Channels** — lets you message Claude Code from a web interface or another device. Sounds perfect for remote agent access.

**Cowork Dispatch** — lets Claude Desktop dispatch tasks to Claude Code. Sounds perfect for orchestration.

**The reality after testing both extensively:**
- Connections disconnect randomly, especially during long-running tasks
- When the connection drops mid-task, the agent loses context and can't resume cleanly
- Reconnection doesn't restore the session state — you start from scratch
- The communication layer is fragile under real workloads (multiple cron jobs, background agents, heavy context)
- It breaks the momentum — you're debugging the communication tool instead of doing actual work

**The verdict:** These tools are promising but not production-ready for autonomous agents that need to run reliably for hours or days. They work fine for short, interactive sessions. They break for the always-on agent pattern this course teaches.

**What works instead:** SSH. It's 40 years old. It doesn't disconnect. It doesn't lose state. tmux keeps your session alive. Tailscale makes it accessible from anywhere. Boring, reliable, done.

---

## The Reliable Stack: tmux + Tailscale + Termius

Three tools, 10-minute setup:

| Tool | What It Does | Why |
|------|-------------|-----|
| **tmux** | Keeps your terminal session alive after disconnect | Agent survives laptop close, SSH drop, even reboot (with systemd) |
| **Tailscale** | Mesh VPN — your devices find each other anywhere | SSH from phone to laptop without port forwarding, static IPs, or VPN servers |
| **Termius** | Mobile SSH client | Check on your agent from your phone |

---

## Step 1: Install tmux

**Intent:** Create an immortal terminal session for your agent.

**Bash (system install):**
```bash
# macOS
brew install tmux

# Ubuntu/Debian
sudo apt install tmux

# Verify
tmux -V
```

**Create a named session and start Claude Code:**
```bash
tmux new -s agent
# Now you're inside the tmux session
claude
```

**Detach without killing it:** Press `Ctrl+B` then `D`. The session keeps running.

**Reattach from anywhere:**
```bash
tmux attach -t agent
```

Close your terminal. Open it again. Run `tmux attach -t agent`. Your agent is still there, mid-conversation, context intact.

---

## Step 2: Install Tailscale

**Intent:** Make your machine reachable from your phone without touching your router.

**Bash:**
```bash
# macOS
brew install tailscale

# Ubuntu/Debian
curl -fsSL https://tailscale.com/install.sh | sh

# Start and authenticate
sudo tailscale up
```

Follow the browser link to authenticate. Your machine gets a Tailscale IP (e.g., `100.x.y.z`) and a hostname (e.g., `eslams-macbook`).

**Verify from another device on the same Tailscale network:**
```bash
ping eslams-macbook
```

---

## Step 3: Install Termius on Your Phone

1. Download Termius from App Store / Play Store
2. Add a new host:
   - Hostname: your Tailscale IP or hostname
   - Username: your macOS/Linux username
   - Auth: SSH key (recommended) or password
3. Connect
4. Run: `tmux attach -t agent`

You're now looking at your Claude Code agent from your phone. Same session. Same context. Same cron jobs running.

---

## Step 4: Survive Reboots (Optional)

If your machine restarts, tmux dies. Fix it with a startup service.

**Intent:** tmux agent session starts automatically on boot.

**Prompt for Claude Code:**
```
Create a launchd plist (macOS) or systemd service (Linux) that starts a tmux session named "agent" on boot. The session should run as my user, not root. Include the full file content and the command to install it.

Rules:
- macOS: write to ~/Library/LaunchAgents/com.agent.tmux.plist
- Linux: write to ~/.config/systemd/user/agent-tmux.service
- Only create the file for my current OS
- Show how to enable it
```

**Expected output:** A service file that auto-starts tmux on boot. Your agent session is now immortal.

---

## The Full Flow

```
Your Phone (Termius)
  → Tailscale (encrypted tunnel)
    → Your Machine (SSH)
      → tmux session "agent"
        → Claude Code (running, context intact)
          → 6 skills on cron
          → Telegram notifications to your phone
          → Heartbeat self-healing every 2h
```

You're at dinner. Your phone buzzes — Telegram notification: "Daily plan ready. 3 meetings tomorrow, 2 tasks carried forward, day score: 8/10."

You want to check details? Open Termius, `tmux attach -t agent`, you're in.

---

## Checkpoint

After this lesson, you have:
- tmux keeping your agent session alive
- Tailscale making your machine reachable from anywhere
- Termius on your phone for SSH access
- Understanding of why Channels/Cowork aren't reliable for this use case

No new .claude/ files — this lesson is about infrastructure around the agent, not inside it.

---

## Fork It

- **Don't use Termius?** Any SSH client works — Blink Shell (iOS), JuiceSSH (Android), or plain `ssh` from another machine
- **Don't want Tailscale?** Any VPN or port forwarding works — Tailscale is just the easiest zero-config option
- **Want a web terminal?** ttyd or Wetty can expose tmux over HTTPS — but adds complexity
- **Corporate network blocks Tailscale?** Use Cloudflare Tunnel or plain WireGuard instead
