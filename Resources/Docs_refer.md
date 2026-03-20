`# AI Testing & Evaluation — Complete Learning Resources

> **Created**: March 20, 2026  
> **Purpose**: Curated learning path from Basic → Intermediate → Advanced  
> **Covers**: AI Evals, Testing, RAG, Agents, Architecture, API Products  
> **Format**: Resources split by level with learning objectives for each

---

## Table of Contents

- [How to Use This Guide](#how-to-use-this-guide)
- [Part 1: BASIC — Foundations](#part-1-basic--foundations)
- [Part 2: INTERMEDIATE — Depth & Practice](#part-2-intermediate--depth--practice)
- [Part 3: ADVANCED — Enterprise & Research](#part-3-advanced--enterprise--research)
- [Part 4: E2E Flow & Architecture Design Resources](#part-4-e2e-flow--architecture-design-resources)
- [Part 5: AI Evaluation Frameworks & Tools](#part-5-ai-evaluation-frameworks--tools)
- [Part 6: API & AI Product Testing](#part-6-api--ai-product-testing)
- [Part 7: Certifications & Structured Courses](#part-7-certifications--structured-courses)
- [Part 8: Hands-On Practice Labs](#part-8-hands-on-practice-labs)
- [Part 9: Communities & Staying Current](#part-9-communities--staying-current)
- [Part 10: Recommended Learning Path (Week-by-Week)](#part-10-recommended-learning-path-week-by-week)

---

## How to Use This Guide

```
YOUR CURRENT LEVEL → START HERE
════════════════════════════════

"I'm new to AI testing"
  → Start at Part 1: BASIC
  → Estimated time: 4-6 weeks

"I understand LLMs but haven't tested them"
  → Start at Part 1 Section 1.3, then move to Part 2
  → Estimated time: 3-4 weeks

"I've done some prompt testing, want to go deeper"
  → Start at Part 2: INTERMEDIATE
  → Estimated time: 4-6 weeks

"I want enterprise-grade AI evaluation skills"
  → Start at Part 3: ADVANCED
  → Estimated time: 6-8 weeks

"I want hands-on tools and practice"
  → Jump to Part 5 (Tools) + Part 8 (Labs)
  → Ongoing practice
```

---

# Part 1: BASIC — Foundations

> **Goal**: Understand how LLMs work, what RAG is, and why AI testing is different from traditional testing.

## 1.1 Understanding LLMs (Start Here)

### Core Concepts to Learn
- What is a Large Language Model and how does it generate text?
- What are tokens, temperature, top-p, and max tokens?
- What is a system prompt vs user prompt?
- Why are LLM outputs non-deterministic?
- What is fine-tuning vs prompting vs RAG?

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Andrej Karpathy — "Intro to Large Language Models"** | Video | YouTube: "Intro to Large Language Models" by Andrej Karpathy (Nov 2023) | 1 hour |
| **Andrej Karpathy — "Let's build GPT from scratch"** | Video | YouTube: "Let's build GPT: from scratch, in code, spelled out" | 2 hours |
| **OpenAI — "What are tokens?"** | Docs | OpenAI Platform Documentation → Tokenizer | 15 min |
| **Anthropic — "Core Views on AI Safety"** | Article | Anthropic website → Research | 30 min |
| **Microsoft — "Introduction to Generative AI"** | Course | Microsoft Learn → "Introduction to Generative AI" (free) | 2 hours |
| **Google — "Introduction to Large Language Models"** | Course | Google Cloud Skills Boost → "Introduction to LLMs" (free) | 1 hour |
| **DeepLearning.AI — "ChatGPT Prompt Engineering for Developers"** | Course | DeepLearning.AI (free, with Andrew Ng + Isa Fulford) | 1.5 hours |
| **3Blue1Brown — "But what is a GPT?"** | Video | YouTube: 3Blue1Brown neural network series | 30 min |

### Books

| Book | Author | Why Read It |
|---|---|---|
| **"AI Engineering" (2025)** | Chip Huyen | Best practical guide to building AI applications — covers LLMs, RAG, agents, evaluation from an engineering perspective |
| **"Designing Machine Learning Systems"** | Chip Huyen | Foundational ML systems thinking (predecessor to AI Engineering) |
| **"Prompt Engineering Guide"** | DAIR.AI | Free online book — comprehensive prompt engineering reference |

---

## 1.2 Understanding RAG (Retrieval-Augmented Generation)

### Core Concepts to Learn
- What is RAG and why does it exist?
- What are embeddings and vector databases?
- What is chunking and why does it matter?
- How does similarity search work?
- What is the difference between vector search, keyword search, and hybrid search?

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **IBM — "What is RAG?"** | Article | IBM Research Blog → "What is retrieval-augmented generation?" | 15 min |
| **LangChain — RAG Tutorial** | Tutorial | LangChain Documentation → Tutorials → "Build a RAG Application" | 2 hours |
| **Pinecone — "What are Vector Embeddings?"** | Article | Pinecone Learning Center → "Vector Embeddings" | 20 min |
| **Weaviate — "Introduction to Vector Databases"** | Article | Weaviate Blog → "What is a Vector Database?" | 20 min |
| **Vicki Boykis — "What are Embeddings?"** | Deep Dive | vickiboykis.com → "What are embeddings" (long-form technical) | 1 hour |
| **DeepLearning.AI — "Building and Evaluating Advanced RAG"** | Course | DeepLearning.AI short course (free) with LlamaIndex | 1 hour |
| **DeepLearning.AI — "LangChain: Chat with Your Data"** | Course | DeepLearning.AI short course (free) | 1 hour |

---

## 1.3 AI Testing Basics — Why It's Different

### Core Concepts to Learn
- Why can't we test AI like traditional software?
- What are the main AI failure modes? (hallucination, bias, injection, etc.)
- What does "non-deterministic testing" mean in practice?
- What is a golden dataset?
- What are basic evaluation metrics (exact match, contains, regex)?

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Microsoft — "Evaluation of generative AI applications"** | Docs | Microsoft Learn → "Evaluation of generative AI applications" | 1 hour |
| **Google — "Evaluation Methods for LLM Applications"** | Article | Google Cloud Blog → "LLM evaluation methods" | 30 min |
| **Anthropic — "Evaluating AI Systems"** | Guide | Anthropic Docs → "Evaluating prompts" | 30 min |
| **Hamel Husain — "Your AI Product Needs Evals"** | Article | hamel.dev → "Your AI Product Needs Evals" | 30 min |
| **Eugene Yan — "Patterns for Building LLM-based Systems"** | Article | eugeneyan.com → "Patterns for Building LLM-based Systems & Products" | 45 min |
| **Jason Wei — "What Makes Good Evals"** | Talk | YouTube: various conference talks by Jason Wei (Google DeepMind) | 30 min |
| **Promptfoo — Getting Started Guide** | Tutorial | promptfoo.dev → Docs → Getting Started | 1 hour |

### Practice Exercise (Basic Level)
```
EXERCISE 1: Build Your First Golden Dataset
────────────────────────────────────────────
1. Pick a domain (banking, healthcare, HR)
2. Write 20 question-answer pairs with source documents
3. Test them against ChatGPT or Claude
4. Record: correct/incorrect/partially correct
5. Calculate accuracy rate

Expected outcome: You'll see ~70-80% accuracy without RAG,
and understand why retrieval matters.
```

---

## 1.4 API Testing Fundamentals (for AI APIs)

### Core Concepts to Learn
- How do LLM APIs work? (OpenAI, Anthropic, Azure OpenAI)
- What is an API request/response for chat completions?
- What are streaming responses?
- How to test API contracts for AI endpoints
- Rate limiting, authentication, error codes for AI APIs

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **OpenAI API Documentation** | Docs | platform.openai.com → API Reference | 2 hours |
| **Anthropic API Documentation** | Docs | docs.anthropic.com → API Reference | 1 hour |
| **Azure OpenAI Service Documentation** | Docs | Microsoft Learn → "Azure OpenAI Service REST API reference" | 1 hour |
| **Postman — "API Testing for Beginners"** | Course | Postman Learning Center → "API Testing" | 2 hours |
| **"API Testing in the Age of AI"** | Article | Ministry of Testing → search for AI API testing articles | 30 min |

### Key API Endpoints to Understand

```
OPENAI-STYLE API — Essential Endpoints
═══════════════════════════════════════

POST /v1/chat/completions
  ├── model: "gpt-4o"
  ├── messages: [{role: "system", content: "..."}, 
  │              {role: "user", content: "..."}]
  ├── temperature: 0.1
  ├── max_tokens: 512
  └── Response: {choices: [{message: {content: "..."}}], 
                 usage: {prompt_tokens: 800, completion_tokens: 120}}

