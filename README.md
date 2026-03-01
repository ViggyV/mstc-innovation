# MSTC Innovation Agent

An AI-powered business strategy agent built from UT Austin's MSTC Innovation & Entrepreneurship curriculum. Applies proven frameworks — Lean Canvas, 7-Powers, Technology Filter, Customer Discovery, and more — to real business opportunities.

## Install

```bash
# 1. Copy the agent config to your project
cp CLAUDE.md /path/to/your/project/CLAUDE.md

# 2. Copy the commands to your Claude commands folder
cp commands/*.md ~/.claude/commands/

# 3. Start Claude Code in your project
cd /path/to/your/project
claude
```

## Commands

| Command | What It Does |
|---------|-------------|
| `/market-validator` | Score an opportunity across 8 dimensions (/40), size the market (TAM/SAM/SOM), run 7-Powers analysis, get a Go/No-Go verdict |
| `/lean-canvas-builder` | Walk through all 9 boxes, build the visual canvas, stress-test assumptions, get a validation roadmap |
| `/customer-discovery` | Define hypotheses, design Mom Test interviews, analyze transcripts, synthesize patterns, build personas |
| `/pitch-builder` | Build a 12-slide investor deck, 30-second elevator pitch, prep 15 investor Q&A answers |
| `/competitive-intelligence` | Map the landscape, deep-dive competitors, run 7-Powers + Porter's Five Forces, build a moat strategy |
| `/solutionstorm` | Frame the problem, generate 20+ ideas across 5 rounds, score on a weighted matrix, design a validation experiment |
| `/framework-analyzer` | Pick any framework from the library, break it down, apply it to your specific business |
| `/course-explorer` | Search and synthesize across all 9 sessions of course materials |
| `/deliverable-builder` | Build Quicklook reports, Lean Canvases, case analyses, interview guides from templates |

## Frameworks

| Category | Frameworks |
|----------|-----------|
| **Discovery** | Big Idea Canvas, Innovator's DNA, CB Insights Failure Analysis, Technology Filter |
| **Validation** | Lean Canvas, Quicklook Assessment, Customer Discovery (Mom Test), Jobs-to-be-Done |
| **Strategy** | 7-Powers (Hamilton Helmer), Porter's Five Forces, Competitive Analysis |
| **Execution** | Design Thinking with AI, Solutionstorming, Innovation Challenges |
| **IP** | Patent Strategy, Trade Secrets, Licensing ("How to Sell a Secret") |

## Example Usage

```
> /market-validator

Describe your opportunity:
> AI-powered inventory management for small restaurants

[Agent runs Technology Filter → Market Sizing → Competitive Analysis → 7-Powers → Verdict]
```

```
> /lean-canvas-builder

What's the business?
> SaaS platform that predicts food waste for restaurant chains

[Agent walks through each box, builds visual canvas, identifies riskiest assumption]
```

```
> /customer-discovery

What hypothesis do you want to test?
> Restaurant managers spend 5+ hours/week on manual inventory counts

[Agent designs interview guide, provides analysis template, sets success criteria]
```

## Repo Structure

```
mstc-innovation/
├── CLAUDE.md                          # Agent configuration
├── README.md
└── commands/
    ├── market-validator.md            # /market-validator
    ├── lean-canvas-builder.md         # /lean-canvas-builder
    ├── customer-discovery.md          # /customer-discovery
    ├── pitch-builder.md               # /pitch-builder
    ├── competitive-intelligence.md    # /competitive-intelligence
    ├── solutionstorm.md               # /solutionstorm
    ├── framework-analyzer.md          # /framework-analyzer
    ├── course-explorer.md             # /course-explorer
    └── deliverable-builder.md         # /deliverable-builder
```
