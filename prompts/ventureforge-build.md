# PROMPT: Build VentureForge — AI Venture Assessment Agent

> Copy everything below the line into Claude Code in a new empty directory.

---

# BUILD: VentureForge

You are building **VentureForge** — an AI-powered venture assessment agent that helps founders and VC analysts evaluate startups using real-time data.

## WHAT TO BUILD

VentureForge orchestrates 5 research tools through a **LangGraph ReAct agent with Claude**, delivering structured analyses with confidence scores and source citations.

### The 5 Research Tools

1. **Market Sizing (BLS API)** — Pull industry data from Bureau of Labor Statistics to calculate TAM/SAM/SOM with real employment, wage, and industry output numbers

2. **Competitive Analysis** — Scrape and analyze competitor landscape: funding rounds (Crunchbase-style), positioning, product comparisons, market share estimates

3. **Investment Scoring** — Score startups across a weighted rubric: team, market, product, traction, financials, moat (using the 7-Powers framework from the MSTC innovation curriculum)

4. **SEC Filing Analysis (EDGAR API)** — Pull and analyze SEC filings for comparable public companies: revenue trends, margin profiles, growth rates, risk factors

5. **Economic Indicators (FRED API)** — Pull Federal Reserve data: GDP growth, industry-specific indicators, interest rates, consumer confidence — to contextualize market timing

### Business Framework Integration

This agent is powered by the frameworks from https://github.com/ViggyV/mstc-innovation — specifically:

- **Technology Filter** (8-dimension scoring) → feeds the Investment Scoring tool
- **7-Powers Framework** (Hamilton Helmer) → competitive moat section of every analysis
- **Lean Canvas** → structured business model output
- **TAM/SAM/SOM methodology** → Market Sizing tool output format
- **Customer Discovery principles** → informs the "Problem Validation" section
- **Competitive Intelligence workflow** → direct/indirect/substitute mapping

Clone and read that repo to understand the frameworks, then encode them into the agent's tools and prompts.

## ARCHITECTURE

```
ventureforge/
├── app/
│   ├── __init__.py
│   ├── main.py                    # FastAPI app entry point
│   ├── agent/
│   │   ├── __init__.py
│   │   ├── graph.py               # LangGraph ReAct agent definition
│   │   ├── state.py               # Agent state schema (TypedDict)
│   │   ├── nodes.py               # Agent nodes (reasoning, tool execution)
│   │   └── prompts.py             # System prompts with framework knowledge
│   ├── tools/
│   │   ├── __init__.py
│   │   ├── market_sizing.py       # BLS API integration
│   │   ├── competitive.py         # Competitive analysis tool
│   │   ├── scoring.py             # Investment scoring (7-Powers + Technology Filter)
│   │   ├── sec_edgar.py           # SEC EDGAR filing analysis
│   │   └── fred.py                # FRED economic indicators
│   ├── models/
│   │   ├── __init__.py
│   │   ├── startup.py             # Startup input schema
│   │   ├── analysis.py            # Analysis output schema
│   │   └── scores.py              # Scoring models with confidence
│   ├── services/
│   │   ├── __init__.py
│   │   ├── report_builder.py      # Assembles final analysis report
│   │   └── citation_tracker.py    # Tracks and formats source citations
│   └── config.py                  # Settings (API keys, model config)
├── tests/
│   ├── __init__.py
│   ├── test_tools/
│   │   ├── test_market_sizing.py
│   │   ├── test_competitive.py
│   │   ├── test_scoring.py
│   │   ├── test_sec_edgar.py
│   │   └── test_fred.py
│   ├── test_agent/
│   │   ├── test_graph.py
│   │   └── test_nodes.py
│   └── test_e2e/
│       └── test_full_analysis.py
├── pyproject.toml
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── CLAUDE.md
└── README.md
```

## IMPLEMENTATION DETAILS

### 1. Agent Graph (LangGraph)

Build a ReAct agent using LangGraph with this flow:

```
User Input (startup description)
        │
        ▼
┌───────────────┐
│   REASONING   │ ← Claude decides which tool to call next
│   (claude)    │
└───────┬───────┘
        │
        ├──→ market_sizing() ──→ BLS API ──→ TAM/SAM/SOM numbers
        ├──→ competitive()   ──→ Web data ──→ competitor landscape
        ├──→ scoring()       ──→ 7-Powers + Technology Filter ──→ weighted score
        ├──→ sec_edgar()     ──→ EDGAR API ──→ comparable company financials
        ├──→ fred()          ──→ FRED API  ──→ economic context
        │
        ▼
┌───────────────┐
│  SYNTHESIZE   │ ← Claude combines all tool results
│  (claude)     │
└───────┬───────┘
        │
        ▼
   Structured Analysis Report
   (scores, citations, confidence levels)
```

Use `langgraph` with:
```python
from langgraph.graph import StateGraph, START, END
from langgraph.prebuilt import ToolNode
from langchain_anthropic import ChatAnthropic
```

Model: `claude-sonnet-4-20250514`

The agent should:
- Decide autonomously which tools to call and in what order
- Call multiple tools as needed (not a fixed pipeline)
- Synthesize all gathered data into a single structured report
- Include confidence scores (HIGH/MEDIUM/LOW) for each section based on data quality
- Cite every data point with its source (API, filing, URL)

### 2. Tool Implementations

