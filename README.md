# Claude Code Template

A template repository for bootstrapping projects using Claude's code execution environment on claude.ai.

## How to Use

### 1. Create a new repo from this template
- Click **"Use this template"** on GitHub
- Name your new repo
- Clone it or leave it on GitHub

### 2. Start a new conversation on claude.ai

Tell Claude:

> Clone https://github.com/YOUR_USERNAME/YOUR_NEW_REPO and build me a [your idea]

Or if you just want to use the template without a GitHub repo:

> Read the CLAUDE.md and flow commands from my template, then interview me about building [your idea]

### 3. The Flow

1. **Interview** — Claude asks you questions about the project
2. **Plan** — Claude generates a detailed implementation plan
3. **Execute** — Ralph (the autonomous executor) builds it step by step

## Structure

```
├── CLAUDE.md                          # Project instructions for Claude
├── .claude/commands/
│   ├── flow-next-interview.md         # Discovery interview
│   ├── flow-next-plan.md              # Plan generation
│   └── flow-next-init-ralph.md        # Autonomous execution
├── scripts/ralph/
│   ├── mark_done.py                   # Step tracking utility
│   ├── steps.json                     # Machine-readable plan (generated)
│   ├── logs/                          # Execution logs
│   └── state/                         # State tracking
├── docs/
│   └── interview-answers.md           # Interview output (generated)
└── plan.md                            # Implementation plan (generated)
```

## Flow Commands

| Command | What it does |
|---------|-------------|
| `/flow-next-interview <idea>` | Structured discovery interview |
| `/flow-next-plan` | Generate implementation plan |
| `/flow-next-init-ralph` | Begin autonomous execution |

## Customization

Edit `CLAUDE.md` to add your own coding standards, preferred tech stack, or project-specific instructions. Add custom flow commands in `.claude/commands/`.