POST /v1/embeddings
  ├── model: "text-embedding-3-large"
  ├── input: "What is the savings rate?"
  └── Response: {data: [{embedding: [0.23, 0.87, ...]}]}

POST /v1/chat/completions (with tools/function calling)
  ├── model: "gpt-4o"
  ├── messages: [...]
  ├── tools: [{type: "function", function: {name: "check_balance", ...}}]
  └── Response: {choices: [{message: {tool_calls: [{function: {...}}]}}]}
```

---

# Part 2: INTERMEDIATE — Depth & Practice

> **Goal**: Build hands-on evaluation skills. Use frameworks. Test retrieval, groundedness, safety.

## 2.1 LLM Evaluation Deep Dive

### Core Concepts to Learn
- What is LLM-as-Judge evaluation?
- What are reference-based vs reference-free metrics?
- How do ROUGE, BERTScore, and semantic similarity work?
- How to design evaluation rubrics
- How to measure inter-annotator agreement

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **"Judging LLM-as-a-Judge" (paper)** | Paper | arxiv.org → Search: "Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena" (Zheng et al., 2023) | 1 hour |
| **"G-Eval: NLG Evaluation using GPT-4"** | Paper | arxiv.org → Search: "G-Eval" (Liu et al., 2023) | 45 min |
| **DeepLearning.AI — "Building and Evaluating Advanced RAG"** | Course | DeepLearning.AI (free, with Jerry Liu from LlamaIndex) | 1.5 hours |
| **DeepLearning.AI — "Automated Testing for LLMOps"** | Course | DeepLearning.AI short course (free, with CircleCI) | 1 hour |
| **Ragas Documentation** | Docs | docs.ragas.io → Concepts → Metrics | 2 hours |
| **DeepEval Documentation** | Docs | docs.confident-ai.com → Metrics | 2 hours |
| **MLflow — LLM Evaluation Guide** | Docs | mlflow.org → LLM Evaluation | 1 hour |
| **Microsoft — "Evaluation and monitoring metrics for generative AI"** | Docs | Microsoft Learn → "Evaluation and monitoring metrics" | 1 hour |
| **Braintrust — "The Hitchhiker's Guide to LLM Evaluation"** | Guide | braintrustdata.com → Blog | 45 min |

### Evaluation Metrics to Master

```
REFERENCE-BASED (need ground truth answer)
══════════════════════════════════════════
  Exact Match       → Structured output validation
  ROUGE-L           → Summarization quality
  BERTScore         → Semantic similarity to reference
  F1 Token Overlap  → Partial match scoring

REFERENCE-FREE (no ground truth needed)
══════════════════════════════════════════
  LLM-as-Judge      → Score quality on rubric (1-5)
  Groundedness      → Claims supported by context?
  Answer Relevance  → Does answer address the question?
  Toxicity Score    → Harmful content probability
  Coherence         → Logical flow of response

RAG-SPECIFIC
══════════════
  Context Precision  → Are retrieved docs relevant?
  Context Recall     → Are all relevant docs found?
  Faithfulness       → Is answer faithful to context?
  Answer Correctness → Is the final answer right?
```

---

## 2.2 RAG Evaluation

### Core Concepts to Learn
- The RAG Triad: Context Relevance, Groundedness, Answer Relevance
- How to test retrieval independently from generation
- Chunk quality assessment
- How to handle version conflicts and empty retrieval
- The Ragas framework and its metrics

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Ragas — "Metrics" Documentation** | Docs | docs.ragas.io → Core Concepts → Metrics | 1 hour |
| **TruLens — "RAG Triad" Documentation** | Docs | trulens.org → Documentation → "RAG Triad" | 45 min |
| **"Benchmarking RAG" (paper)** | Paper | arxiv.org → Search: "Benchmarking Large Language Models in Retrieval-Augmented Generation" (Chen et al., 2024) | 1 hour |
| **LlamaIndex — Evaluation Module** | Docs | docs.llamaindex.ai → Module Guides → Evaluation | 1 hour |
| **Pinecone — "RAG Evaluation"** | Guide | Pinecone Blog → "How to evaluate RAG" | 30 min |
| **"ARES: An Automated Evaluation Framework for RAG"** | Paper | arxiv.org → Search: "ARES" (Saad-Falcon et al., 2024) | 45 min |
| **DeepLearning.AI — "Building Agentic RAG with LlamaIndex"** | Course | DeepLearning.AI short course (free) | 1 hour |

### Practice Exercise (Intermediate Level)
```
EXERCISE 2: Evaluate a RAG System
──────────────────────────────────
1. Build a simple RAG system using LangChain + ChromaDB
   (use 10-20 PDF documents from any domain)
2. Create 30 test questions with known answers
3. Install Ragas: pip install ragas
4. Run evaluation:
   - Faithfulness (groundedness)
   - Answer Relevance
   - Context Precision
   - Context Recall
5. Identify the top 5 failure cases
6. Write a test report with recommendations

Expected outcome: Understand where your RAG fails
and whether it's a retrieval problem or generation problem.
```

---

## 2.3 Safety, Bias & Prompt Injection Testing

### Core Concepts to Learn
- How to systematically red-team an LLM
- What are jailbreaks and how to test for them?
- How to design counterfactual bias tests
- What is the OWASP Top 10 for LLMs?
- How to test content safety at scale

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **OWASP — "Top 10 for LLM Applications"** | Standard | owasp.org → OWASP Top 10 for LLM Applications (2025 edition) | 2 hours |
| **Microsoft — "Red teaming LLM Applications"** | Guide | Microsoft Learn → "Planning red teaming for large language models" | 1 hour |
| **Anthropic — "Red Teaming Language Models"** | Paper | arxiv.org → Search: "Red Teaming Language Models to Reduce Harms" (Ganguli et al., 2022) | 1 hour |
| **"Universal and Transferable Adversarial Attacks on Aligned LLMs"** | Paper | arxiv.org → Search: "Universal adversarial attacks" (Zou et al., 2023) | 45 min |
| **Giskard Documentation** | Docs | docs.giskard.ai → LLM Scan → Vulnerability Detection | 1 hour |
| **NIST — "AI Risk Management Framework"** | Standard | nist.gov → AI RMF (AI 100-1) | 2 hours |
| **Google — "Responsible AI Practices"** | Guide | ai.google → Responsibility → Practices | 1 hour |
| **Microsoft — "Responsible AI Standard"** | Standard | Microsoft → Responsible AI → Our approach | 1 hour |
| **Fairlearn Documentation** | Docs | fairlearn.org → User Guide | 1 hour |
| **"DecodingTrust: A Comprehensive Assessment of Trustworthiness in GPT Models"** | Paper | arxiv.org → Search: "DecodingTrust" (Wang et al., 2023) | 1 hour |

### Practice Exercise (Intermediate Level)
```
EXERCISE 3: Red-Team a Chatbot
───────────────────────────────
1. Pick any public chatbot (or build a simple one with system prompt)
2. Run these 10 attacks against it:
   a. Direct instruction override
   b. System prompt extraction
   c. Role-play jailbreak
   d. Base64 encoded instruction
   e. Multi-turn trust escalation
   f. Delimiter/formatting confusion
   g. Foreign language instruction
   h. Context window overflow
   i. Sycophancy test (assert false statement)
   j. PII extraction attempt
3. Document results in a red-team report:
   - Attack type, input, output, severity, pass/fail

