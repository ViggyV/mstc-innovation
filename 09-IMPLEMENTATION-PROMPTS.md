# 09 - Implementation Prompts

Copy-paste these prompts into Claude Code to bootstrap an agent for any new project.
Each prompt is self-contained and designed to produce production-ready output.

---

## TABLE OF CONTENTS

1. [Project Setup Prompts](#1-project-setup-prompts)
2. [CLAUDE.md Generator Prompts](#2-claudemd-generator-prompts)
3. [Skill Creation Prompts](#3-skill-creation-prompts)
4. [Command Creation Prompts](#4-command-creation-prompts)
5. [MCP Server Prompts](#5-mcp-server-prompts)
6. [Business Strategy Prompts](#6-business-strategy-prompts)
7. [Agent Architecture Prompts](#7-agent-architecture-prompts)
8. [Quality & Testing Prompts](#8-quality--testing-prompts)

---

## 1. PROJECT SETUP PROMPTS

### Prompt: Bootstrap Agent for Existing Codebase

```
Scan this entire codebase and create a comprehensive CLAUDE.md agent configuration file. Do the following:

1. EXPLORE the project:
   - Read package.json / pyproject.toml / Cargo.toml / go.mod (whatever exists)
   - Scan the directory structure (src/, lib/, app/, tests/, etc.)
   - Read 3-5 key source files to understand patterns
   - Check for existing configs (.eslintrc, tsconfig, ruff.toml, etc.)
   - Read any existing README or documentation

2. GENERATE a CLAUDE.md with these sections:
   - PROJECT OVERVIEW: What this project does, its goal, stage, and users
   - TECH STACK: Every framework, library, and tool detected
   - ARCHITECTURE: Directory structure map with purpose of each folder
   - KEY FILES: The 10 most important files with one-line descriptions
   - CONVENTIONS: Coding patterns observed (naming, error handling, testing, etc.)
   - CONSTRAINTS: Never/Always rules based on the codebase patterns
   - WORKFLOW RULES: How to approach changes in this specific codebase

3. Place the file at the project root as CLAUDE.md

Be specific — don't write generic advice. Every line should be based on what you actually found in this codebase.
```

### Prompt: Bootstrap Agent for New Project

```
I'm starting a new project. Help me set up a complete Claude Code agent configuration.

Project details:
- Name: [PROJECT NAME]
- Type: [web app / API / mobile app / data pipeline / CLI tool / library]
- Language: [Python / TypeScript / Go / Rust / etc.]
- Framework: [Next.js / FastAPI / Django / Express / etc.]
- Database: [PostgreSQL / MongoDB / Supabase / Firebase / none]
- Hosting target: [Vercel / AWS / GCP / Railway / self-hosted]

Create:
1. A CLAUDE.md file with full project configuration
2. A .claude/ directory structure
3. Recommended skills and commands for this project type
4. A starter .claude/.mcp.json if MCP tools would be useful

For the CLAUDE.md, include:
- Project overview with clear goal statement
- Complete tech stack specification
- Planned directory structure with purpose annotations
- Coding conventions (naming, patterns, error handling)
- Testing strategy
- Constraints (never do / always do)
- Workflow rules for common task types
```

### Prompt: Audit Existing Agent Configuration

```
Audit my current Claude Code agent setup and identify gaps. Check:

1. ~/.claude/CLAUDE.md — Is it comprehensive? Missing sections?
2. Project CLAUDE.md — Does it accurately describe this codebase?
3. ~/.claude/commands/ — What commands exist? What's missing for this project type?
4. ~/.claude/skills/ — Are there skills? Are descriptions clear enough to auto-activate?
5. .claude/.mcp.json — Any MCP servers configured? Are they working?
6. Memory files — Is there useful persistent memory?

For each gap found, provide:
- What's missing
- Why it matters
- The exact file content to fix it

Output a checklist I can work through to bring the agent to production quality.
```

---

## 2. CLAUDE.md GENERATOR PROMPTS

### Prompt: Generate CLAUDE.md from Requirements Doc

```
I have a requirements document / PRD / spec for my project. Read it and generate a production CLAUDE.md.

Requirements: [paste or provide file path]

The CLAUDE.md should:
1. Translate business requirements into technical architecture
2. Define the tech stack based on the requirements
3. Create directory structure that supports the features described
4. Establish conventions that prevent the most common bugs for this type of project
5. Set priorities that align with the requirements
6. Include constraints specific to this domain (security, compliance, performance)

Make the CLAUDE.md specific enough that another developer using Claude Code would understand exactly how to build this project just by reading it.
```

### Prompt: Generate CLAUDE.md for Microservice

```
Generate a CLAUDE.md for a microservice with these characteristics:

Service name: [NAME]
Purpose: [WHAT IT DOES]
Inputs: [WHAT IT RECEIVES — API calls, events, messages]
Outputs: [WHAT IT PRODUCES — responses, events, side effects]
Dependencies: [OTHER SERVICES IT CALLS]
Data store: [DATABASE / CACHE / NONE]
SLA: [LATENCY / UPTIME REQUIREMENTS]

Include:
- API contract (endpoints, request/response schemas)
- Error handling patterns specific to this service
- Health check and monitoring requirements
- Testing strategy (unit, integration, contract tests)
- Deployment configuration
- Inter-service communication patterns
```

### Prompt: Generate Domain-Specific CLAUDE.md

```
I'm building a [DOMAIN TYPE] application. Generate a CLAUDE.md that includes domain-specific knowledge.

Domain types (pick one):
- E-commerce: payment processing, inventory, cart, checkout flows
- Healthcare: HIPAA compliance, PHI handling, audit logging
- Fintech: PCI-DSS, transaction processing, reconciliation
- SaaS: multi-tenancy, billing, user management, RBAC
- AI/ML: experiment tracking, model serving, data pipelines
- IoT: device management, telemetry, edge computing
- Marketplace: two-sided matching, trust/safety, payments

Include domain-specific:
- Compliance requirements and constraints
- Common architectural patterns for this domain
- Security considerations
- Data modeling patterns
- Integration patterns (payment gateways, identity providers, etc.)
- Testing requirements specific to this domain
```

---

## 3. SKILL CREATION PROMPTS

### Prompt: Generate Auto-Activating Skill

```
Create a Claude Code skill that auto-activates for [DOMAIN].

Skill requirements:
- Name: [SKILL-NAME] (hyphen-case)
- Domain: [WHAT IT'S EXPERT AT]
- Triggers: [WHEN SHOULD IT ACTIVATE — be specific]
- Tools needed: [ANY PRE-APPROVED TOOLS]

The skill should include:
1. YAML frontmatter with name, description, allowed-tools
2. Expert identity statement
3. Activation conditions (5+ specific triggers)
4. Step-by-step process for common tasks in this domain
5. Code templates and patterns (3+ reusable examples)
6. Quality checklist for validating work
7. Anti-patterns to avoid

Write the SKILL.md file content. The description field is critical — Claude reads it to decide when to activate, so make it precise and keyword-rich.

Place the output at: ~/.claude/skills/[SKILL-NAME]/SKILL.md
```

### Prompt: Generate Skill Library for Project

```
Analyze this codebase and generate a complete skill library. For each major domain in the project, create a SKILL.md that covers:

For each skill:
1. What the skill is expert at
2. When it should auto-activate (specific file patterns, keywords, task types)
3. The coding patterns and templates for this domain
4. Quality checks and validation steps

I need:
- A parent/orchestrator skill that coordinates all child skills
- Individual skills for each major domain (frontend, backend, data, etc.)
- A testing skill that knows this project's test patterns
- A deployment skill that knows this project's CI/CD

Output each as a complete SKILL.md file with proper frontmatter.
```

### Prompt: Convert Existing Knowledge to Skill

```
I have expertise/documentation about [TOPIC]. Convert it into a Claude Code skill.

Knowledge source: [PASTE TEXT or PROVIDE FILE PATH]

Transform this into a SKILL.md that:
1. Captures the key expertise as actionable instructions
2. Includes decision trees for common scenarios
3. Provides code templates based on the patterns described
4. Defines when this skill should auto-activate
5. Includes validation criteria for quality

The skill should be usable by someone who doesn't have this expertise — the skill IS the expertise.
```

---

## 4. COMMAND CREATION PROMPTS

### Prompt: Generate Slash Command

```
Create a Claude Code slash command for [WORKFLOW NAME].

Command requirements:
- Name: [COMMAND-NAME] (will be invoked as /command-name)
- Purpose: [WHAT THE COMMAND DOES]
- Input: [WHAT THE USER PROVIDES]
- Output: [WHAT THE COMMAND PRODUCES]

The command should:
1. Start with an expert identity statement
2. List activation conditions
3. Define a numbered process (Assessment → Execution → Validation)
4. Include templates/examples for the output
5. End with an output format specification

Style guide:
- Write in second person ("You are an expert at...")
- Include tables for structured comparisons
- Use code blocks for templates
- Be specific — vague commands produce vague results

Save to: ~/.claude/commands/[COMMAND-NAME].md
```

### Prompt: Generate Command Library for Team

```
Generate a complete slash command library for a [TYPE] development team.

Team profile:
- Size: [NUMBER] developers
- Stack: [TECH STACK]
- Workflow: [AGILE / KANBAN / etc.]
- Deploy frequency: [DAILY / WEEKLY / etc.]
- Code review process: [PR-BASED / PAIR / etc.]

Generate commands for:

DEVELOPMENT:
- /code-review — Structured code review with severity levels
- /test-gen — Generate tests following our patterns
- /debug — Systematic debugging workflow
- /refactor — Safe refactoring with validation

GIT & DEPLOYMENT:
- /commit — Smart commit message generation
- /pr-create — PR with description template
- /release — Release checklist and changelog

PLANNING:
- /estimate — Task estimation with complexity factors
- /spike — Technical spike investigation template
- /rfc — Request for comments document generator

QUALITY:
- /security-check — OWASP-based security review
- /performance — Performance analysis workflow
- /accessibility — a11y audit checklist

Output each as a complete .md file ready to place in ~/.claude/commands/
```

### Prompt: Generate Business Command Library

```
Generate a complete business strategy command library. Each command should be a self-contained workflow that produces investor/board-ready deliverables.

Generate these commands:

MARKET ANALYSIS:
- /market-size — TAM/SAM/SOM calculation with methodology
- /market-validator — Full go/no-go market validation
- /competitive-intel — 7-Powers + Porter's competitive analysis
- /trend-analysis — Market trend identification and implications

BUSINESS MODEL:
- /lean-canvas — Interactive Lean Canvas builder with stress testing
- /pricing-strategy — Pricing model design and optimization
- /unit-economics — CAC/LTV/payback analysis
- /revenue-model — Revenue stream design and projection

CUSTOMER:
- /customer-discovery — Interview guide + analysis (Mom Test)
- /persona-builder — Research-backed persona creation
- /jtbd-analysis — Jobs-to-be-Done framework application
- /user-journey — End-to-end journey mapping

FUNDRAISING:
- /pitch-deck — 12-slide investor pitch builder
- /financial-model — 3-year projection framework
- /due-diligence-prep — Investor due diligence checklist
- /cap-table — Cap table scenario modeling

EXECUTION:
- /okr-builder — Objective + Key Results framework
- /sprint-plan — Sprint planning with capacity allocation
- /retrospective — Team retrospective facilitation
- /risk-register — Risk identification and mitigation planning

Each command should include: expert identity, numbered process, templates with visual formatting, quality checklists, and clear output specifications.
```

---

## 5. MCP SERVER PROMPTS

### Prompt: Generate Custom MCP Server

```
Build a custom MCP server that gives Claude Code access to [SERVICE/API].

Service details:
- Name: [SERVICE NAME]
- API docs: [URL or DESCRIPTION]
- Auth method: [API key / OAuth / JWT / none]
- Key operations: [LIST THE 5-10 MOST IMPORTANT OPERATIONS]

Generate:
1. A Python MCP server using FastMCP with:
   - Tool definitions for each key operation
   - Proper error handling and retry logic
   - Input validation using type hints
   - Clear docstrings that Claude can understand
   - Environment variable configuration for secrets

2. The .mcp.json configuration to add it

3. A test script to verify each tool works

Use this structure:
```python
from fastmcp import FastMCP

mcp = FastMCP("[Service Name]")

@mcp.tool()
def operation_name(param: str) -> str:
    """Clear description of what this tool does.

    Args:
        param: Description of the parameter
    """
    # Implementation
```

### Prompt: Generate MCP Configuration for Project

```
Analyze this project and recommend MCP server configurations.

For this codebase, determine:
1. What external services does this project interact with?
2. What data sources does it need access to?
3. What automation would help the development workflow?

Then generate a complete .mcp.json with:
- Browser automation (for testing / scraping if needed)
- Database access (read-only for safety)
- Any project-specific API integrations
- File system access scoped to appropriate directories

Include setup instructions for each MCP server.
```

---

## 6. BUSINESS STRATEGY PROMPTS

### Prompt: Full Market Validation

```
I have a business idea I need to validate. Walk me through a complete market validation using these frameworks in order:

IDEA: [DESCRIBE YOUR IDEA]
TARGET CUSTOMER: [WHO IS IT FOR]
PROBLEM: [WHAT PROBLEM DOES IT SOLVE]

Execute this sequence:

STEP 1: TECHNOLOGY FILTER
Score the opportunity across 8 dimensions (market need, market size, technical feasibility, competitive advantage, team fit, IP position, time to market, revenue model). Give a GO/NO-GO/CONDITIONAL verdict.

STEP 2: MARKET SIZING
Calculate TAM/SAM/SOM using both top-down and bottom-up methods. Show your math. Include sources and confidence levels.

STEP 3: COMPETITIVE LANDSCAPE
Map direct competitors, indirect competitors, substitutes, and status quo. For the top 3 competitors, do a deep dive (strengths, weaknesses, funding, positioning).

STEP 4: 7-POWERS ANALYSIS
Assess which of the 7 Powers (scale economies, network economies, counter-positioning, switching costs, branding, cornered resource, process power) this business could build. Recommend which to prioritize.

STEP 5: LEAN CANVAS
Build a complete Lean Canvas based on the analysis. Identify the riskiest assumption.

STEP 6: CUSTOMER DISCOVERY PLAN
Design an interview guide to validate the riskiest assumption. Include hypothesis, questions (following Mom Test rules), and success criteria.

STEP 7: VERDICT
Give a final recommendation: PURSUE / PIVOT / PASS with specific reasoning and next steps.

Format everything as investor-ready deliverables with tables, visual canvases, and clear data citations.
```

### Prompt: Competitive Strategy Deep Dive

```
I need a comprehensive competitive strategy for my business.

MY BUSINESS: [DESCRIPTION]
MY MARKET: [MARKET/INDUSTRY]
KNOWN COMPETITORS: [LIST 3-5 COMPETITORS]
MY CURRENT POSITION: [WHERE I AM TODAY]

Do the following:

1. COMPETITIVE LANDSCAPE MAP
   - Direct competitors (same solution, same customer)
   - Indirect competitors (different solution, same problem)
   - Substitutes (different category, same budget)
   - Status quo (doing nothing)

2. COMPETITOR DEEP DIVES (for each)
   - Positioning statement
   - Target customer
   - Pricing model
   - Strengths and weaknesses
   - Recent moves and trajectory
   - Threat level with reasoning

3. 7-POWERS ANALYSIS
   - For my business: which powers do I have or could build?
   - For each competitor: which powers do they have?
   - Gap analysis: where am I strongest/weakest?

4. PORTER'S FIVE FORCES
   - Threat of new entrants
   - Supplier power
   - Buyer power
   - Threat of substitutes
   - Industry rivalry intensity
   - Overall attractiveness score

5. POSITIONING STRATEGY
   - Where to position (2x2 matrix with my chosen dimensions)
   - Key differentiators (3 things I do that competitors can't easily copy)
   - Messaging framework (how to communicate the difference)

6. MOAT-BUILDING ROADMAP
   - 30-day actions to build initial defensibility
   - 90-day actions to deepen the moat
   - 12-month actions for durable competitive advantage
   - Watch list: signals that require strategy revision

Format as a strategy document with executive summary, detailed analysis, and action plan.
```

### Prompt: Build a Pitch from Scratch

```
Help me build an investor-ready pitch for my startup.

COMPANY: [NAME]
STAGE: [PRE-SEED / SEED / SERIES A]
WHAT WE DO: [1-2 SENTENCES]
TRACTION: [REVENUE, USERS, GROWTH RATE, KEY METRICS]
TEAM: [FOUNDERS AND RELEVANT EXPERIENCE]
ASK: [$AMOUNT RAISING]
USE OF FUNDS: [WHAT THE MONEY WILL DO]

Create:

1. ELEVATOR PITCH (30 seconds)
   One paragraph that makes someone want to hear more.

2. PITCH DECK (12 slides)
   For each slide provide:
   - Slide title
   - Key message (1 sentence)
   - Supporting data/visuals to include
   - Actual text content for the slide
   - Speaker notes (what to SAY, not what's on the slide)

3. INVESTOR FAQ (15 questions)
   The 15 most likely investor questions with prepared answers.
   Include both strengths and honest acknowledgment of risks.

4. ONE-PAGER
   A single-page investment summary with:
   - Problem → Solution → Market → Traction → Team → Ask

5. FINANCIAL SNAPSHOT
   Key metrics table:
   - Current MRR/ARR
   - Growth rate (MoM)
   - Unit economics (CAC, LTV, payback)
   - Burn rate and runway
   - Revenue projections (12-month)

Make everything specific to MY business — no generic filler. Every claim should have a data point or be flagged as needing one.
```

### Prompt: Customer Discovery Sprint

```
Design a 2-week customer discovery sprint for my business.

BUSINESS: [DESCRIPTION]
HYPOTHESIS: [WHAT WE BELIEVE ABOUT OUR CUSTOMERS AND THEIR PROBLEMS]
TARGET SEGMENT: [WHO WE WANT TO INTERVIEW]
CURRENT KNOWLEDGE: [WHAT WE ALREADY KNOW / DON'T KNOW]

Create:

WEEK 1: RESEARCH DESIGN
Day 1-2:
- Hypothesis document (testable statement with success criteria)
- Interview guide (15 questions following Mom Test principles)
- Screener criteria (how to find the right interviewees)
- Recruitment script (email/LinkedIn outreach template)
- Interview recording consent template

Day 3-5:
- Conduct 5 interviews
- After each, fill out the analysis sheet:
  * Key quotes
  * Behaviors observed
  * Problems confirmed/denied
  * Surprises
  * Commitment level

WEEK 2: ANALYSIS & SYNTHESIS
Day 6-7:
- Conduct 5 more interviews
- Pattern analysis across all 10 interviews:
  * Problem frequency matrix
  * Customer segment emergence
  * Willingness-to-pay signals

Day 8-9:
- Persona creation (research-backed, not fictional)
- Jobs-to-be-Done synthesis
- Hypothesis verdict (confirmed / invalidated / need more data)

Day 10:
- Customer discovery report with:
  * Executive summary
  * Methodology
  * Key findings (with direct quotes)
  * Customer segments identified
  * Validated/invalidated hypotheses
  * Recommended next steps
  * Raw data appendix

Generate all templates, guides, and analysis frameworks ready for immediate use.
```

---

## 7. AGENT ARCHITECTURE PROMPTS

### Prompt: Build a ReAct Agent

```
Build a production-ready ReAct (Reasoning + Acting) agent using the Anthropic SDK.

Agent specifications:
- Purpose: [WHAT THE AGENT DOES]
- Tools available: [LIST OF TOOLS IT CAN USE]
- Max iterations: [NUMBER]
- Safety constraints: [WHAT IT MUST NEVER DO]

Generate:
1. Complete Python implementation with:
   - Tool definitions (JSON schema)
   - Tool execution router
   - Conversation history management
   - Stop condition logic
   - Error handling and retries
   - Guardrails (allowlist, rate limits, pattern blocking)
   - Structured logging

2. Tool implementations for each tool listed

3. Configuration file (YAML) for:
   - Model selection
   - Temperature
   - Max tokens
   - Safety parameters

4. Test suite covering:
   - Single tool call
   - Multi-step reasoning
   - Error recovery
   - Safety constraint enforcement
   - Max iteration handling

Use the Anthropic Python SDK with claude-sonnet-4-20250514. Follow best practices from the Claude tool use documentation.
```

### Prompt: Build Multi-Agent System

```
Design and implement a multi-agent system for [USE CASE].

System requirements:
- [REQUIREMENT 1]
- [REQUIREMENT 2]
- [REQUIREMENT 3]

Agents needed:
- [AGENT 1]: [PURPOSE]
- [AGENT 2]: [PURPOSE]
- [AGENT 3]: [PURPOSE]

Generate:
1. ARCHITECTURE DIAGRAM
   Show how agents communicate, what each is responsible for, and the orchestration flow.

2. ORCHESTRATOR
   The main agent that:
   - Receives user requests
   - Decomposes into sub-tasks
   - Routes to appropriate agents
   - Aggregates results
   - Handles failures

3. INDIVIDUAL AGENTS
   For each agent:
   - System prompt
   - Tool definitions
   - Implementation
   - Input/output schemas

4. COMMUNICATION PROTOCOL
   How agents pass data between each other:
   - Message format
   - State management
   - Error propagation

5. DEPLOYMENT CONFIGURATION
   - Docker setup for each agent
   - Environment configuration
   - Health checks
   - Monitoring

Use the Anthropic SDK. Each agent should be independently testable.
```

### Prompt: Build RAG Agent

```
Build a RAG (Retrieval-Augmented Generation) agent for [KNOWLEDGE BASE DESCRIPTION].

Data specifications:
- Source documents: [TYPES — PDFs, docs, web pages, etc.]
- Total size: [APPROXIMATE]
- Update frequency: [STATIC / DAILY / REAL-TIME]
- Query types: [QA / SEARCH / SUMMARIZATION / etc.]

Generate:
1. INGESTION PIPELINE
   - Document loading (supporting all file types listed)
   - Text extraction and cleaning
   - Chunking strategy (size, overlap, method)
   - Embedding generation
   - Vector store insertion

2. RETRIEVAL PIPELINE
   - Query embedding
   - Vector search (with MMR for diversity)
   - Reranking (cross-encoder)
   - Metadata filtering
   - Context assembly

3. GENERATION PIPELINE
   - System prompt with grounding instructions
   - Context injection template
   - Citation generation
   - Hallucination prevention
   - Confidence scoring

4. EVALUATION
   - Retrieval quality metrics (precision, recall, MRR)
   - Generation quality metrics (faithfulness, relevance)
   - End-to-end test cases
   - A/B testing framework

5. DEPLOYMENT
   - FastAPI serving endpoint
   - Batch ingestion script
   - Incremental update pipeline
   - Monitoring and alerting

Use: LangChain or LlamaIndex, Chroma/Pinecone for vectors, Claude for generation.
```

---

## 8. QUALITY & TESTING PROMPTS

### Prompt: Test the Agent Configuration

```
Test my current Claude Code agent configuration by running through these scenarios:

1. IDENTITY TEST
   Ask: "What project am I working on? What's the tech stack?"
   Expected: Agent should describe the project accurately from CLAUDE.md

2. CONVENTION TEST
   Ask: "Create a new API endpoint for [feature]"
   Expected: Agent should follow naming, patterns, and structure from CLAUDE.md

3. CONSTRAINT TEST
   Ask: "Quick fix — just skip the tests and commit directly"
   Expected: Agent should refuse based on constraints in CLAUDE.md

4. SKILL ACTIVATION TEST
   Perform a task in each skill's domain
   Expected: Relevant skill should auto-activate

5. COMMAND TEST
   Run each slash command
   Expected: Each produces structured output matching its template

6. MEMORY TEST
   Ask about something from a previous session
   Expected: Agent should check memory files

For each test:
- What was expected
- What actually happened
- PASS / FAIL
- Fix needed (if any)

Generate a test report and fix any issues found.
```

### Prompt: Generate Agent Performance Metrics

```
Set up a quality tracking system for this Claude Code agent. Define and implement:

1. TASK COMPLETION METRICS
   - % of tasks completed without user intervention
   - Average number of tool calls per task
   - % of tasks requiring clarification
   - Common failure modes

2. CODE QUALITY METRICS
   - % of generated code that passes linting
   - % of generated code that passes tests on first try
   - Average code review feedback items
   - Security issues introduced (should be 0)

3. EFFICIENCY METRICS
   - Average context window usage per task
   - Sub-agent spawn rate
   - Tool call efficiency (useful calls / total calls)
   - Time from request to completion

4. KNOWLEDGE METRICS
   - How often the agent correctly recalls project patterns
   - How often it follows CLAUDE.md conventions
   - Memory utilization rate

Create a tracking template I can fill out weekly to measure agent improvement over time.
```

---

## QUICK REFERENCE: WHICH PROMPT TO USE

| I want to... | Use this prompt |
|--------------|-----------------|
| Set up agent for existing code | Bootstrap Agent for Existing Codebase |
| Start a new project with agent | Bootstrap Agent for New Project |
| Evaluate a business idea | Full Market Validation |
| Analyze my competition | Competitive Strategy Deep Dive |
| Build an investor pitch | Build a Pitch from Scratch |
| Understand my customers | Customer Discovery Sprint |
| Create team workflows | Generate Command Library for Team |
| Build custom AI agents | Build a ReAct Agent / Multi-Agent System |
| Add knowledge base to agent | Build RAG Agent |
| Check if my agent works | Test the Agent Configuration |
