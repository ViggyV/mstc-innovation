# 06 - Agent Patterns

Production-ready patterns for building AI agents, from simple tool-users to complex multi-agent orchestrations.

## Pattern 1: ReAct Agent (Reasoning + Acting)

The foundation of most Claude Code workflows. The agent reasons about what to do, takes an action, observes the result, and repeats.

```
┌─────────────────────────────────────┐
│           REACT LOOP                │
├─────────────────────────────────────┤
│  ┌─────────┐     ┌─────────┐       │
│  │ Observe │────▶│ Think   │       │
│  └─────────┘     └────┬────┘       │
│       ▲               │             │
│       │          ┌────▼────┐       │
│       │          │  Act    │       │
│       │          └────┬────┘       │
│       │               │             │
│       └───────────────┘             │
│          (loop until done)          │
└─────────────────────────────────────┘
```

### Implementation (Anthropic SDK)

```python
from anthropic import Anthropic

class ReActAgent:
    def __init__(self, system_prompt: str, tools: list[dict]):
        self.client = Anthropic()
        self.system = system_prompt
        self.tools = tools
        self.messages = []

    def run(self, task: str, max_turns: int = 15) -> str:
        self.messages = [{"role": "user", "content": task}]

        for _ in range(max_turns):
            response = self.client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=4096,
                system=self.system,
                tools=self.tools,
                messages=self.messages
            )

            if response.stop_reason == "end_turn":
                return self._extract_text(response)

            if response.stop_reason == "tool_use":
                self.messages.append({"role": "assistant", "content": response.content})
                tool_results = []
                for block in response.content:
                    if block.type == "tool_use":
                        result = self._execute(block.name, block.input)
                        tool_results.append({
                            "type": "tool_result",
                            "tool_use_id": block.id,
                            "content": result
                        })
                self.messages.append({"role": "user", "content": tool_results})

        return "Max turns reached"

    def _execute(self, tool_name: str, args: dict) -> str:
        # Route to your tool implementations
        ...

    def _extract_text(self, response) -> str:
        return "".join(b.text for b in response.content if hasattr(b, "text"))
```

## Pattern 2: Multi-Agent Orchestration

Multiple specialized agents coordinated by an orchestrator. Claude Code's sub-agent system does this natively.

```
┌─────────────────────────────────────────────┐
│              ORCHESTRATOR                    │
│         (main Claude Code session)           │
├─────────────────────────────────────────────┤
│                   │                          │
│    ┌──────────────┼──────────────┐          │
│    ▼              ▼              ▼          │
│ ┌──────┐     ┌──────┐      ┌──────┐       │
│ │Explore│    │Plan  │      │General│       │
│ │Agent  │    │Agent │      │Agent  │       │
│ │(fast  │    │(arch │      │(code  │       │
│ │search)│    │design)│     │ write)│       │
│ └──────┘     └──────┘      └──────┘       │
│    │              │              │          │
│    └──────────────┼──────────────┘          │
│                   ▼                          │
│           Merged Results                     │
└─────────────────────────────────────────────┘
```

### When to Use Sub-Agents

| Use Sub-Agent | Don't Use Sub-Agent |
|---------------|---------------------|
| Parallel independent searches | Simple file read |
| Deep codebase exploration | Known file path |
| Complex multi-step research | Single grep/glob |
| Protecting main context | Quick lookups |

### Claude Code Sub-Agent Usage

```
# In your CLAUDE.md or skill, instruct the agent:

When the user asks to refactor a module:
1. Spawn an Explore agent to map all usages of the module
2. Spawn a Plan agent to design the refactoring approach
3. Execute the refactoring in the main session
4. Spawn a general-purpose agent to run tests
```

## Pattern 3: RAG-Enhanced Agent

Agent that retrieves relevant context before generating responses.

```
┌──────────────────────────────────────────────┐
│             RAG AGENT FLOW                    │
├──────────────────────────────────────────────┤
│                                               │
│  User Query                                   │
│      │                                        │
│      ▼                                        │
│  ┌──────────┐    ┌──────────┐                │
│  │ Embed    │───▶│ Vector   │                │
│  │ Query    │    │ Search   │                │
│  └──────────┘    └────┬─────┘                │
│                       │                       │
│                  ┌────▼─────┐                │
│                  │ Retrieve │                │
│                  │ Top-K    │                │
│                  └────┬─────┘                │
│                       │                       │
│  ┌──────────┐    ┌────▼─────┐                │
│  │ System   │───▶│ Generate │                │
│  │ Prompt   │    │ Response │                │
│  └──────────┘    └────┬─────┘                │
│                       │                       │
│                  ┌────▼─────┐                │
│                  │ Cite     │                │
│                  │ Sources  │                │
│                  └──────────┘                │
└──────────────────────────────────────────────┘
```

### Implementation with LangChain

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatAnthropic

# Build the knowledge base
vectorstore = Chroma.from_documents(
    documents=chunked_docs,
    embedding=OpenAIEmbeddings(),
    persist_directory="./chroma_db"
)

# Create the RAG chain
qa = RetrievalQA.from_chain_type(
    llm=ChatAnthropic(model="claude-sonnet-4-20250514"),
    chain_type="stuff",
    retriever=vectorstore.as_retriever(search_kwargs={"k": 5}),
    return_source_documents=True
)