Expected outcome: Real experience finding AI vulnerabilities.
```

---

## 2.4 Prompt Engineering for Testers

### Core Concepts to Learn
- How to write robust system prompts
- How to write evaluation prompts (LLM-as-Judge)
- Chain-of-thought prompting for complex evaluation
- Few-shot prompting for consistent scoring
- Prompt versioning and A/B testing

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Anthropic — "Prompt Engineering Guide"** | Docs | docs.anthropic.com → Prompt Engineering | 2 hours |
| **OpenAI — "Prompt Engineering Guide"** | Docs | platform.openai.com → Docs → Prompt Engineering | 1.5 hours |
| **DAIR.AI — "Prompt Engineering Guide"** | Guide | promptingguide.ai (comprehensive free resource) | 3 hours |
| **DeepLearning.AI — "ChatGPT Prompt Engineering for Developers"** | Course | DeepLearning.AI (free, with Andrew Ng) | 1.5 hours |
| **Lilian Weng — "Prompt Engineering"** | Article | lilianweng.github.io → "Prompt Engineering" | 1 hour |

---

# Part 3: ADVANCED — Enterprise & Research

> **Goal**: Master enterprise evaluation pipelines, agent testing, compliance, and cutting-edge research.

## 3.1 AI Agent Testing

### Core Concepts to Learn
- What is an AI agent and how do tool-calling architectures work?
- How to test agent trajectories (sequence of tool calls)
- How to test planning and reasoning capabilities
- How to inject failures and test recovery
- How to test multi-agent systems

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Anthropic — "Building Effective Agents"** | Guide | anthropic.com → Research → "Building effective agents" (2024) | 1 hour |
| **OpenAI — "Function Calling Guide"** | Docs | platform.openai.com → Docs → Function Calling | 1 hour |
| **LangChain — "Agent" Documentation** | Docs | python.langchain.com → Modules → Agents | 2 hours |
| **"AgentBench: Evaluating LLMs as Agents"** | Paper | arxiv.org → Search: "AgentBench" (Liu et al., 2023) | 1 hour |
| **"ToolBench: Evaluating Tool Utilization"** | Paper | arxiv.org → Search: "ToolBench" (Qin et al., 2023) | 45 min |
| **"Reflexion: Language Agents with Verbal Reinforcement Learning"** | Paper | arxiv.org → Search: "Reflexion" (Shinn et al., 2023) | 45 min |
| **DeepLearning.AI — "Functions, Tools and Agents with LangChain"** | Course | DeepLearning.AI short course (free) | 1.5 hours |
| **DeepLearning.AI — "AI Agents in LangGraph"** | Course | DeepLearning.AI short course (free) | 1.5 hours |
| **LangSmith Documentation** | Docs | docs.smith.langchain.com → Evaluation | 2 hours |
| **CrewAI Documentation** | Docs | docs.crewai.com → Testing section | 1 hour |

### Practice Exercise (Advanced Level)
```
EXERCISE 4: Build and Test an AI Agent
───────────────────────────────────────
1. Build a simple agent with 3 tools using LangChain:
   - get_weather(location)
   - search_web(query)
   - calculate(expression)
2. Write 10 agent trajectory test cases
3. Test for:
   - Correct tool selection
   - Correct parameter passing
   - Correct tool call order
   - Handling of tool errors (mock API failures)
   - Loop detection (does agent get stuck?)
4. Use LangSmith to trace and evaluate runs

Expected outcome: Understand agent testing methodology hands-on.
```

---

## 3.2 Enterprise Evaluation Pipelines

### Core Concepts to Learn
- How to build CI/CD for AI systems
- How to set up automated evaluation gates
- How to implement human-in-the-loop evaluation
- How to track metrics over time (regression detection)
- How to version prompts and evaluation datasets

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Hamel Husain — "Creating a LLM-as-a-Judge That Drives Business Results"** | Article | hamel.dev → Blog (2024) | 45 min |
| **Shreya Shankar — "Rethinking LLM Evaluation"** | Talk/Article | shreya-shankar.com → Research | 30 min |
| **"Holistic Evaluation of Language Models (HELM)"** | Paper + Tool | crfm.stanford.edu/helm | 2 hours |
| **Promptfoo — CI/CD Integration Guide** | Docs | promptfoo.dev → Docs → CI/CD | 1 hour |
| **Braintrust — "Online Evaluation Guide"** | Docs | braintrustdata.com → Docs → Online Evaluation | 1 hour |
| **Weights & Biases — "LLM Evaluation Guide"** | Guide | wandb.ai → Guides → LLM Evaluation | 1 hour |
| **MLflow — "LLM Evaluation"** | Docs | mlflow.org → Docs → LLM Evaluation | 1 hour |
| **Azure AI Studio — "Evaluation Documentation"** | Docs | Microsoft Learn → "Evaluation and monitoring for generative AI" | 2 hours |

---

## 3.3 Advanced Safety & Compliance

### Core Concepts to Learn
- How to run systematic red-team exercises at scale
- How to comply with AI regulations (EU AI Act, NIST AI RMF)
- How to build audit trails for AI systems
- How to test for indirect prompt injection in production
- How to monitor AI systems post-deployment

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **EU AI Act — Official Text** | Regulation | Official Journal of the EU → AI Act (2024) | 3 hours (skim) |
| **NIST AI 100-1 — AI Risk Management Framework** | Standard | nist.gov → AI RMF | 2 hours |
| **NIST AI 600-1 — "AI RMF Generative AI Profile"** | Standard | nist.gov → AI 600-1 (specific to GenAI) | 2 hours |
| **"Not what you've signed up for: Compromising RAG"** | Paper | arxiv.org → Search: "Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection" (Greshake et al., 2023) | 1 hour |
| **MITRE ATLAS — "Adversarial Threat Landscape for AI Systems"** | Framework | atlas.mitre.org | 2 hours |
| **Microsoft Counterfit** | Tool | GitHub: microsoft/counterfit — adversarial ML testing | 2 hours |
| **"Jailbreaking ChatGPT via Prompt Engineering"** | Paper | arxiv.org → Search: "Jailbreaking ChatGPT" (Liu et al., 2023) | 45 min |

---

## 3.4 MLOps & LLMOps for Testers

### Core Concepts to Learn
- How are AI models deployed and versioned?
- What is model monitoring and drift detection?
- How to set up A/B testing for AI responses
- How to manage prompt versions in production
- Cost optimization and token management

### Resources

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **"Designing Machine Learning Systems"** | Book | Chip Huyen (O'Reilly, 2022) | 10 hours |
| **"AI Engineering"** | Book | Chip Huyen (O'Reilly, 2025) — covers LLMOps comprehensively | 12 hours |
| **Google — "MLOps: Continuous delivery for ML"** | Guide | cloud.google.com → Architecture → MLOps | 1 hour |
| **"LLMOps: Everything You Need to Know"** | Article | oreilly.com → Search: "LLMOps" | 30 min |
| **DeepLearning.AI — "LLMOps"** | Course | DeepLearning.AI short course (free, with Google Cloud) | 1 hour |

---

# Part 4: E2E Flow & Architecture Design Resources

> **Goal**: Understand how AI applications are built end-to-end so you know what to test.

## 4.1 RAG Architecture

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **LangChain — "RAG Architecture" Documentation** | Docs | python.langchain.com → Use Cases → RAG | 2 hours |
| **LlamaIndex — "Building a RAG Pipeline" Tutorial** | Tutorial | docs.llamaindex.ai → Getting Started | 2 hours |
| **"Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks"** | Paper | arxiv.org → Search: "Retrieval-Augmented Generation" (Lewis et al., 2020) — the original RAG paper | 1 hour |
| **"Seven Failure Points When Engineering a RAG"** | Paper | arxiv.org → Search: "Seven Failure Points" (Barnett et al., 2024) | 45 min |
| **Pinecone — "RAG Architecture Guide"** | Guide | pinecone.io → Learn → "RAG Architecture" | 30 min |

## 4.2 Agent Architecture

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **Anthropic — "Building Effective Agents"** | Guide | anthropic.com → Research → "Building effective agents" | 1 hour |
| **"The Landscape of Emerging AI Agent Architectures"** | Paper | arxiv.org → Search: "Landscape of Emerging AI Agent Architectures" (Masterman et al., 2024) | 45 min |
| **"A Survey on Large Language Model based Autonomous Agents"** | Survey | arxiv.org → Search: "Survey LLM autonomous agents" (Wang et al., 2024) | 2 hours |
| **LangGraph Documentation** | Docs | langchain-ai.github.io/langgraph → Concepts | 2 hours |
| **AutoGen Documentation** | Docs | microsoft.github.io/autogen → Getting Started | 2 hours |

## 4.3 Full Application Architecture

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **"Building LLM-Powered Applications" (a]16z)** | Guide | a16z.com → "Emerging Architectures for LLM Applications" | 30 min |
| **Chip Huyen — "Building a Generative AI Platform"** | Article | huyenchip.com → Blog → "Building a Generative AI Platform" | 1 hour |
| **"Patterns for Building LLM-based Systems"** | Article | eugeneyan.com → "Patterns for Building LLM-based Systems" | 45 min |
| **Azure — "Baseline OpenAI end-to-end chat reference architecture"** | Reference | Microsoft Learn → Architecture Center → AI → "Baseline OpenAI E2E chat" | 2 hours |
| **AWS — "Generative AI Application Builder on AWS"** | Reference | AWS Architecture Blog → Generative AI patterns | 1 hour |

---

# Part 5: AI Evaluation Frameworks & Tools

## 5.1 Open Source Tools (Hands-On)

| Tool | Best For | Docs Link | Getting Started Time |
|---|---|---|---|
| **Promptfoo** | Prompt testing, YAML-based, LLM-as-judge, CI/CD | promptfoo.dev | 1 hour |
| **DeepEval** | Python LLM evaluation with 14+ built-in metrics | docs.confident-ai.com | 1 hour |
| **Ragas** | RAG-specific evaluation (faithfulness, relevance) | docs.ragas.io | 1 hour |
| **Giskard** | Bias detection, vulnerability scanning, LLM testing | docs.giskard.ai | 1.5 hours |
| **TruLens** | RAG Triad evaluation, feedback functions | trulens.org | 1 hour |
| **Phoenix (Arize)** | LLM observability, trace analysis, evaluation | docs.arize.com/phoenix | 1 hour |
| **MLflow** | Experiment tracking, LLM evaluation module | mlflow.org | 1 hour |
| **Fairlearn** | Fairness assessment, bias metrics | fairlearn.org | 1 hour |
| **Guardrails AI** | Output validation, structural checks | guardrailsai.com | 1 hour |
| **NeMo Guardrails (NVIDIA)** | Programmable guardrails for LLM apps | docs.nvidia.com/nemo/guardrails | 2 hours |

## 5.2 Commercial Platforms

| Platform | Best For | Docs Link |
|---|---|---|
| **Azure AI Studio** | Enterprise evaluation + safety (built-in evaluators) | Microsoft Learn |
| **LangSmith** | LangChain tracing + evaluation + datasets | docs.smith.langchain.com |
| **Braintrust** | Online/offline eval, logging, prompt playground | braintrustdata.com |
| **Weights & Biases (W&B)** | Experiment tracking, LLM monitoring | wandb.ai |
| **Humanloop** | Prompt management + evaluation + monitoring | humanloop.com |
| **Patronus AI** | Automated LLM evaluation, hallucination detection | patronus.ai |
| **Galileo** | RAG evaluation, hallucination index, guardrails | rungalileo.io |
| **Dynamo AI** | Enterprise AI safety, red-teaming at scale | dynamo.ai |

## 5.3 Tool Selection Guide

```
WHICH TOOL SHOULD I START WITH?
═══════════════════════════════

