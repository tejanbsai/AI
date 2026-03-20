# Complete End-to-End Architecture of AI Applications

> **Domain**: Banking & Financial Services  
> **Created**: March 20, 2026  
> **Purpose**: Full architectural reference for AI-powered applications  
> **Audience**: QA Engineers, Test Architects, Developers, AI Engineers

---

## Table of Contents

- [1. The 3 Main AI Application Patterns](#1-the-3-main-ai-application-patterns)
- [2. Complete E2E Architecture — Every Component](#2-complete-e2e-architecture--every-component)
- [3. E2E Flow — Step by Step (Banking Example)](#3-e2e-flow--step-by-step-banking-example)
- [4. The Data Pipeline (Offline / Background)](#4-the-data-pipeline-offline--background)
- [5. Agent Loop — Detailed Flow](#5-agent-loop--detailed-flow)
- [6. What Breaks Where — Test Points](#6-what-breaks-where--test-points)
- [7. Architecture Variants by Scale](#7-architecture-variants-by-scale)

---

## 1. The 3 Main AI Application Patterns

Before diving deep, understand that most AI products in enterprise follow one of three patterns:

```
PATTERN 1: SIMPLE LLM APP (Chatbot)
════════════════════════════════════
User → Guardrail → LLM → Guardrail → Response

Example: Customer support chatbot with fixed knowledge in system prompt
Complexity: Low
Testing difficulty: Medium


PATTERN 2: RAG APPLICATION (Knowledge Bot)
══════════════════════════════════════════
User → Guardrail → Retrieval → LLM + Context → Guardrail → Response
                      ↑
                  Knowledge Base

Example: Banking policy bot, HR knowledge assistant
Complexity: Medium-High
Testing difficulty: High (must test retrieval + generation separately)


PATTERN 3: AI AGENT (Tool-Using System)
═══════════════════════════════════════
User → Guardrail → LLM (Planning) → Tool Call → Result → LLM (Reasoning)
                        ↑                                      │
                   Tool Registry                               ↓
                   (APIs, DBs, Functions)              Loop until done
                                                              │
                                                    Guardrail → Response

Example: Banking assistant that checks balances, transfers funds
Complexity: High
Testing difficulty: Very High (non-deterministic multi-step workflows)
```

Most enterprise AI products are **Pattern 2 (RAG) + Pattern 3 (Agent) combined**.

---

## 2. Complete E2E Architecture — Every Component

### 2.1 Traditional App vs AI App — Mindset Shift

```
┌──────────────────────────────────────────────────────────────────┐
│                    TRADITIONAL WEB APP                           │
│                                                                  │
│   User → UI → API → Business Logic → Database → Response        │
│                                                                  │
│   Testing: Deterministic. Same input = Same output. Always.     │
└──────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                    AI-POWERED BANKING APP                        │
│                                                                  │
│   User → UI → Guardrails → Retrieval → LLM → Guardrails → Resp │
│                               ↑                                  │
│                          Knowledge Base                          │
│                         (Policy Docs, FAQs)                      │
│                                                                  │
│   Testing: Non-deterministic. Same input ≈ Similar output.      │
│            Must test BEHAVIOR, not exact strings.                │
└──────────────────────────────────────────────────────────────────┘
```

**Key difference**: In traditional testing, you assert exact values.  
In AI testing, you assert **behavioral properties** — correctness, safety, groundedness, tone.

### 2.2 Full Architecture Diagram

Below is a typical architecture for an AI-powered banking assistant. Every numbered box is a **test point**.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        BANKING AI ASSISTANT                            │
│                                                                         │
│  ┌──────────┐    ┌─────────────┐    ┌──────────────┐    ┌───────────┐  │
│  │  ① USER  │───▶│ ② INPUT     │───▶│ ③ RETRIEVAL  │───▶│ ④ LLM     │  │
│  │  INPUT   │    │ GUARDRAILS  │    │   ENGINE     │    │ GENERATION│  │
│  │          │    │             │    │              │    │           │  │
│  │ - Chat   │    │ - PII scan  │    │ - Vector DB  │    │ - GPT-4 / │  │
│  │ - Voice  │    │ - Injection │    │ - Keyword    │    │   Claude  │  │
│  │ - Form   │    │   detect    │    │ - Hybrid     │    │ - System  │  │
│  │          │    │ - Rate limit│    │ - Reranker   │    │   prompt  │  │
│  └──────────┘    └─────────────┘    └──────┬───────┘    └─────┬─────┘  │
│                                            │                  │         │
│                                     ┌──────▼───────┐   ┌─────▼──────┐  │
│                                     │ ⑤ KNOWLEDGE  │   │ ⑥ OUTPUT   │  │
│                                     │    BASE      │   │ GUARDRAILS │  │
│                                     │              │   │            │  │
│                                     │ - Policy docs│   │ - Toxicity │  │
│                                     │ - Product DB │   │ - PII mask │  │
│                                     │ - Compliance │   │ - Format   │  │
│                                     │ - Rates/fees │   │ - Citation │  │
│                                     └──────────────┘   └─────┬──────┘  │
│                                                              │         │
│  ┌──────────┐    ┌─────────────┐    ┌──────────────┐         │         │
│  │ ⑨ AUDIT  │◀───│ ⑧ FEEDBACK  │◀───│ ⑦ RESPONSE   │◀────────┘         │
│  │   LOG    │    │   LOOP      │    │  DELIVERY    │                   │
│  │          │    │             │    │              │                   │
│  │ - Traces │    │ - Thumbs    │    │ - Chat UI    │                   │
│  │ - Scores │    │ - Escalate  │    │ - API JSON   │                   │
│  │ - Alerts │    │ - Retrain   │    │ - Email      │                   │
│  └──────────┘    └─────────────┘    └──────────────┘                   │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ ⑩ AGENT LAYER (if applicable)                                   │   │
│  │                                                                  │   │
│  │  LLM ──▶ Tool Selection ──▶ API Call ──▶ Result ──▶ Next Step   │   │
│  │          (balance_check)    (core bank)   ($1,234)   (respond)   │   │
│  │                                                                  │   │
│  │  Tools: balance_check | transfer | loan_calc | escalate_human   │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### 2.3 Test Point Map — What to Test at Each Layer

| # | Layer | What to Test | Test Level |
|---|---|---|---|
| ① | User Input | All input types handled (text, voice transcript, forms) | Basic |
| ② | Input Guardrails | PII detection, prompt injection blocking, rate limiting | Basic |
| ③ | Retrieval Engine | Correct documents retrieved, ranking quality | Basic → Intermediate |
| ④ | LLM Generation | Correctness, groundedness, format compliance | Intermediate |
| ⑤ | Knowledge Base | Data freshness, completeness, version control | Basic |
| ⑥ | Output Guardrails | No PII in output, toxicity filter, citation present | Basic → Intermediate |
| ⑦ | Response Delivery | Format correct for channel (JSON, chat, email) | Basic |
| ⑧ | Feedback Loop | Escalation triggers work, feedback captured | Intermediate |
| ⑨ | Audit Log | Every interaction traced, scores logged | Intermediate → Advanced |
| ⑩ | Agent Layer | Tool selection, parameter accuracy, sequence, safety | Advanced |

---

## 3. E2E Flow — Step by Step (Banking Example)

Tracing a single request through every layer:

> **User query**: "What is the interest rate on Premium Savings?"

### STEP 1 — Client Layer

```
User types message in web chat widget.
─────────────────────────────────────
Input: "What is the interest rate on Premium Savings?"
Channel: Web Chat
Action: HTTP POST to /api/chat
Payload: { "message": "...", "session_id": "abc-123", "token": "jwt-xyz" }
```

### STEP 2 — API Gateway

```
Authentication & session management.
────────────────────────────────────
✓ JWT token validated → User: employee_042
✓ Rate limit check → 15/60 requests used (under limit)
✓ Session abc-123 loaded → Turn #3 in conversation
✓ Previous history:
    Turn 1: User asked about account types
    Turn 2: User asked about savings options
```

### STEP 3 — Input Processing

```
Safety checks and query optimization.
─────────────────────────────────────

Step 3a: PII Detection
  Input: "What is the interest rate on Premium Savings?"
  Result: ✓ No PII detected
  Action: Pass through

Step 3b: Injection Detection
  Input: "What is the interest rate on Premium Savings?"
  Result: ✓ No injection patterns detected
  Action: Pass through

Step 3c: Content Classification
  Input: "What is the interest rate on Premium Savings?"
  Result: Category = "product_inquiry", Topic = "savings", Intent = "rate_check"
  Action: Pass through (on-topic)

Step 3d: Query Rewriting
  Original: "What is the interest rate on Premium Savings?"
  Context from history: User was discussing savings accounts
  Rewritten: "What is the current annual percentage yield (APY) interest
              rate on the Premium Savings account product?"
  Why: Adds specificity for better retrieval
```

### STEP 4 — Retrieval

```
Finding the right information from the knowledge base.
──────────────────────────────────────────────────────

Step 4a: Embedding
  Query text → Embedding model → [0.23, 0.87, 0.12, ..., 0.45] (1536-dim vector)

Step 4b: Vector Search
  Search vector DB with query embedding
  Returns 10 candidate chunks ranked by cosine similarity

Step 4c: Keyword Search (Hybrid)
  BM25 search for "Premium Savings" + "interest rate" + "APY"
  Returns 10 candidate chunks ranked by keyword relevance

Step 4d: Reranking
  Combine 20 candidates, deduplicate
  Cross-encoder reranker scores each chunk against the original query

  Final Top-3 chunks:
  ┌─────────────────────────────────────────────────────────────┐
  │ Chunk 1 (score: 0.94)                                      │
  │ Source: products/savings-accounts.pdf, page 4               │
  │ "Premium Savings Account offers a competitive 4.25% APY.   │
  │ Available to customers with a minimum opening deposit       │
  │ of $5,000. Rate is variable and subject to change."        │
  ├─────────────────────────────────────────────────────────────┤
  │ Chunk 2 (score: 0.87)                                      │
  │ Source: products/savings-accounts.pdf, page 5               │
  │ "Premium Savings rate tiers:                                │
  │  $5,000 - $24,999: 4.25% APY                              │
  │  $25,000 - $99,999: 4.35% APY                             │
  │  $100,000+: 4.50% APY"                                    │
  ├─────────────────────────────────────────────────────────────┤
  │ Chunk 3 (score: 0.79)                                      │
  │ Source: products/savings-accounts.pdf, page 2               │
  │ "Standard Savings: 2.10% APY. Premium Savings: See page 4 │
  │  for current rates and requirements."                       │
  └─────────────────────────────────────────────────────────────┘
```

### STEP 5 — Prompt Assembly

```
Building the complete prompt for the LLM.
─────────────────────────────────────────

┌──────────────────────────────────────────────────────────────┐
│ SYSTEM: You are a banking assistant for XYZ National Bank.  │
│ Answer ONLY from the provided context. If the context does  │
│ not contain the answer, say "I don't have that information."│
│ Always include the source document. Never give personal     │
│ financial advice. Include standard disclaimer.              │
│                                                              │
│ CONTEXT:                                                     │
│ [Chunk 1]: Premium Savings Account offers a competitive...  │
│ [Chunk 2]: Premium Savings rate tiers: $5,000-$24,999...   │
│ [Chunk 3]: Standard Savings: 2.10% APY...                  │
│                                                              │
│ CONVERSATION HISTORY:                                        │
│ User: What types of accounts do you offer?                  │
│ Assistant: We offer Checking, Savings, CD, and Money Market │
│ User: Tell me more about savings options                    │
│ Assistant: We have Standard Savings and Premium Savings...  │
│                                                              │
│ USER: What is the interest rate on Premium Savings?         │
└──────────────────────────────────────────────────────────────┘

Token count: ~800 input tokens
```

### STEP 6 — LLM Inference

```
Model generates the response.
──────────────────────────────

Model: GPT-4o
Temperature: 0.1 (low = more deterministic for factual banking answers)
Max tokens: 512

LLM Processing:
  1. Reads system prompt → Understands role and rules
  2. Reads context → Finds rate information in Chunks 1 & 2
  3. Reads history → Understands this is a follow-up about savings
  4. Reads current query → "interest rate on Premium Savings"
  5. Generates response token by token

Raw Output:
  "The Premium Savings account offers tiered interest rates based
   on your balance:

   - $5,000 – $24,999: **4.25% APY**
   - $25,000 – $99,999: **4.35% APY**
   - $100,000+: **4.50% APY**

   The minimum opening deposit is $5,000. Please note that rates
   are variable and subject to change.

   *Source: Savings Account Product Sheet, pages 4-5*

   *This is general product information and not personalized
   financial advice.*"

Output tokens: ~120
Latency: 1.2 seconds
```

### STEP 7 — Output Processing

```
Safety and quality checks on the response.
──────────────────────────────────────────

Step 7a: PII Scrubber
  Scan output for PII patterns
  Result: ✓ No PII found in output

Step 7b: Toxicity Filter
  Score: 0.01 (very low)
  Result: ✓ Pass

Step 7c: Groundedness Check
  Claims extracted:
    1. "tiered rates based on balance" → SUPPORTED (Chunk 2)
    2. "$5K-$24K: 4.25% APY"         → SUPPORTED (Chunk 2)
    3. "$25K-$99K: 4.35% APY"        → SUPPORTED (Chunk 2)
    4. "$100K+: 4.50% APY"           → SUPPORTED (Chunk 2)
    5. "Min deposit $5,000"           → SUPPORTED (Chunk 1)
    6. "Rates are variable"           → SUPPORTED (Chunk 1)

  Groundedness score: 6/6 = 1.0 ✓
  Result: ✓ Fully grounded

Step 7d: Format Enforcer
  ✓ Markdown formatting correct
  ✓ Source citation present
  ✓ Disclaimer present
  ✓ Length within limit (under 500 words)
  Result: ✓ Pass
```

### STEP 8 — Observability

```
Logging everything for monitoring and audit.
─────────────────────────────────────────────

Trace Record:
{
  "request_id": "req-7f3a2b",
  "session_id": "abc-123",
  "user_id": "employee_042",
  "timestamp": "2026-03-20T14:32:15Z",
  "turn_number": 3,
  "query": "What is the interest rate on Premium Savings?",
  "rewritten_query": "What is the current APY on Premium Savings account?",
  "retrieval": {
    "chunks_retrieved": 3,
    "top_chunk_score": 0.94,
    "sources": ["savings-accounts.pdf p4", "savings-accounts.pdf p5",
                "savings-accounts.pdf p2"]
  },
  "generation": {
    "model": "gpt-4o-2026-01",
    "temperature": 0.1,
    "input_tokens": 803,
    "output_tokens": 118,
    "latency_ms": 1247
  },
  "evaluation": {
    "groundedness_score": 1.0,
    "toxicity_score": 0.01,
    "pii_detected_input": false,
    "pii_detected_output": false,
    "injection_detected": false
  },
  "cost": {
    "input_cost": "$0.008",
    "output_cost": "$0.004",
    "total_cost": "$0.012"
  }
}
```

### STEP 9 — Response Delivered

```
User sees the final response.
──────────────────────────────

The Premium Savings account offers tiered interest rates based
on your balance:

- $5,000 – $24,999: **4.25% APY**
- $25,000 – $99,999: **4.35% APY**
- $100,000+: **4.50% APY**

The minimum opening deposit is $5,000. Please note that rates
are variable and subject to change.

Source: Savings Account Product Sheet, pages 4-5

This is general product information and not personalized financial advice.
```

---

## 4. The Data Pipeline (Offline / Background)

This runs separately — it's how documents get INTO the knowledge base.

```
RAG DATA PIPELINE — How Documents Become Searchable
════════════════════════════════════════════════════

PHASE 1: INGESTION
────────────────────
Source documents arrive:
  ├── Policy team uploads new_fee_schedule_v4.2.pdf
  ├── Product team updates savings_accounts.pdf
  ├── Compliance pushes reg_e_updates.docx
  └── Automated sync pulls rates from core banking API

PHASE 2: PREPROCESSING
───────────────────────
For each document:
  1. Extract text (OCR for scanned PDFs)
  2. Clean formatting artifacts
  3. Detect and preserve tables
  4. Extract metadata (title, date, version, author, department)

PHASE 3: CHUNKING
─────────────────
Split documents into retrieval-sized pieces:

  Strategy options:
  ┌──────────────────┬────────────────────────────────────┐
  │ Fixed-size       │ Every 512 tokens, 50-token overlap │
  │ Semantic         │ Split on paragraph/section breaks  │
  │ Sliding window   │ Overlapping windows of text        │
  │ Hierarchical     │ Section → Subsection → Paragraph   │
  └──────────────────┴────────────────────────────────────┘

  Example output:
  savings_accounts.pdf → 23 chunks
    chunk_001: "Account Overview... Standard Savings..."
    chunk_002: "Premium Savings Account offers..."
    chunk_003: "Rate tiers: $5,000-$24,999..."
    ...

PHASE 4: EMBEDDING
──────────────────
Convert each chunk to a numerical vector:

  chunk_002 text →  Embedding Model  → [0.23, 0.87, 0.12, ..., 0.45]
                    (e.g., text-embedding-3-large)

  Output: 23 vectors, each 1536 dimensions

PHASE 5: INDEXING
─────────────────
Store vectors + metadata in vector database:

  Vector DB Entry:
  {
    "id": "savings_002",
    "vector": [0.23, 0.87, ...],
    "text": "Premium Savings Account offers...",
    "metadata": {
      "source": "savings-accounts.pdf",
      "page": 4,
      "version": "v4.1",
      "date": "2026-03-15",
      "department": "products"
    }
  }

PHASE 6: VALIDATION
───────────────────
After indexing, run automated checks:
  ✓ All expected documents indexed
  ✓ No duplicate chunks
  ✓ Sample queries return correct chunks
  ✓ Old version chunks replaced (not duplicated)
  ✓ Chunk counts within expected range
```

---

## 5. Agent Loop — Detailed Flow

### 5.1 Banking Agent Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                    BANKING AI AGENT                            │
│                                                                │
│  Available Tools:                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌───────────────┐  │
│  │ check_balance   │  │ get_transactions│  │ transfer_funds│  │
│  │ (read-only)     │  │ (read-only)     │  │ (WRITE action)│  │
│  └─────────────────┘  └─────────────────┘  └───────────────┘  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌───────────────┐  │
│  │ calculate_loan  │  │ look_up_rate    │  │ escalate_to   │  │
│  │ (compute)       │  │ (read-only)     │  │ _human        │  │
│  └─────────────────┘  └─────────────────┘  └───────────────┘  │
│                                                                │
│  Safety Rules:                                                 │
│  - Authenticate before ANY account access                     │
│  - Confirm before ANY write action                            │
│  - Never expose full account numbers                          │
│  - Max 3 retries on tool failure                              │
│  - ALWAYS escalate: fraud, threats, complaints                │
└────────────────────────────────────────────────────────────────┘
```

### 5.2 Agent E2E Flow — "Transfer $500 from checking to savings"

```
     User: "Transfer $500 from checking to savings"
              │
              ▼
┌─────────────────────────┐
│ INPUT PROCESSING        │
│ ✓ PII check: clean     │
│ ✓ Injection check: clean│
│ ✓ Intent: transfer_funds│
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ LLM TURN 1 (Planning)  │──────────────────────────────┐
│                         │                              │
│ "I need to:             │    Decision: CALL TOOL       │
│  1. Verify auth         │    Tool: authenticate_user   │
│  2. Check balance       │    Args: {method: "session"} │
│  3. Execute transfer"   │                              │
└─────────────────────────┘──────────────────────────────┘
             │
             ▼
┌─────────────────────────┐
│ TOOL EXECUTION          │
│ authenticate_user()     │
│ Result: { status: "ok", │
│   user: "john_doe",     │
│   accounts: [           │
│     {type:"checking",   │
│      id:"CHK-3456"},    │
│     {type:"savings",    │
│      id:"SAV-7890"}     │
│   ]                     │
│ }                       │
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ LLM TURN 2             │──────────────────────────────┐
│                         │                              │
│ "User authenticated.    │    Decision: CALL TOOL       │
│  Now check balance."    │    Tool: check_balance       │
│                         │    Args: {account:"CHK-3456"}│
└─────────────────────────┘──────────────────────────────┘
             │
             ▼
┌─────────────────────────┐
│ TOOL EXECUTION          │
│ check_balance()         │
│ Result: {               │
│   account: "CHK-3456",  │
│   balance: 2847.50,     │
│   available: 2847.50    │
│ }                       │
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ LLM TURN 3             │──────────────────────────────┐
│                         │                              │
│ "Balance $2,847.50 >    │    Decision: ASK USER        │
│  $500. Sufficient.      │    (Confirm before write     │
│  Need confirmation."    │     action — safety rule)    │
└─────────────────────────┘──────────────────────────────┘
             │
             ▼
         ┌───────────────────────────────────────────────┐
         │ "Please confirm: Transfer $500.00 from        │
         │  Checking (****3456) to Savings (****7890)?"  │
         └───────────────────────────────────┬───────────┘
                                             │
             User: "Yes, confirm"            │
                                             ▼
┌─────────────────────────┐
│ LLM TURN 4             │──────────────────────────────┐
│                         │                              │
│ "User confirmed.        │    Decision: CALL TOOL       │
│  Execute transfer."     │    Tool: transfer_funds      │
│                         │    Args: {from:"CHK-3456",   │
│                         │     to:"SAV-7890",           │
│                         │     amount: 500.00}          │
└─────────────────────────┘──────────────────────────────┘
             │
             ▼
┌─────────────────────────┐
│ TOOL EXECUTION          │
│ transfer_funds()        │
│ Result: {               │
│   status: "success",    │
│   confirmation: "TXN-   │
│     98765",             │
│   new_checking: 2347.50,│
│   new_savings: 8500.00  │
│ }                       │
└────────────┬────────────┘
             ▼
┌─────────────────────────┐
│ LLM TURN 5 (Final)     │──────────────────────────────┐
│                         │                              │
│ "Transfer complete.     │    Decision: RESPOND TO USER │
│  Compose final message."│    (No more tools needed)    │
└─────────────────────────┘──────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────────────────────┐
│ OUTPUT PROCESSING                                       │
│ ✓ PII check: account numbers masked                    │
│ ✓ Toxicity: clean                                      │
│ ✓ Format: includes confirmation number                 │
└─────────────────────────────────────────────────────────┘
             │
             ▼
         ┌───────────────────────────────────────────────┐
         │ "Transfer complete!                           │
         │                                               │
         │  $500.00 transferred from Checking (****3456) │
         │  to Savings (****7890).                       │
         │                                               │
         │  Confirmation #: TXN-98765                    │
         │  New Checking Balance: $2,347.50              │
         │  New Savings Balance: $8,500.00"              │
         └───────────────────────────────────────────────┘
```

---

## 6. What Breaks Where — Test Points

### 6.1 Failure Map per Layer

| Layer | What Can Go Wrong | Impact | Test Level |
|---|---|---|---|
| **Client** | Input not sanitized client-side | XSS, injection reaches backend | Basic |
| **API Gateway** | Auth bypass, rate limit failure | Unauthorized access, abuse | Basic |
| **PII Detection** | Misses new PII patterns | Data leakage to LLM/logs | Basic |
| **Injection Detection** | Novel injection bypasses filter | System prompt leaked, rules overridden | Basic + Advanced |
| **Embedding Model** | Poor embedding for domain terms | Wrong docs retrieved | Intermediate |
| **Vector Search** | Index stale, wrong similarity metric | Outdated or wrong context | Basic + Intermediate |
| **Reranker** | Ranks wrong doc higher | LLM reads wrong context | Intermediate |
| **Knowledge Base** | Outdated docs, duplicates, bad parsing | Wrong facts served to LLM | Basic |
| **Prompt Assembly** | History too long, context order wrong | Token overflow, lost context | Intermediate |
| **System Prompt** | Weak guardrails, exploitable rules | Safety bypass, persona break | Basic + Advanced |
| **LLM** | Hallucination, ignores context, wrong format | Wrong or dangerous answers | Intermediate |
| **Agent Loop** | Wrong tool, wrong params, infinite loop | Wrong transactions, system abuse | Advanced |
| **Tool Execution** | API timeout, wrong data, permission error | Failed operations, wrong results | Advanced |
| **PII Scrubber (output)** | Misses PII in generated text | Customer data exposed | Basic |
| **Toxicity Filter** | False negative (misses toxic content) | Harmful response delivered | Basic + Advanced |
| **Groundedness Check** | Slow or inaccurate scoring | Hallucinated responses slip through | Intermediate |
| **Audit Log** | Missing fields, PII in logs | Compliance violation | Advanced |
| **Data Pipeline** | Bad chunking, stale index, embedding drift | Foundation-level quality degradation | Basic |

### 6.2 Detailed Component-Level Failure Scenarios

```
LAYER: INPUT GUARDRAILS
═══════════════════════
┌──────────────┬──────────────────────────┬────────────────────────────┐
│ Component    │ Failure Scenario         │ Impact                     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ PII Scanner  │ New PII format not       │ SSN/account number sent    │
│              │ recognized (e.g.,        │ to LLM and logged in       │
│              │ international formats)   │ plain text                 │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Injection    │ Encoded attack (base64,  │ System prompt revealed,    │
│ Detector     │ unicode) bypasses rules  │ guardrails disabled        │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Rate Limiter │ Per-session vs per-user  │ Attacker opens multiple    │
│              │ mismatch                 │ sessions to bypass limit   │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Content      │ Off-topic query not      │ LLM answers questions      │
│ Classifier   │ detected                 │ outside banking domain     │
└──────────────┴──────────────────────────┴────────────────────────────┘

LAYER: RETRIEVAL
═══════════════
┌──────────────┬──────────────────────────┬────────────────────────────┐
│ Component    │ Failure Scenario         │ Impact                     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Embedding    │ Domain term not well     │ "APY" and "interest rate"  │
│ Model        │ represented              │ don't match in vector space│
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Vector DB    │ Index not refreshed      │ Old rates served as current│
│              │ after document update    │                            │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Hybrid       │ Keyword search returns   │ Wrong product info mixed   │
│ Search       │ partial name matches     │ into context               │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Reranker     │ Similar product names    │ "Premium Checking" chunk   │
│              │ confuse ranking          │ ranked above "Premium      │
│              │                          │ Savings" chunk             │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Chunking     │ Table split across       │ Incomplete rate table      │
│              │ chunk boundary           │ delivered to LLM           │
└──────────────┴──────────────────────────┴────────────────────────────┘

LAYER: LLM GENERATION
════════════════════
┌──────────────┬──────────────────────────┬────────────────────────────┐
│ Component    │ Failure Scenario         │ Impact                     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ System       │ Instructions too vague   │ Model invents information  │
│ Prompt       │ about refusal behavior   │ instead of saying "I don't │
│              │                          │ know"                      │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Context      │ Token limit exceeded     │ Early context (system      │
│ Window       │ with long history        │ rules) truncated           │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Temperature  │ Set too high for         │ Different rates quoted     │
│              │ factual domain           │ on each call               │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Model        │ Model updated/swapped    │ Safety behaviors change    │
│ Version      │ without re-testing       │ silently                   │
└──────────────┴──────────────────────────┴────────────────────────────┘

LAYER: AGENT / TOOL USE
═══════════════════════
┌──────────────┬──────────────────────────┬────────────────────────────┐
│ Component    │ Failure Scenario         │ Impact                     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Tool         │ Model calls transfer     │ Money moved without        │
│ Selection    │ without checking balance │ sufficient funds check     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Parameter    │ Amount parsed as string  │ "$500" → type error or     │
│ Passing      │ instead of number        │ wrong amount               │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Confirmation │ Write action executed    │ Irreversible transaction   │
│ Gate         │ without user confirming  │ without consent            │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Error        │ API returns 500, model   │ User told balance is $0    │
│ Handling     │ hallucinates result      │ when API was just down     │
├──────────────┼──────────────────────────┼────────────────────────────┤
│ Loop         │ Model keeps calling      │ Infinite API calls,        │
│ Detection    │ same tool in a loop      │ cost explosion             │
└──────────────┴──────────────────────────┴────────────────────────────┘
```

---

## 7. Architecture Variants by Scale

```
SMALL SCALE (Prototype / Internal Tool)
════════════════════════════════════════
User → OpenAI API (system prompt + user query) → Response

Components: 1 API call
Vector DB: None (or simple FAISS in-memory)
Guardrails: Minimal (system prompt instructions only)
Testing: Manual + basic prompt tests


MEDIUM SCALE (Department-Level Bot)
═══════════════════════════════════
User → Backend → Guardrails → RAG (pgvector) → LLM → Guardrails → Response

Components: 5-8 services
Vector DB: pgvector or Qdrant
Guardrails: Rule-based + ML classifier
Testing: Automated test suite (Promptfoo/DeepEval)


LARGE SCALE (Enterprise Production — Banking)
═════════════════════════════════════════════
User → Gateway → Input Processing (3 stages) → Hybrid Retrieval
     → Reranker → Prompt Assembly → LLM → Agent Loop → Output
       Processing (4 stages) → Delivery → Observability

Components: 15-25 services
Vector DB: Pinecone/Weaviate (managed)
Guardrails: Multi-layer (input + output + runtime)
Agent: Tool-calling with auth and confirmation
Testing: Full CI/CD pipeline + red-team + human eval + compliance audit
```

### Scale Comparison Table

| Aspect | Small | Medium | Large (Banking) |
|---|---|---|---|
| **Users** | < 50 | 50 - 1,000 | 1,000+ |
| **Documents** | < 100 | 100 - 10,000 | 10,000+ |
| **Guardrails** | System prompt only | Rule-based | Multi-layer ML + rules |
| **Retrieval** | Basic vector | Hybrid search | Hybrid + reranker + filters |
| **Agent/Tools** | None | Optional | Full tool suite with auth |
| **Evaluation** | Manual | Semi-automated | Full CI/CD + human eval |
| **Compliance** | None | Basic logging | Full audit trail + regulatory |
| **Cost/query** | ~$0.01 | ~$0.02-0.05 | ~$0.03-0.10 |
| **Latency target** | < 5s | < 3s | < 2s (P95 < 5s) |

---

## Summary — Key Takeaway

```
The critical insight for AI application architecture:

  AI testing is NOT just testing the model.
  It is testing EVERY LAYER that feeds into and processes the model's work.

  ┌──────────────────────────────────────────────┐
  │                                              │
  │   Bad data      → Bad retrieval              │
  │   Bad retrieval → Bad context                │
  │   Bad context   → Bad generation             │
  │   Bad guardrails → Dangerous output          │
  │   Bad agent logic → Wrong real-world actions  │
  │   Bad logging   → Compliance violations      │
  │                                              │
  │   Quality is a CHAIN.                        │
  │   The weakest link is usually BELOW the LLM. │
  │                                              │
  └──────────────────────────────────────────────┘
```

---

> **Document Version**: 1.0  
> **Last Updated**: March 20, 2026  
> **Domain**: Banking & Financial Services  
> **Classification**: Internal — AI Quality Engineering