# Query
result = qa.invoke("What is the refund policy?")
print(result["result"])
print(result["source_documents"])
```

## Pattern 4: Pipeline Agent

Sequential processing with checkpoints. Good for data transformation, build processes, or content generation.

```
┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐
│Ingest│───▶│Clean │───▶│Trans-│───▶│Valid-│───▶│Output│
│      │    │      │    │form  │    │ate   │    │      │
└──────┘    └──────┘    └──────┘    └──────┘    └──────┘
   │            │            │           │           │
   ▼            ▼            ▼           ▼           ▼
checkpoint  checkpoint  checkpoint  checkpoint   final
```

### Implementation

```python
class PipelineAgent:
    def __init__(self):
        self.steps = []
        self.checkpoints = {}

    def add_step(self, name: str, fn):
        self.steps.append((name, fn))

    def run(self, data):
        for name, fn in self.steps:
            print(f"Running: {name}")
            data = fn(data)
            self.checkpoints[name] = data
            print(f"Checkpoint saved: {name}")
        return data

    def resume_from(self, step_name: str):
        """Resume pipeline from a checkpoint."""
        data = self.checkpoints.get(step_name)
        if not data:
            raise ValueError(f"No checkpoint for {step_name}")
        # Find step index and continue from there
        ...
```

## Pattern 5: Conversational Agent with State

For chatbots and interactive assistants that maintain context.

```python
from enum import Enum
from dataclasses import dataclass, field

class Phase(Enum):
    GREETING = "greeting"
    DISCOVERY = "discovery"
    ANALYSIS = "analysis"
    RECOMMENDATION = "recommendation"
    FOLLOWUP = "followup"

@dataclass
class ConversationState:
    phase: Phase = Phase.GREETING
    context: dict = field(default_factory=dict)
    history: list = field(default_factory=list)

    def transition(self, user_input: str) -> Phase:
        """Determine next phase based on current state and input."""
        transitions = {
            Phase.GREETING: Phase.DISCOVERY,
            Phase.DISCOVERY: Phase.ANALYSIS,
            Phase.ANALYSIS: Phase.RECOMMENDATION,
            Phase.RECOMMENDATION: Phase.FOLLOWUP,
        }
        self.phase = transitions.get(self.phase, Phase.FOLLOWUP)
        return self.phase
```

## Pattern 6: Guardrailed Agent

Agent with safety controls, rate limiting, and audit logging.

```python
import re
from functools import wraps

class GuardrailedAgent:
    MAX_TOOL_CALLS = 50
    FORBIDDEN_PATTERNS = [r"rm\s+-rf", r"DROP\s+TABLE", r"sudo"]
    ALLOWED_TOOLS = {"read_file", "search", "analyze"}

    def __init__(self, agent):
        self.agent = agent
        self.call_count = 0
        self.audit_log = []

    def execute_tool(self, name: str, args: dict) -> str:
        # Check allowlist
        if name not in self.ALLOWED_TOOLS:
            self._log("BLOCKED", f"Tool {name} not in allowlist")
            return f"Error: Tool {name} not allowed"

        # Check rate limit
        self.call_count += 1
        if self.call_count > self.MAX_TOOL_CALLS:
            self._log("RATE_LIMITED", f"Exceeded {self.MAX_TOOL_CALLS} calls")
            return "Error: Rate limit exceeded"

        # Check for dangerous patterns
        args_str = str(args)
        for pattern in self.FORBIDDEN_PATTERNS:
            if re.search(pattern, args_str, re.IGNORECASE):
                self._log("BLOCKED", f"Dangerous pattern: {pattern}")
                return "Error: Potentially dangerous operation blocked"

        # Execute and log
        result = self.agent.execute_tool(name, args)
        self._log("EXECUTED", f"{name}({args}) -> {result[:100]}")
        return result

    def _log(self, event: str, detail: str):
        self.audit_log.append({"event": event, "detail": detail})
```

## Choosing the Right Pattern

| Scenario | Pattern | Why |
|----------|---------|-----|
| General coding assistant | ReAct | Flexible reasoning + tool use |
| Large codebase refactor | Multi-Agent | Parallel search + planning |
| Knowledge base Q&A | RAG | Grounded in your documents |
| Data processing workflow | Pipeline | Sequential with checkpoints |
| Customer support bot | Conversational | State management + personality |
| Production deployment | Guardrailed | Safety + audit trail |

## Combining Patterns

Real-world agents often combine multiple patterns:

```
┌─────────────────────────────────────────┐
│          PRODUCTION AGENT               │
├─────────────────────────────────────────┤
│                                          │
│  ┌──────────┐  wraps  ┌──────────┐     │
│  │Guardrails│────────▶│  ReAct   │     │
│  │(safety)  │         │  Agent   │     │
│  └──────────┘         └────┬─────┘     │
│                            │             │
│              ┌─────────────┼─────────┐  │
│              ▼             ▼         ▼  │
│         ┌────────┐   ┌────────┐  ┌─────┐│
│         │  RAG   │   │Sub-    │  │Tool ││
│         │Retrieval│  │Agents  │  │Exec ││
│         └────────┘   └────────┘  └─────┘│
│                                          │
└─────────────────────────────────────────┘
```
