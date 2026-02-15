# Recommended Plugins

This template supports two optional Claude Code plugins that improve safety and continuity across sessions.

## Destructive Command Guard (dcg)

**Repository:** [Dicklesworthstone/destructive_command_guard](https://github.com/Dicklesworthstone/destructive_command_guard)

A high-performance Rust-based safety hook that intercepts and blocks destructive commands before Claude Code can execute them. Protects against accidental `rm -rf`, `git reset --hard`, `git push --force`, database drops, and more.

### What It Blocks

- **Git:** `git reset --hard`, `git push --force`, `git branch -D`, `git clean -f`, `git checkout -- .`
- **Filesystem:** `rm -rf` outside temp directories, destructive operations on system paths
- **Databases:** `DROP TABLE`, `TRUNCATE`, `DELETE` without `WHERE`
- **Cloud/Infra:** AWS/GCP/Azure resource deletion, Kubernetes namespace deletion
- **Embedded scripts:** Destructive code hidden in `python -c`, `node -e`, heredocs

### Key Features

- Sub-millisecond latency (SIMD-accelerated)
- 49+ modular security packs (database, Kubernetes, Docker, cloud providers, CI/CD)
- Fail-open design — never breaks legitimate workflows
- Heredoc and inline script scanning
- `dcg explain "command"` to understand why something was blocked

### Install

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/destructive_command_guard/master/install.sh?$(date +%s)" | bash -s -- --easy-mode
```

Or use the project setup script:

```bash
bash scripts/setup-plugins.sh
```

### Configuration

After installation, dcg config lives at `~/.config/dcg/config.toml`:

```toml
[packs]
enabled = [
    "core.filesystem",
    "core.git",
    "database.postgresql",
    "containers.docker",
]

[agents.claude-code]
trust_level = "high"
```

### Useful Commands

```bash
dcg status                    # Check installation status
dcg packs --verbose           # List all available security packs
dcg explain "rm -rf /"        # See why a command would be blocked
```

---

## Claude-Mem

**Repository:** [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)

Persistent memory for Claude Code across sessions. Automatically captures observations during your session, compresses them with AI, and injects relevant context into future sessions.

### How It Works

Claude-Mem operates through 5 lifecycle hooks:

| Hook | Purpose |
|------|---------|
| `SessionStart` | Retrieves relevant memories from prior sessions |
| `UserPromptSubmit` | Captures user input context |
| `PostToolUse` | Records tool execution results and observations |
| `Stop` | Handles session pause events |
| `SessionEnd` | Compresses and stores session memories |

### Key Features

- Automatic observation capture — no manual intervention needed
- AI-powered compression for efficient storage
- Progressive disclosure with layered context retrieval
- Web viewer UI at `http://localhost:37777`
- 5 MCP search tools: `search`, `timeline`, `get_observations`, `save_memory`, and docs

### Install

In a Claude Code session:

```
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```

Or use the project setup script:

```bash
bash scripts/setup-plugins.sh
```

### Configuration

Settings are stored at `~/.claude-mem/settings.json`, auto-generated with defaults on first launch.

---

## Skill Selector

**Repository:** [sandalsoft/skill-selector](https://github.com/sandalsoft/skill-selector)

Analyzes implementation plans, discovers relevant skills from the ecosystem, installs them, and rewrites plans to leverage what's available. Use it after generating a plan (via `/flow-next-plan`) to equip the agent with the best available skills before building.

### What It Does

1. **Plan Analysis** — Parses your plan to identify technology and domain boundaries where expert skills would improve output quality
2. **Ecosystem Search** — Uses `npx skills find` with targeted queries for each identified domain
3. **Skill Recommendations** — Presents a prioritized table (capped at 5-7 recommendations) showing install counts, source repos, and relevance
4. **Automated Installation** — Installs selected skills to project scope after user confirmation
5. **Plan Enhancement** — Rewrites the original plan to reference installed skills, adopt their conventions, and remove generic guidance the skills now handle

### Install

```bash
npx skills add sandalsoft/skill-selector
```

Or use the project setup script:

```bash
bash scripts/setup-plugins.sh
```

### Usage

```
/skill-selector path/to/plan.md
/skill-selector fn-1
/skill-selector "build a SvelteKit dashboard with D3 charts"
```

When invoked without arguments, scans `.flow/specs/` for the most recent spec file.

---

## Setup Script

Install all plugins at once:

```bash
bash scripts/setup-plugins.sh
```

The script will:
1. Install dcg with automatic platform detection
2. Install claude-mem plugin
3. Install skill-selector skill via `npx skills add`
4. Verify installations
5. Display status summary
