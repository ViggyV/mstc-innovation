# Claude Code Agent Blueprint

A comprehensive, production-ready framework for creating Claude Code agents for **any project**. Clone this repo, fill in your project specifics, and you have a fully configured AI development agent — plus a complete business strategy toolkit built from proven innovation frameworks.

---

## What's Inside

**33 files** organized into 5 sections:

### Documentation (9 files)
The complete guide to building Claude Code agents from scratch.

| File | What You'll Learn |
|------|-------------------|
| [`01-ARCHITECTURE.md`](01-ARCHITECTURE.md) | The 5-layer agent stack — instructions, skills, commands, tools, memory. How Claude Code loads and prioritizes configuration. |
| [`02-CLAUDE-MD-TEMPLATE.md`](02-CLAUDE-MD-TEMPLATE.md) | Full CLAUDE.md template with working examples for Python, Next.js, React Native, and Data Science projects. |
| [`03-SKILLS-SYSTEM.md`](03-SKILLS-SYSTEM.md) | Creating auto-activating SKILL.md modules. 4 design patterns: Domain Expert, Workflow Orchestrator, Quality Gate, Project Orchestrator. |
| [`04-SLASH-COMMANDS.md`](04-SLASH-COMMANDS.md) | Building `/commands` for repeatable workflows. Examples for development, content, data, git, and DevOps. |
| [`05-MCP-INTEGRATION.md`](05-MCP-INTEGRATION.md) | Connecting external tools (browser, databases, custom APIs) via Model Context Protocol. Python and TypeScript examples. |
| [`06-AGENT-PATTERNS.md`](06-AGENT-PATTERNS.md) | 6 production agent patterns: ReAct, Multi-Agent, RAG, Pipeline, Conversational, Guardrailed. With implementation code. |
| [`08-DEPLOYMENT-CHECKLIST.md`](08-DEPLOYMENT-CHECKLIST.md) | Step-by-step setup checklist from zero to production agent. Quick reference card. |
| [`09-IMPLEMENTATION-PROMPTS.md`](09-IMPLEMENTATION-PROMPTS.md) | All implementation prompts in one reference doc with a decision table. |
| [`CLAUDE.md`](CLAUDE.md) | Live example — Innovation Strategy Agent configured for business application. |

### Project Templates (6 files)
Ready-to-copy `CLAUDE.md` configurations. Pick your project type, customize, and go.

| Template | Best For |
|----------|----------|
| [`python-saas.md`](07-PROJECT-TEMPLATES/python-saas.md) | FastAPI + PostgreSQL + Redis SaaS applications |
| [`nextjs-app.md`](07-PROJECT-TEMPLATES/nextjs-app.md) | Next.js 14 App Router + Supabase + Tailwind full-stack apps |
| [`data-science.md`](07-PROJECT-TEMPLATES/data-science.md) | ML/data projects with pandas, scikit-learn, MLflow |
| [`mobile-app.md`](07-PROJECT-TEMPLATES/mobile-app.md) | React Native + Expo mobile applications |
| [`knowledge-base-agent.md`](07-PROJECT-TEMPLATES/knowledge-base-agent.md) | Document collections, course materials, research libraries |
| [`business-innovation-agent.md`](07-PROJECT-TEMPLATES/business-innovation-agent.md) | Startup strategy, market validation, competitive analysis |

### Business Strategy Commands (9 files)
Slash commands that turn Claude Code into a business strategy copilot. Each is a complete, self-contained workflow.