"I want to test prompts quickly with YAML configs"
  → Promptfoo

"I want Python-based evaluation with rich metrics"
  → DeepEval

"I'm building a RAG system and need RAG-specific eval"
  → Ragas + TruLens

"I need to find vulnerabilities and bias"
  → Giskard

"I'm using LangChain and need tracing + eval"
  → LangSmith

"I'm in an Azure enterprise environment"
  → Azure AI Studio Evaluation

"I want to start free and scale later"
  → Promptfoo (open source) → Add LangSmith or Azure as you grow
```

---

# Part 6: API & AI Product Testing

## 6.1 Testing AI API Products

### Resources for AI API Testing

| Resource | Type | Link / Location | Time |
|---|---|---|---|
| **OpenAI — API Reference** | Docs | platform.openai.com → API Reference | 2 hours |
| **Anthropic — API Reference** | Docs | docs.anthropic.com → API Reference | 1 hour |
| **Postman — "Testing APIs"** | Course | Postman Learning Center | 2 hours |
| **"REST API Design Best Practices"** | Article | Various (Swagger Blog, Microsoft API Guidelines) | 1 hour |
| **k6 — Load Testing Guide** | Docs | k6.io → Documentation (for performance testing AI APIs) | 2 hours |
| **Locust — Load Testing for AI** | Docs | locust.io → Documentation | 1 hour |

### What to Test in AI APIs

```
AI API TEST CATEGORIES
══════════════════════

1. CONTRACT TESTING
   - Request schema validation
   - Response schema validation
   - Error response format
   - Streaming vs non-streaming responses

2. FUNCTIONAL TESTING
   - Correct model routing
   - System prompt handling
   - Temperature/parameter effects
   - Tool calling / function calling
   - Multi-turn conversation state

3. PERFORMANCE TESTING
   - Time to first token (TTFT)
   - Tokens per second (TPS)
   - Concurrent request handling
   - Token limit behavior
   - Streaming latency

4. SECURITY TESTING
   - Authentication (API keys, OAuth)
   - Rate limiting enforcement
   - Input sanitization
   - PII handling in request/response
   - CORS and header security

5. COST TESTING
   - Token counting accuracy
   - Cost per request class
   - Budget limit enforcement
   - Token optimization validation

6. RELIABILITY TESTING
   - Retry behavior on 429/500
   - Timeout handling
   - Partial response handling
   - Failover between model versions
```

## 6.2 AI Product Testing Lifecycle

```
AI PRODUCT TEST LIFECYCLE
═════════════════════════

Phase 1: DESIGN REVIEW
  │ - Review system prompt
  │ - Review tool definitions
  │ - Review safety requirements
  │ - Identify risk categories
  │
Phase 2: GOLDEN DATASET
  │ - Create 50-100 test cases
  │ - Get domain expert review
  │ - Tag by category and priority
  │
Phase 3: AUTOMATED SUITE
  │ - Set up Promptfoo or DeepEval
  │ - Write assertions (contains, regex, LLM-judge)
  │ - Integrate into CI/CD
  │
Phase 4: SAFETY TESTING
  │ - Run red-team suite
  │ - Run bias counterfactuals
  │ - Run injection attacks
  │ - Run Giskard vulnerability scan
  │
Phase 5: HUMAN EVALUATION
  │ - Sample 50 responses
  │ - Score on rubric (correctness, helpfulness, safety, tone)
  │ - Calculate inter-annotator agreement
  │
Phase 6: PERFORMANCE TESTING
  │ - Load test with k6/Locust
  │ - Measure TTFT and TPS
  │ - Verify SLA compliance
  │
Phase 7: MONITORING
  │ - Deploy observability (traces, scores, alerts)
  │ - Set up drift detection
  │ - Weekly metric reviews
  │
Phase 8: CONTINUOUS EVALUATION
    - Regression suite on every change
    - Monthly red-team exercises
    - Quarterly bias audits
    - Annual compliance review
```

---

# Part 7: Certifications & Structured Courses

## 7.1 Free Courses (Start Here)

| Course | Provider | Duration | Level |
|---|---|---|---|
| **"Introduction to Generative AI"** | Microsoft Learn | 2 hours | Basic |
| **"Introduction to Large Language Models"** | Google Cloud Skills Boost | 1 hour | Basic |
| **"ChatGPT Prompt Engineering for Developers"** | DeepLearning.AI | 1.5 hours | Basic |
| **"Building and Evaluating Advanced RAG"** | DeepLearning.AI | 1.5 hours | Intermediate |
| **"Automated Testing for LLMOps"** | DeepLearning.AI | 1 hour | Intermediate |
| **"Quality and Safety for LLM Applications"** | DeepLearning.AI | 1.5 hours | Intermediate |
| **"Red Teaming LLM Applications"** | DeepLearning.AI | 1 hour | Intermediate |
| **"Functions, Tools and Agents with LangChain"** | DeepLearning.AI | 1.5 hours | Advanced |
| **"AI Agents in LangGraph"** | DeepLearning.AI | 1.5 hours | Advanced |
| **"LLMOps"** | DeepLearning.AI + Google Cloud | 1 hour | Advanced |

## 7.2 Paid Courses & Certifications

| Course / Cert | Provider | Duration | Level | Cost |
|---|---|---|---|---|
| **"AI Engineering" Specialization** | Coursera / DeepLearning.AI | 3 months | Intermediate-Advanced | ~$50/month |
| **Microsoft Certified: Azure AI Engineer Associate (AI-102)** | Microsoft | Self-paced | Intermediate | $165 exam |
| **Google Cloud Professional ML Engineer** | Google Cloud | Self-paced | Advanced | $200 exam |
| **AWS Certified Machine Learning - Specialty** | AWS | Self-paced | Advanced | $300 exam |
| **Certified Kubernetes Application Developer (CKAD)** | CNCF | Self-paced | Advanced (for MLOps/deployment) | $395 exam |
| **ISTQB AI Testing Certification (CT-AI)** | ISTQB | Self-paced | Intermediate | ~$250 exam |

## 7.3 Books — Complete Reading List

| Book | Author | Level | Focus |
|---|---|---|---|
| **"AI Engineering"** | Chip Huyen (2025) | Intermediate-Advanced | Best single book for AI application building + evaluation |
| **"Designing Machine Learning Systems"** | Chip Huyen (2022) | Intermediate | ML systems design, monitoring, testing |
| **"Prompt Engineering Guide"** | DAIR.AI (free online) | Basic-Intermediate | Comprehensive prompt engineering |
| **"Building LLMs for Production"** | Oreilly / Various | Intermediate | Practical LLM deployment |
| **"Natural Language Processing with Transformers"** | Lewis Tunstall et al. (O'Reilly) | Intermediate | Deep understanding of transformer models |
| **"Hands-On Large Language Models"** | Jay Alammar & Maarten Grootendorst (O'Reilly, 2024) | Basic-Intermediate | Visual, practical LLM guide |
| **"Responsible AI"** | Various / O'Reilly | Intermediate | Ethics, bias, fairness in AI |

---

# Part 8: Hands-On Practice Labs

## 8.1 Build These Projects (Progressively)

### Project 1: BASIC — Prompt Testing with Promptfoo
```
WHAT: Test 5 different system prompts for a banking FAQ bot
SKILLS: Prompt engineering, assertion writing, YAML config
TOOLS: Promptfoo, OpenAI API
TIME: 4-6 hours

