# PROMPT: Build a Knowledge Base Agent for Any Document Collection

> Copy everything below the line into Claude Code. Point it at your document folder.

---

Transform this folder of documents into an intelligent knowledge agent. The agent should be able to answer questions, synthesize information, and apply frameworks found in the documents.

## MY DOCUMENTS

**Location**: [FOLDER PATH]
**Content type**: [Course materials / Research papers / Company docs / Training materials / etc.]
**File types present**: [PDF / DOCX / XLSX / PPTX / MD / etc.]
**Total files**: [APPROXIMATE NUMBER]
**Topics covered**: [BRIEF DESCRIPTION OF WHAT THE DOCUMENTS ARE ABOUT]

## PHASE 1: DOCUMENT INVENTORY

Scan the folder and create a complete inventory:

1. **List every file** with: path, type, size, and inferred topic
2. **Identify structure**: How are files organized? (by topic, date, author, session, etc.)
3. **Detect key documents**: Which files are the most important? (templates, frameworks, core references)
4. **Map relationships**: Which documents reference or build on each other?

## PHASE 2: GENERATE CLAUDE.md

Create a CLAUDE.md in the folder root that configures this as a knowledge agent:

```markdown
# [Knowledge Base Name] — Knowledge Agent

## CONTENT MAP
[Table of all documents organized by category/topic]

## KEY CONCEPTS
[List of major frameworks, concepts, or themes with source document references]

## DOCUMENT TYPES & ACCESS
[Table: file type, count, how to read each type]

## CONVENTIONS
### When Answering Questions
1. Always cite the specific source document
2. Use direct quotes when possible
3. Cross-reference across documents when topics connect
4. Clearly flag when information is NOT in the materials

### When Synthesizing
1. Read primary sources first
2. Identify key themes and connections
3. Present in structured format
4. Link to source files for deeper reading

## WORKFLOW TRIGGERS
[Table: "User says X" → "Agent does Y"]
```

## PHASE 3: GENERATE COMMANDS

Create slash commands for the most common queries:

1. **`/explore-topic`** — Deep dive into a specific topic across all documents
2. **`/summarize`** — Generate structured summary of a document or category
3. **`/find-framework`** — Locate and explain a specific framework or concept
4. **`/compare`** — Compare two concepts, approaches, or documents
5. **`/apply`** — Apply a framework from the documents to a new context

## PHASE 4: TEST

Verify the agent works by:
1. Asking a factual question → confirm it cites the correct source
2. Asking a synthesis question → confirm it pulls from multiple sources
3. Asking about something NOT in the docs → confirm it says so
4. Asking to apply a framework → confirm it reads the source first

Report results and fix any issues.