| Command | What It Does |
|---------|-------------|
| [`/market-validator`](commands/market-validator.md) | Technology Filter scoring (8 dimensions, /40) → TAM/SAM/SOM market sizing → 7-Powers competitive moat analysis → Go/No-Go verdict |
| [`/lean-canvas-builder`](commands/lean-canvas-builder.md) | Walks through all 9 Lean Canvas boxes → builds visual canvas → stress-tests assumptions → generates 4-8 week validation roadmap |
| [`/customer-discovery`](commands/customer-discovery.md) | Hypothesis definition → Mom Test interview guide design → interview analysis sheets → pattern synthesis → research-backed personas |
| [`/pitch-builder`](commands/pitch-builder.md) | 12-slide investor pitch deck → 30-second elevator pitch → 15 investor Q&A answers → one-pager → financial snapshot |
| [`/competitive-intelligence`](commands/competitive-intelligence.md) | Landscape mapping (direct/indirect/substitutes) → competitor deep dives → 7-Powers analysis → Porter's Five Forces → moat-building roadmap |
| [`/solutionstorm`](commands/solutionstorm.md) | Problem framing → 20+ idea generation (5 rounds) → feasibility-impact matrix → weighted scoring → solution concept → experiment design |
| [`/course-explorer`](commands/course-explorer.md) | Navigate and synthesize course/training materials across sessions and topics |
| [`/framework-analyzer`](commands/framework-analyzer.md) | Deep-dive any business framework (Lean Canvas, 7-Powers, JTBD, etc.) and apply it to your specific context |
| [`/deliverable-builder`](commands/deliverable-builder.md) | Build structured business deliverables: Quicklook reports, Lean Canvases, case analyses, interview guides |

### Implementation Prompts (8 files)
Copy-paste prompts to bootstrap agents in any new project. Each is self-contained — open the file, copy below the `---`, paste into Claude Code.

| Prompt | What It Does |
|--------|-------------|
| [`00-bootstrap-agent.md`](prompts/00-bootstrap-agent.md) | Scans any existing codebase → auto-generates CLAUDE.md + 3 project-specific commands |
| [`01-generate-skills.md`](prompts/01-generate-skills.md) | Analyzes codebase domains → creates auto-activating SKILL.md for each |
| [`02-generate-commands.md`](prompts/02-generate-commands.md) | Generates complete slash command library tailored to your stack and workflow |
| [`03-business-validation.md`](prompts/03-business-validation.md) | Runs 8-step market validation pipeline: Filter → Size → Compete → 7-Powers → Canvas → Discovery → Verdict |
| [`04-build-agent-api.md`](prompts/04-build-agent-api.md) | Builds production ReAct agent with Anthropic SDK: tools, guardrails, FastAPI server, tests |
| [`05-mcp-server-builder.md`](prompts/05-mcp-server-builder.md) | Creates custom MCP server for any API with FastMCP, config, and tests |
| [`06-knowledge-agent.md`](prompts/06-knowledge-agent.md) | Turns any folder of documents into an intelligent knowledge agent |
| [`07-team-onboarding.md`](prompts/07-team-onboarding.md) | Sets up shared Claude Code agent config for an entire dev team |

---

## Quick Start

### Option 1: Bootstrap Agent for an Existing Codebase
```bash
# Clone this repo
git clone https://github.com/ViggyV/mstc-innovation.git

# Go to your project
cd /path/to/your/project

# Open the bootstrap prompt, copy it, paste into Claude Code
cat /path/to/mstc-innovation/prompts/00-bootstrap-agent.md

# Or just start Claude Code and paste the prompt content
claude
```

### Option 2: Use a Project Template
```bash
# Pick your project type and copy the template
cp mstc-innovation/07-PROJECT-TEMPLATES/python-saas.md ~/my-project/CLAUDE.md

# Edit to match your project
# Then start Claude Code — it reads CLAUDE.md automatically
claude
```

### Option 3: Install Business Strategy Commands
```bash
# Copy all business commands to your Claude commands directory
cp mstc-innovation/commands/*.md ~/.claude/commands/

# Now you can use them in any Claude Code session:
# /market-validator
# /lean-canvas-builder
# /customer-discovery
# /pitch-builder
# /competitive-intelligence
# /solutionstorm
```