Steps:
1. Install Promptfoo: npx promptfoo@latest init
2. Create 20 test cases in YAML
3. Write assertions: contains, not-contains, llm-rubric
4. Run evaluation: npx promptfoo eval
5. View results: npx promptfoo view
6. Compare 3 different system prompts side by side
```

### Project 2: BASIC — Golden Dataset Evaluation
```
WHAT: Create and evaluate a golden dataset for a domain
SKILLS: Test case design, accuracy measurement, basic metrics
TOOLS: Python, OpenAI API, spreadsheet
TIME: 6-8 hours

Steps:
1. Choose a domain (banking, healthcare, HR)
2. Write 50 question-answer-source triplets
3. Send each question to an LLM, collect responses
4. Score each response (correct/partial/incorrect)
5. Calculate overall accuracy, identify failure patterns
6. Write a test report
```

### Project 3: INTERMEDIATE — RAG System Evaluation
```
WHAT: Build and evaluate a complete RAG system
SKILLS: RAG architecture, retrieval metrics, Ragas framework
TOOLS: LangChain, ChromaDB/FAISS, Ragas, Python
TIME: 10-15 hours

Steps:
1. Collect 20 PDF documents (use banking or any domain)
2. Build a RAG pipeline: load → chunk → embed → store → retrieve → generate
3. Create 30 evaluation questions with ground truth
4. Run Ragas evaluation:
   - Faithfulness
   - Answer Relevance
   - Context Precision
   - Context Recall
5. Analyze results: Where does retrieval fail? Where does generation fail?
6. Optimize (change chunk size, add reranker, adjust prompt)
7. Re-evaluate and compare
```

### Project 4: INTERMEDIATE — Red-Team a Chatbot
```
WHAT: Systematically attack an AI chatbot for vulnerabilities
SKILLS: Prompt injection, safety testing, adversarial thinking
TOOLS: Any chatbot + Giskard or manual testing
TIME: 6-8 hours

Steps:
1. Deploy a simple chatbot (or use a public one)
2. Run 20 prompt injection attacks
3. Run 10 bias tests (counterfactual pairs)
4. Run 10 data leakage tests
5. Score each: pass/fail + severity
6. Write a red-team report with findings and recommendations
```

### Project 5: ADVANCED — Agent Trajectory Testing
```
WHAT: Build a tool-using agent and test its behavior
SKILLS: Agent architecture, tool calling, trajectory evaluation
TOOLS: LangChain/LangGraph, LangSmith, Python
TIME: 15-20 hours

Steps:
1. Build an agent with 4 tools:
   - check_balance(account_id)
   - transfer_funds(from, to, amount)
   - get_transactions(account_id, days)
   - escalate_to_human(reason)
2. Write 15 test scenarios covering:
   - Happy path (correct tool sequence)
   - Error handling (tool failures)
   - Safety boundaries (dangerous actions)
   - Edge cases (ambiguous requests)
3. Use LangSmith to trace each run
4. Assert on tool call sequences, parameters, and final output
5. Generate a test report with pass rates per category
```

### Project 6: ADVANCED — CI/CD Evaluation Pipeline
```
WHAT: Build an automated evaluation pipeline that runs on every change
SKILLS: CI/CD for AI, automated evaluation, monitoring
TOOLS: Promptfoo or DeepEval, GitHub Actions, Python
TIME: 10-15 hours

Steps:
1. Set up a Git repository with your AI application
2. Create evaluation dataset (50+ test cases in YAML/JSON)
3. Write GitHub Action that:
   a. Triggers on push to main
   b. Runs evaluation suite
   c. Checks gates (accuracy ≥ 80%, safety = 100%)
   d. Posts results as PR comment
   e. Blocks merge if gates fail
4. Set up a dashboard to track metrics over time
5. Add alert for any safety failures
```

---

# Part 9: Communities & Staying Current

## 9.1 Communities to Join

| Community | Platform | Why Join |
|---|---|---|
| **r/MachineLearning** | Reddit | Latest papers and discussions |
| **r/LocalLLaMA** | Reddit | Open-source LLM community |
| **MLOps Community** | Slack | MLOps and LLMOps practitioners |
| **LangChain Discord** | Discord | LangChain users, RAG discussions |
| **Weights & Biases Community** | Slack/Forum | ML experiment tracking community |
| **AI Quality Engineering** | LinkedIn Groups | AI testing professionals |
| **OWASP LLM Top 10** | OWASP | AI security community |
| **Ministry of Testing** | Website/Slack | Testing community with AI testing track |
| **Latent Space Podcast Community** | Discord/Podcast | AI engineering discussions |

## 9.2 Newsletters & Blogs to Follow

| Source | Type | Focus |
|---|---|---|
| **The Batch (Andrew Ng)** | Newsletter (weekly) | AI news and practical insights |
| **Ahead of AI (Sebastian Raschka)** | Newsletter | LLM research explained practically |
| **Lilian Weng's Blog** | Blog | Deep technical AI explanations |
| **Chip Huyen's Blog** | Blog | AI engineering, MLOps, evaluation |
| **Eugene Yan's Blog** | Blog | ML systems, LLM patterns, evaluation |
| **Hamel Husain's Blog** | Blog | LLM evaluation, practical AI engineering |
| **Simon Willison's Blog** | Blog | LLM tools, prompt engineering, security |
| **The AI Monitor (Hugging Face)** | Newsletter | Open-source AI updates |
| **Last Week in AI** | Newsletter | Weekly AI news roundup |

## 9.3 Conferences & Events

| Event | Focus | Frequency |
|---|---|---|
| **NeurIPS** | AI/ML research (evaluation papers) | Annual (December) |
| **ICML** | ML research | Annual (July) |
| **AI Engineer Summit** | AI engineering practice | Annual |
| **AI Quality Conference** | AI testing and quality | Annual |
| **Microsoft Build** | Azure AI, Responsible AI | Annual (May) |
| **Google I/O** | Google AI, Gemini | Annual (May) |
| **QCon** | Software engineering (AI track) | Multiple/year |

---

# Part 10: Recommended Learning Path (Week-by-Week)

## 10.1 The 12-Week Plan

```
WEEK 1-2: FOUNDATIONS
═════════════════════
□ Watch: Karpathy "Intro to LLMs" (1 hr)
□ Course: Microsoft "Intro to Generative AI" (2 hrs)
□ Course: DeepLearning.AI "Prompt Engineering for Developers" (1.5 hrs)
□ Read: OpenAI API documentation (2 hrs)
□ Practice: Send 20 API calls. Experiment with temperature, system prompts.
□ Read: Chapters 1-3 of "AI Engineering" by Chip Huyen

WEEK 3-4: RAG & RETRIEVAL
══════════════════════════
□ Read: IBM "What is RAG?" + Pinecone "What are Embeddings?"
□ Course: DeepLearning.AI "LangChain: Chat with Your Data" (1 hr)
□ Read: "Seven Failure Points When Engineering a RAG" paper
□ Practice: Build a simple RAG system with 10 documents
□ Practice: PROJECT 2 — Golden Dataset Evaluation
□ Read: Chapters 4-6 of "AI Engineering" by Chip Huyen

WEEK 5-6: EVALUATION FUNDAMENTALS
══════════════════════════════════
□ Read: Hamel Husain "Your AI Product Needs Evals"
□ Course: DeepLearning.AI "Building and Evaluating Advanced RAG" (1.5 hrs)
□ Read: Ragas documentation — all metrics explained
□ Practice: PROJECT 1 — Prompt Testing with Promptfoo
□ Practice: Set up DeepEval and run built-in metrics
□ Read: "Judging LLM-as-a-Judge" paper

WEEK 7-8: SAFETY & BIAS
════════════════════════
□ Read: OWASP Top 10 for LLM Applications (full document)
□ Read: Microsoft "Red teaming LLM Applications"
□ Course: DeepLearning.AI "Quality and Safety for LLM Applications"
□ Course: DeepLearning.AI "Red Teaming LLM Applications"
□ Practice: PROJECT 4 — Red-Team a Chatbot
□ Read: NIST AI RMF (skim for relevance)

WEEK 9-10: RAG DEEP DIVE & AGENTS
══════════════════════════════════
□ Practice: PROJECT 3 — RAG System Evaluation (full project)
□ Read: Anthropic "Building Effective Agents"
□ Course: DeepLearning.AI "AI Agents in LangGraph"
□ Read: LangSmith documentation — tracing and evaluation
□ Practice: Start PROJECT 5 — Agent Trajectory Testing
□ Read: "AgentBench" paper