**market_sizing.py** — BLS API
```python
# Use BLS Public Data API v2: https://api.bls.gov/publicAPI/v2/timeseries/data/
# Pull: industry employment, wages, output by NAICS code
# Calculate: TAM (total industry), SAM (addressable segment), SOM (capturable)
# Return: structured MarketSize model with methodology and sources
# API key via BLS_API_KEY env var
```

**competitive.py** — Competitive Analysis
```python
# Search for competitors using web search
# For each competitor extract: name, funding, positioning, strengths, weaknesses
# Map: direct vs indirect vs substitutes vs status quo
# Apply 7-Powers framework to assess competitive moats
# Return: CompetitiveLandscape model with competitor profiles
```

**scoring.py** — Investment Scoring
```python
# Encode the Technology Filter (8 dimensions, score 1-5 each):
#   market_need, market_size, technical_feasibility, competitive_advantage,
#   team_fit, ip_position, time_to_market, revenue_model
# Encode 7-Powers assessment:
#   scale_economies, network_economies, counter_positioning,
#   switching_costs, branding, cornered_resource, process_power
# Weight and combine into overall score /100
# Return: InvestmentScore with dimension breakdown and verdict
```

**sec_edgar.py** — SEC EDGAR
```python
# Use EDGAR full-text search API: https://efts.sec.gov/LATEST/search-index?q=
# And company filings API: https://data.sec.gov/submissions/CIK{cik}.json
# Pull 10-K and 10-Q filings for comparable public companies
# Extract: revenue, growth rate, margins, risk factors
# Return: ComparableAnalysis model with financial benchmarks
# Set User-Agent header per SEC requirements
```

**fred.py** — FRED API
```python
# Use FRED API: https://api.stlouisfed.org/fred/series/observations
# Pull relevant series: GDP (GDPC1), CPI (CPIAUCSL), unemployment (UNRATE),
#   consumer confidence (UMCSENT), industry-specific indicators
# Contextualize: is the macro environment favorable for this type of business?
# Return: EconomicContext model with indicators and interpretation
# API key via FRED_API_KEY env var
```

### 3. Output Schema

Every analysis produces this structure:

```python
class VentureAnalysis(BaseModel):
    # Header
    startup_name: str
    analysis_date: datetime
    analyst: str = "VentureForge AI"
    overall_score: float          # 0-100
    overall_confidence: str       # HIGH / MEDIUM / LOW
    verdict: str                  # STRONG INVEST / INVEST / CONDITIONAL / PASS

    # Sections
    executive_summary: str        # 3-sentence summary
    market_sizing: MarketSize     # TAM/SAM/SOM with methodology
    competitive_landscape: CompetitiveLandscape  # Map + profiles
    investment_score: InvestmentScore  # Technology Filter + 7-Powers
    comparable_analysis: ComparableAnalysis  # SEC filing benchmarks
    economic_context: EconomicContext  # FRED macro indicators
    lean_canvas: LeanCanvas       # Auto-generated business model

    # Meta
    key_risks: list[Risk]         # Top 5 risks with mitigation
    next_steps: list[str]         # Recommended actions
    sources: list[Citation]       # Every data source cited
    methodology: str              # How the analysis was conducted
```

### 4. System Prompt

The agent's system prompt should encode:
- The Technology Filter scoring rubric (from mstc-innovation)
- The 7-Powers framework definitions (from mstc-innovation)
- TAM/SAM/SOM calculation methodology (from mstc-innovation)
- Competitive mapping categories (direct/indirect/substitute/status quo)
- Mom Test principles for evaluating customer evidence
- Instructions to ALWAYS cite sources and include confidence levels
- Instructions to flag assumptions explicitly

### 5. API Server

FastAPI with these endpoints:
```
POST /analyze              # Full venture analysis (async, returns job ID)
GET  /analyze/{job_id}     # Get analysis status/results
POST /analyze/quick        # Quick score only (synchronous, just scoring tool)
GET  /health               # Health check
GET  /tools                # List available research tools
```

### 6. Configuration

```
# .env.example
ANTHROPIC_API_KEY=your-key
BLS_API_KEY=your-key           # https://data.bls.gov/registrationEngine/
FRED_API_KEY=your-key          # https://fred.stlouisfed.org/docs/api/api_key.html
SEC_USER_AGENT=your-email      # Required by SEC for EDGAR API
```

## BUILD SEQUENCE

Execute in this order:

1. **Project setup**: pyproject.toml, directory structure, .env.example
2. **Models**: All Pydantic schemas (startup input, analysis output, scores)
3. **Tools**: Implement each of the 5 tools with real API calls
4. **Agent**: LangGraph ReAct graph with Claude, wiring in all tools
5. **Report builder**: Synthesize tool outputs into VentureAnalysis
6. **API server**: FastAPI endpoints
7. **Tests**: Unit tests for each tool, integration test for agent, e2e test
8. **CLAUDE.md**: Agent config for developing on this repo
9. **README**: Setup, usage, API docs
10. **Docker**: Dockerfile + docker-compose.yml

For each step: write the code, then verify it works before moving to the next.

## CONSTRAINTS

- All API calls must handle rate limits and failures gracefully
- Every data point in the output must have a source citation
- Never fabricate data — if an API fails, say "data unavailable" with confidence: LOW
- SEC EDGAR requires a User-Agent header with contact email
- BLS and FRED have rate limits — implement backoff
- The agent should complete a full analysis in under 60 seconds
- All scores must show their methodology (not just a number)
- Use async where possible for parallel API calls

## START

Begin with step 1 (project setup) and work through sequentially. Ask me for API keys when you need them for testing. Show me the project structure after setup, then proceed to implementation.