### Option 4: Use as Reference
Read the documentation in order (01 through 09) for a complete understanding of the Claude Code agent system. Each doc builds on the previous.

---

## Repo Structure

```
mstc-innovation/
├── README.md                          # You are here
├── CLAUDE.md                          # Live example: Innovation Strategy Agent
│
├── # DOCUMENTATION
├── 01-ARCHITECTURE.md                 # Agent stack (5 layers)
├── 02-CLAUDE-MD-TEMPLATE.md           # CLAUDE.md template + examples
├── 03-SKILLS-SYSTEM.md                # Auto-activating skills guide
├── 04-SLASH-COMMANDS.md               # Slash command creation guide
├── 05-MCP-INTEGRATION.md              # External tool connection guide
├── 06-AGENT-PATTERNS.md               # 6 agent architecture patterns
├── 08-DEPLOYMENT-CHECKLIST.md         # Setup checklist
├── 09-IMPLEMENTATION-PROMPTS.md       # All prompts reference
│
├── # PROJECT TEMPLATES
├── 07-PROJECT-TEMPLATES/
│   ├── python-saas.md                 # FastAPI + PostgreSQL SaaS
│   ├── nextjs-app.md                  # Next.js 14 full-stack
│   ├── data-science.md                # ML/data pipeline
│   ├── mobile-app.md                  # React Native + Expo
│   ├── knowledge-base-agent.md        # Document collection agent
│   └── business-innovation-agent.md   # Strategy & innovation agent
│
├── # BUSINESS COMMANDS
├── commands/
│   ├── market-validator.md            # /market-validator
│   ├── lean-canvas-builder.md         # /lean-canvas-builder
│   ├── customer-discovery.md          # /customer-discovery
│   ├── pitch-builder.md               # /pitch-builder
│   ├── competitive-intelligence.md    # /competitive-intelligence
│   ├── solutionstorm.md              # /solutionstorm
│   ├── course-explorer.md            # /course-explorer
│   ├── framework-analyzer.md         # /framework-analyzer
│   └── deliverable-builder.md        # /deliverable-builder
│
└── # IMPLEMENTATION PROMPTS
    prompts/
    ├── 00-bootstrap-agent.md          # Auto-setup for any codebase
    ├── 01-generate-skills.md          # Generate project skills
    ├── 02-generate-commands.md        # Generate command library
    ├── 03-business-validation.md      # 8-step market validation
    ├── 04-build-agent-api.md          # Build ReAct agent with SDK
    ├── 05-mcp-server-builder.md       # Build custom MCP server
    ├── 06-knowledge-agent.md          # Document → agent converter
    └── 07-team-onboarding.md          # Team agent setup
```

---

## Frameworks Included

The business commands and CLAUDE.md encode these proven frameworks:

| Category | Frameworks |
|----------|-----------|
| **Discovery** | Big Idea Canvas, Innovator's DNA, CB Insights Failure Analysis, Technology Filter |
| **Validation** | Lean Canvas, Quicklook Assessment, Customer Discovery (Mom Test), Jobs-to-be-Done |
| **Strategy** | 7-Powers (Hamilton Helmer), Porter's Five Forces, Blue Ocean, Wardley Mapping |
| **Execution** | Design Thinking, Solutionstorming, Lean Startup Cycle, OKR Framework |
| **IP & Legal** | Patent Strategy, Trade Secrets, Licensing Models |

---

## Who This Is For

- **Founders** — Validate ideas, build pitches, analyze markets without hiring consultants
- **Consultants** — Standardize your strategy deliverables with repeatable AI workflows
- **Product Managers** — Customer discovery, competitive analysis, and business modeling on demand
- **Developers** — Set up Claude Code agents that actually understand your codebase
- **Teams** — Shared agent configuration so everyone gets consistent AI assistance
- **Students** — Apply MBA frameworks to real business problems with guided workflows

---

## License

MIT — use it however you want.