WEEK 11-12: ENTERPRISE & CI/CD
═══════════════════════════════
□ Complete: PROJECT 5 — Agent Trajectory Testing
□ Practice: PROJECT 6 — CI/CD Evaluation Pipeline
□ Read: Chip Huyen "Building a Generative AI Platform"
□ Read: Azure "Baseline OpenAI E2E chat reference architecture"
□ Review: All projects, consolidate learnings
□ Create: Your personal AI testing playbook

ONGOING (After Week 12):
  □ Weekly: Follow newsletters (The Batch, Ahead of AI)
  □ Monthly: Read 2 new papers from arxiv
  □ Monthly: Try a new evaluation tool
  □ Quarterly: Update your test suite with new techniques
  □ Quarterly: Attend 1 meetup or conference talk
```

## 10.2 Skill Progression Tracker

```
BASIC (Weeks 1-4)                 DONE?
════════════════                  ═════
□ Can explain how LLMs generate text         [ ]
□ Can explain what RAG is and why it matters  [ ]
□ Can write effective system prompts          [ ]
□ Can create a golden evaluation dataset      [ ]
□ Can use OpenAI / Anthropic APIs             [ ]
□ Can identify the 5 main AI failure modes    [ ]
□ Can write basic test assertions             [ ]

INTERMEDIATE (Weeks 5-8)
════════════════════════
□ Can set up Promptfoo and run evaluations    [ ]
□ Can use LLM-as-Judge evaluation             [ ]
□ Can evaluate RAG with Ragas metrics         [ ]
□ Can run red-team attacks on chatbots        [ ]
□ Can design counterfactual bias tests        [ ]
□ Can explain groundedness vs hallucination   [ ]
□ Can write evaluation rubrics                [ ]
□ Can calculate inter-annotator agreement     [ ]

ADVANCED (Weeks 9-12)
═════════════════════
□ Can test agent tool-calling trajectories    [ ]
□ Can build CI/CD evaluation pipelines        [ ]
□ Can set up LLM observability               [ ]
□ Can design evaluation for multi-turn convos [ ]
□ Can test for indirect prompt injection      [ ]
□ Can comply with OWASP Top 10 for LLMs     [ ]
□ Can create enterprise release gates         [ ]
□ Can track evaluation metrics over time      [ ]
```

---

## Quick Reference — All Resources by Type

### Papers (Most Important)

| # | Paper | Year | Topic |
|---|---|---|---|
| 1 | Retrieval-Augmented Generation (Lewis et al.) | 2020 | Original RAG paper |
| 2 | Judging LLM-as-a-Judge (Zheng et al.) | 2023 | LLM evaluation methodology |
| 3 | G-Eval (Liu et al.) | 2023 | LLM-based evaluation framework |
| 4 | Red Teaming Language Models (Ganguli et al.) | 2022 | Safety testing |
| 5 | DecodingTrust (Wang et al.) | 2023 | Comprehensive trustworthiness |
| 6 | Seven Failure Points in RAG (Barnett et al.) | 2024 | RAG failure analysis |
| 7 | AgentBench (Liu et al.) | 2023 | Agent evaluation |
| 8 | Indirect Prompt Injection (Greshake et al.) | 2023 | Security in RAG |
| 9 | ARES (Saad-Falcon et al.) | 2024 | Automated RAG evaluation |
| 10 | HELM (Liang et al.) | 2022 | Holistic LLM evaluation |

### Tools (Quick Start Priority)

| Priority | Tool | First Command |
|---|---|---|
| 1st | **Promptfoo** | `npx promptfoo@latest init` |
| 2nd | **DeepEval** | `pip install deepeval` |
| 3rd | **Ragas** | `pip install ragas` |
| 4th | **Giskard** | `pip install giskard` |
| 5th | **LangSmith** | Sign up at smith.langchain.com |

### Books (Reading Order)

| Order | Book | Author |
|---|---|---|
| 1st | **"Hands-On Large Language Models"** | Alammar & Grootendorst |
| 2nd | **"AI Engineering"** | Chip Huyen |
| 3rd | **"Designing Machine Learning Systems"** | Chip Huyen |
| 4th | **"NLP with Transformers"** | Tunstall et al. |

---
# AI Testing — Best Blogs, Channels & Community Resources

> **Created**: March 20, 2026  
> **Purpose**: Curated blogs, YouTube channels, podcasts, substacks, GitHub repos, and community resources for AI testers  
> **Companion to**: AI-Testing-Learning-Resources.md

---

## Table of Contents

- [Tier 1: Must-Read Blogs for AI Testers](#tier-1-must-read-blogs-for-ai-testers)
- [Tier 2: Deep Technical AI Blogs](#tier-2-deep-technical-ai-blogs)
- [Tier 3: AI Safety, Security & Red-Teaming Blogs](#tier-3-ai-safety-security--red-teaming-blogs)
  - [Tier 3a — Industry & Strategy Perspective](#tier-3a--industry--strategy-perspective)
  - [Tier 3b — Testing Community Specific](#tier-3b--testing-community-specific)
- [Tier 4: Industry & Practitioner Blogs](#tier-4-industry--practitioner-blogs)
- [Top 5 Must-Read Blog Posts](#top-5-must-read-blog-posts)
- [YouTube Channels](#youtube-channels)
- [Podcasts](#podcasts)
- [Substacks & Newsletters](#substacks--newsletters)
- [GitHub Repositories](#github-repositories)
- [Twitter/X Accounts to Follow](#twitterx-accounts-to-follow)
- [Company Engineering Blogs](#company-engineering-blogs)
- [Additional Resources (Extended)](#additional-resources-extended)
  - [Substacks / Deeper Niche Blogs](#substacks--deeper-niche-blogs)
  - [GitHub Repositories (Study + Reference)](#github-repositories-study--reference)
  - [Twitter/X Accounts to Follow (Extended)](#twitterx-accounts-to-follow-extended)
  - [Hidden Gem — Company Engineering Blogs](#hidden-gem--company-engineering-blogs)
- [Pure Niche Blogs — By Category](#pure-niche-blogs--by-category)

---

## Tier 1: Must-Read Blogs for AI Testers

> These are the most directly relevant to AI testing and evaluation.

| # | Blog | Author/Org | Why It's Essential | Key Posts |
|---|---|---|---|---|
| 1 | **hamel.dev** | Hamel Husain | The single best blog for AI evaluation. Hamel was at GitHub (Copilot), Airbnb, and is deeply focused on LLM evals. | "Your AI Product Needs Evals", "Creating a LLM-as-a-Judge That Drives Business Results" |
| 2 | **eugeneyan.com** | Eugene Yan (Amazon) | LLM patterns, evaluation strategies, RAG testing. Incredibly well-structured posts. | "Patterns for Building LLM-based Systems & Products", "Evaluation Strategies for LLMs" |
| 3 | **huyenchip.com** | Chip Huyen | AI Engineering, MLOps, evaluation, building AI platforms. Author of the best AI Engineering book. | "Building a Generative AI Platform", "Evaluation Metrics for LLMs" |
| 4 | **simonwillison.net** | Simon Willison | LLM tools, prompt injection security, practical AI testing. Co-creator of Django. Posts daily. | Prompt injection series, LLM tool reviews, security analyses |
| 5 | **lilianweng.github.io** | Lilian Weng (OpenAI) | Deep technical explainers on LLM concepts. Best for understanding what you're testing. | "Prompt Engineering", "LLM Powered Autonomous Agents", "Anti-hype LLM reading list" |
| 6 | **huyenchip.com** | Chip Huyen | Author of "AI Engineering." Writes about AI platform design, evaluation pipelines, and MLOps. | "Building a Generative AI Platform" — maps the entire architecture |
| 7 | **shreya-shankar.com** | Shreya Shankar (UC Berkeley) | Research on LLM evaluation methodology. How we evaluate LLMs is cutting-edge. | LLM evaluation research papers |

---

## Tier 2: Deep Technical AI Blogs

> Great for understanding the systems you're testing at a deeper level.

| # | Blog | Author/Org | Focus |
|---|---|---|---|
| 6 | **jayalammar.com** | Jay Alammar | Visual explanations of transformers, embeddings, LLMs — best for visual learners |
| 7 | **sebastianraschka.com** | Sebastian Raschka | LLM research explained practically. Author of "Build an LLM from Scratch" |
| 8 | **shreya-shankar.com** | Shreya Shankar (UC Berkeley) | LLM evaluation research, data quality for AI, rethinking eval approaches |
| 9 | **cameronrwolfe.substack.com** | Cameron R. Wolfe | Deep dives into AI research papers — makes papers accessible |
| 10 | **promptingguide.ai** | DAIR.AI | Comprehensive prompt engineering reference — useful for testing prompt robustness |
| 11 | **jxnl.co** | Jason Liu | Creator of Instructor library. Writes about structured output testing, LLM reliability, and practical AI engineering patterns. |

---

## Tier 3: AI Safety, Security & Red-Teaming Blogs

> Essential for safety testing, red-teaming, and compliance.

| # | Blog | Author/Org | Focus |
|---|---|---|---|
| 11 | **OWASP LLM Top 10 Blog** | OWASP | Official updates on LLM security vulnerabilities and mitigations |
| 12 | **Anthropic Research Blog** | Anthropic | AI safety research, constitutional AI, red-teaming methodologies |
| 13 | **Microsoft Responsible AI Blog** | Microsoft | Enterprise AI safety, responsible AI tooling, evaluation frameworks |
| 14 | **Google AI Blog — Safety Section** | Google DeepMind | AI safety research, evaluation benchmarks, alignment |
| 15 | **NIST AI Blog** | NIST | AI risk management, regulatory frameworks, compliance guidance |
| 16 | **Trail of Bits Blog** | Trail of Bits | Security auditing applied to AI systems, adversarial ML |
| 17 | **Lakera AI Blog** | Lakera | Prompt injection defense, LLM security, gandalf challenges |

### Tier 3a — Industry & Strategy Perspective

| # | Blog | Author/Org | Best For |
|---|---|---|---|
| 18 | **anthropic.com/research** | Anthropic | Red-teaming methodology, safety research, evaluation approaches straight from a model provider |
| 19 | **openai.com/research** | OpenAI | System card analysis, safety evaluations, model capabilities |
| 20 | **ai.meta.com/blog** | Meta AI | Open-source model evaluations (Llama), benchmarking methodology |

### Tier 3b — Testing Community Specific

| # | Blog | Best For |
|---|---|---|
| 21 | **ministryoftesting.com** | AI testing track — practical QA perspective on AI systems |
| 22 | **promptingguide.ai (DAIR.AI)** | Living document on prompt engineering — updated frequently |
| 23 | **owasp.org/llm** | LLM security vulnerabilities, top 10 risks — updated annually |

---

## Tier 4: Industry & Practitioner Blogs

> Real-world AI testing experiences and patterns.

| # | Blog | Author/Org | Focus |
|---|---|---|---|
| 18 | **LangChain Blog** | LangChain | RAG patterns, agent architectures, evaluation with LangSmith |
| 19 | **Pinecone Blog / Learning Center** | Pinecone | Vector search, RAG evaluation, retrieval optimization |
| 20 | **Arize AI Blog** | Arize | LLM observability, evaluation metrics, monitoring in production |
| 21 | **Confident AI Blog** | Confident AI (DeepEval) | LLM evaluation metrics, testing strategies, benchmark results |
| 22 | **Braintrust Blog** | Braintrust | Practical evaluation, LLM-as-Judge tips, online eval patterns |
| 23 | **Weights & Biases Blog** | W&B | Experiment tracking, LLM evaluation, ML monitoring |
| 24 | **Ministry of Testing — AI Track** | Community | Testing practitioners sharing AI testing experiences |
| 25 | **Patronus AI Blog** | Patronus AI | Hallucination detection, automated LLM evaluation research |

---

## Top 5 Must-Read Blog Posts

> If you read nothing else, read these five.

| # | Post | Author | Why Read It |
|---|---|---|---|
| 1 | **"Your AI Product Needs Evals"** | Hamel Husain | Foundational thinking on why and how to evaluate AI — start here |
| 2 | **"Patterns for Building LLM-based Systems & Products"** | Eugene Yan | Maps the entire landscape of LLM patterns — tells you what to test |
| 3 | **"Building a Generative AI Platform"** | Chip Huyen | End-to-end architecture — shows every component you need to evaluate |
| 4 | **"Prompt Injection" series** | Simon Willison | Best explanation of the #1 AI security risk — essential for safety testing |
| 5 | **"Prompt Engineering"** | Lilian Weng | Deep technical reference for understanding prompts — the foundation of all AI testing |

---

## YouTube Channels

> Best video content for AI testing and evaluation.

| Channel | Focus | Best For |
|---|---|---|
| **Andrej Karpathy** | LLM fundamentals from first principles | Understanding what you're testing — "Intro to LLMs" and "Let's build GPT" are essential |
| **3Blue1Brown** | Visual math/neural network explanations | Understanding transformers and embeddings visually |
| **DeepLearning.AI** | Short courses on RAG, agents, evals | Hands-on eval skills — all free short courses |
| **Yannic Kilcher** | AI paper walkthroughs | Understanding evaluation papers and benchmarks |
| **AI Engineer** | Conference talks from AI practitioners | Real-world AI engineering talks, evaluation sessions |
| **Two Minute Papers** | Quick AI research summaries | Staying current on AI capabilities (know what to test) |
| **Sam Witteveen** | LangChain, RAG, agent tutorials | Practical building + testing tutorials |
| **Matt Williams** | Ollama, local LLM tutorials | Testing open-source models locally |
| **James Briggs** | Pinecone, RAG, vector search tutorials | RAG implementation and testing |
| **Dave Ebbelaar** | AI engineering tutorials | End-to-end AI application building |
| **AI Jason** | Practical AI engineering tutorials, RAG implementations, agent builds | Building and testing RAG and agent systems |
| **Fireship** | Quick overviews of AI concepts | Great for initial understanding of new AI topics |

---

## Podcasts

> Listen during commute or walks — stay current without screen time.

| Podcast | Host(s) | Focus | Frequency |
|---|---|---|---|
| **Latent Space** | Alessio Fanelli, swyx | AI engineering deep dives — interviews top AI engineers | Weekly |
| **Practical AI** | Daniel Whitenack, Chris Benson | Making AI practical — good for testing perspective | Weekly |
| **The TWIML AI Podcast** | Sam Charrington | AI/ML research and practice — academic + industry | Weekly |
| **Gradient Dissent** | Weights & Biases team | ML practitioners sharing real experiences | Biweekly |
| **AI Engineering Podcast** | Various | Focused on building AI applications | Weekly |
| **Lex Fridman Podcast** | Lex Fridman | Long-form AI interviews with researchers and builders | Weekly |
| **Last Week in AI** | Andrey Kurenkov | Weekly AI news roundup podcast | Weekly |
| **Not So Standard Deviations** | Roger Peng, Hilary Parker | Data science practice discussions | Biweekly |
| **High Agency** | Imbue | Deep dives into agent behavior, reasoning, evaluation | Weekly |

---

## Substacks & Newsletters

> Curated reading delivered to your inbox.

| Substack/Newsletter | Author | Focus | Frequency |
|---|---|---|---|
| **The Batch** | Andrew Ng / DeepLearning.AI | AI news digest — most accessible weekly AI summary | Weekly |
| **Ahead of AI** | Sebastian Raschka | LLM research explained with practical implications | Biweekly |
| **Interconnects** | Nathan Lambert | RLHF, alignment, LLM training — understand what you're evaluating | Weekly |
| **The Gradient** | Stanford AI community | AI research summaries and interviews | Weekly |
| **Import AI** | Jack Clark (Anthropic co-founder) | AI policy, safety, and capabilities | Weekly |
| **AI Tidbits** | Sahar Mor | Curated AI engineering links and summaries | Weekly |
| **Transformer Circuits Thread** | Anthropic | Mechanistic interpretability research | Irregular |
| **By Howard** | Howard Huang | AI engineering practical insights | Weekly |
| **The Sequence** | Jesus Rodriguez | Enterprise AI, MLOps updates | Weekly |
| **Davis Summarizes Papers** | Davis Blalock | Quick paper summaries — great for staying current on eval research | Weekly |

---

## GitHub Repositories

> Hands-on code and frameworks to practice with.

| Repository | What It Is | Why It's Useful for Testers |
|---|---|---|
| **promptfoo/promptfoo** | LLM evaluation framework | #1 tool to learn for prompt/LLM testing |
| **confident-ai/deepeval** | Python LLM evaluation | 14+ built-in metrics, Python-native testing |
| **explodinggradients/ragas** | RAG evaluation framework | Standard for RAG quality measurement |
| **Giskard-AI/giskard** | AI vulnerability scanner | Automated bias and vulnerability detection |
| **trulens/trulens** | RAG Triad evaluation | Feedback functions for RAG systems |
| **OWASP/www-project-top-10-for-llm-applications** | LLM security top 10 | Security test case reference |
| **leondz/garak** | LLM vulnerability scanner | Automated red-teaming tool |
| **microsoft/counterfit** | Adversarial ML testing | Enterprise adversarial testing framework |
| **jxnl/instructor** | Structured LLM outputs | Testing structured output parsing |
| **langchain-ai/langchain** | LLM application framework | Understanding what you're testing |
| **run-llama/llama_index** | RAG framework | RAG evaluation module included |
| **huggingface/evaluate** | ML evaluation library | Standard metrics implementation |
| **EleutherAI/lm-evaluation-harness** | LLM benchmarking suite | Running standard benchmarks locally |
| **openai/evals** | OpenAI evaluation framework | OpenAI's approach to LLM evaluation |

---

## Twitter/X Accounts to Follow

> Real-time AI insights and discussions.

| Account | Who | Why Follow |
|---|---|---|
| **@haaborui** | Hamel Husain | LLM evaluation insights, practical advice |
| **@eugeneyan** | Eugene Yan | LLM patterns, evaluation strategies |
| **@chiphuyen** | Chip Huyen | AI engineering, evaluation, MLOps |
| **@simonw** | Simon Willison | Daily LLM updates, prompt injection research |
| **@lilaborudi** | Lilian Weng | OpenAI research, technical AI content |
| **@kaborodigital** | Sebastian Raschka | LLM research explained |
| **@swaborudi** | Shreya Shankar | LLM evaluation research |
| **@jaborudi** | Jason Wei | Google DeepMind, evaluation methodology |
| **@AndrewYNg** | Andrew Ng | AI education, industry trends |
| **@karpathy** | Andrej Karpathy | LLM fundamentals, AI education |
| **@oaborudi** | OWASP LLM | LLM security updates |
| **@laborudi** | Lakera AI | Prompt injection defense |

---

## Company Engineering Blogs

> How top companies test and evaluate their AI systems.

| Company Blog | Focus Area | Key AI Testing Content |
|---|---|---|
| **OpenAI Blog** | Model capabilities, safety research | Evaluation methodology, safety testing approaches |
| **Anthropic Research** | AI safety, constitutional AI | Red-teaming, alignment evaluation, safety testing |
| **Google AI Blog** | DeepMind research, evaluation | Benchmarks, responsible AI, evaluation frameworks |
| **Microsoft AI Blog** | Azure AI, Responsible AI | Enterprise evaluation, safety tooling, compliance |
| **Meta AI Blog** | Open-source models (Llama), research | Model evaluation, open benchmarks |
| **Hugging Face Blog** | Open-source ML tooling | Evaluation libraries, model benchmarks, leaderboards |
| **Netflix Tech Blog** | ML at scale | A/B testing, ML monitoring, production evaluation |
| **Uber Engineering** | ML operations | ML testing in production, monitoring patterns |
| **Airbnb Tech Blog** | ML systems | ML quality, experimentation, evaluation |
| **Spotify Engineering** | ML/AI in production | Recommendation evaluation, ML testing |
| **LinkedIn Engineering** | AI at scale | LLM deployment, quality evaluation, responsible AI |
| **Stripe Blog** | AI for fintech | Financial AI safety, fraud detection evaluation |

---

## Reading Strategy

```
HOW TO USE THESE RESOURCES
══════════════════════════

DAILY (5-10 min):
  → Scan Twitter/X for breaking AI news
  → Check 1-2 blog RSS feeds

WEEKLY (30-60 min):
  → Read The Batch newsletter
  → Read 1 blog post from Tier 1 blogs
  → Listen to 1 podcast episode

MONTHLY (2-3 hours):
  → Deep read 2-3 posts from Tier 2/3 blogs
  → Explore 1 new GitHub repository
  → Watch 1 YouTube deep dive

QUARTERLY:
  → Review OWASP LLM Top 10 for updates
  → Check for new evaluation tools/frameworks
  → Read 1 company engineering blog case study
```

---

## Additional Resources (Extended)

> Additional blogs, repos, accounts, and engineering blogs collected from deeper research. Some entries overlap with sections above — kept for alternate descriptions and context.

### Substacks / Deeper Niche Blogs

| Blog | Author | Why It's Valuable |
|---|---|---|
| **The Pragmatic Engineer** | Gergely Orosz | Occasionally covers AI engineering from a senior engineering perspective — great for understanding how AI fits into real product teams |
| **Interconnects** | Nathan Lambert (ex-HuggingFace) | Deep on RLHF, model alignment, evaluation methodology — understand how models are trained and why they fail |
| **Cameron Wolfe's Newsletter** | Cameron Wolfe | Weekly deep dives explaining a single AI concept thoroughly — great for building foundational knowledge |
| **AI Snake Oil** | Arvind Narayanan + Sayash Kapoor (Princeton) | Critical perspective on AI hype vs reality — sharpens your skeptical testing mindset |
| **Weights & Biases Blog** | W&B team | Practical MLOps, experiment tracking, production monitoring |
| **The Gradient** | Stanford AI students | Accessible academic perspectives on AI research |
| **Probably Approximately Correct** | Various | Math and theory behind ML evaluation metrics |

### GitHub Repositories (Study + Reference)

| Repo | What It Contains |
|---|---|
| **promptfoo/promptfoo** | Study the test configs and examples directory — real YAML eval patterns |
| **confident-ai/deepeval** | Study metric implementations to understand how evaluation works under the hood |
| **explodinggradients/ragas** | RAG evaluation metric source code — understand faithfulness/relevance scoring |
| **OWASP/www-project-top-10-for-large-language-model-applications** | The full OWASP LLM Top 10 with examples and mitigations |
| **microsoft/counterfit** | Adversarial ML testing tool — study for security testing patterns |
| **huggingface/evaluate** | HuggingFace evaluation library — tons of metric implementations |
| **langchain-ai/langsmith-cookbook** | Real evaluation recipes and patterns |
| **anthropics/evals** | Anthropic's eval methodology examples |

### Twitter/X Accounts to Follow (Extended)

| Account | Why |
|---|---|
| **@simonw** (Simon Willison) | Daily AI security + tooling insights |
| **@chipro** (Chip Huyen) | AI engineering, evaluation, platform design |
| **@eugeneyan** | LLM patterns, evaluation, production systems |
| **@hamaborsa** (Hamel Husain) | LLM evals, practical engineering |
| **@shanaborsa** (Shreya Shankar) | LLM evaluation research |
| **@kaboroevich** (Sebastian Raschka) | LLM research explained |
| **@jaboroevich** (Jason Liu) | Structured outputs, reliability patterns |
| **@swaborson** (Swyx) | AI engineering, Latent Space podcast host |
| **@laboroeng** (Lilian Weng) | Deep technical AI posts (infrequent but high quality) |

### Hidden Gem — Company Engineering Blogs

> These engineering blogs show how top companies actually test and monitor AI in production — invaluable for learning real-world patterns beyond theory.

| Blog | Notable Posts |
|---|---|
| **Notion AI Blog** | How they evaluate their AI features at scale |
| **Stripe Engineering** | ML in production for payments — safety-critical AI |
| **Airbnb Engineering** | ML platform design and testing at scale |
| **LinkedIn Engineering** | AI-assisted features, evaluation methodology |
| **Spotify Engineering** | Recommendation system evaluation (transferable to LLM eval) |
| **Uber Engineering** | ML platform, model monitoring, A/B testing at massive scale |

---

## Pure Niche Blogs — By Category

### AI Evaluation & Testing Niche

| Blog | Author | Niche Focus |
|---|---|---|
| **hamel.dev** | Hamel Husain | LLM evaluation methodology, LLM-as-Judge |
| **shreya-shankar.com** | Shreya Shankar | LLM evaluation research |
| **jxnl.co** | Jason Liu | Structured output testing, LLM reliability |
| **Confident AI Blog** | DeepEval team | LLM evaluation metrics, testing strategies |
| **Patronus AI Blog** | Patronus AI | Hallucination detection, automated LLM eval |
| **Braintrust Blog** | Braintrust | LLM-as-Judge, online/offline eval patterns |
| **AI Snake Oil** | Narayanan & Kapoor (Princeton) | Critical AI evaluation, hype vs reality |

### AI Safety & Red-Teaming Niche

| Blog | Author | Niche Focus |
|---|---|---|
| **Lakera AI Blog** | Lakera | Prompt injection defense, LLM security |
| **Trail of Bits Blog** | Trail of Bits | Security auditing for AI systems |
| **OWASP LLM Top 10 Blog** | OWASP | LLM vulnerability standards |
| **NIST AI Blog** | NIST | AI risk management, compliance |

### AI Observability & Monitoring Niche

| Blog | Author | Niche Focus |
|---|---|---|
| **Arize AI Blog** | Arize | LLM observability, production monitoring |
| **Weights & Biases Blog** | W&B | Experiment tracking, ML monitoring |

### Testing Community Niche

| Blog | Author | Niche Focus |
|---|---|---|
| **Ministry of Testing — AI Track** | Community | QA perspective on AI systems |
| **promptingguide.ai** | DAIR.AI | Prompt engineering reference (testing robustness) |

> **15 pure niche blogs total.** The rest of the blogs in this file (Karpathy, Lilian Weng, Chip Huyen, Eugene Yan, LangChain, etc.) are broader — they cover AI engineering/building with evaluation as one topic among many.

---





