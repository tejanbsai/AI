Becoming an AI Eval Tester — Focused Roadmap
An AI Eval Tester designs, runs, and maintains evaluations that prove an AI system is correct, safe, fair, grounded, and stable — across offline regression, CI gates, and live production.

1. Role Definition
You own: golden datasets, rubrics, LLM-as-judge pipelines, RAG/agent eval harnesses, red-team suites, CI quality gates, and online eval dashboards.

You deliver: trustworthy go/no-go signals for every model, prompt, dataset, or agent change.

2. Core Skill Stack
2.1 Foundations (must have)
Python (pytest, pandas, numpy, async), Git, CI (GitHub Actions)
Stats: mean/median/variance, confidence intervals, hypothesis tests, bootstrap, inter-rater agreement (Cohen's κ, Krippendorff's α)
Classic ML metrics: precision, recall, F1, ROC-AUC, calibration (ECE, Brier)
NLP metrics: BLEU, ROUGE, METEOR, BERTScore, semantic similarity, perplexity
Retrieval metrics: Recall@k, MRR, nDCG, MAP
2.2 LLM-specific
Prompt engineering (role, few-shot, CoT, constraint, structured output)
Tokenization, context windows, temperature/top-p effects on variance
Structured outputs (JSON Schema, function/tool calling) and validation
Embeddings, vector search, chunking strategies
2.3 Eval-specific
Rubric design (definition + 1–5 anchors + pass threshold per criterion)
LLM-as-judge: pointwise, pairwise, reference-based, reference-free
Judge-bias mitigation: position, verbosity, self-preference
Judge validation: judge–human agreement study (target κ ≥ 0.6)
Reliability: seeds, repetition, variance reporting, statistical significance
3. Eval Taxonomy You Must Master
Layer	What you evaluate	Examples
Output quality	Correctness, relevance, format	Reference + rubric scoring
Faithfulness (RAG)	Grounded in retrieved context	Ragas faithfulness, TruLens triad
Retrieval (RAG)	Right docs, right order	Recall@k, nDCG, context precision
Agentic	Tool choice, args, trajectory, task success	LangSmith traces, τ-bench style
Safety / red-team	Jailbreaks, injection, PII, toxicity	Garak, PyRIT, Promptfoo red-team
Bias / fairness	Persona-held-constant deltas	Persona harness, slice metrics
Robustness	Paraphrase, typo, adversarial	NL-Augmenter, CheckList, TextAttack
Calibration	Confidence vs. correctness	Reliability diagrams, ECE
Operational	Latency, cost, stability	p50/p95/p99, $/1k tok, error rate


4. Golden Dataset Discipline
Size: 25–50 per capability to start; grow to 200–1,000 for regression.
Composition: ~60% happy path, 20% edge, 10% adversarial, 10% drift-prone.
Sources: sanitized production logs, SME sessions, synthetic + human curation, public benchmarks for sanity only.
Per case: id, capability tag, persona tag, difficulty, expected behavior, rubric, owner, dataset version.
Versioning: semver the dataset, freeze for releases, track per-case pass/fail history.
Hygiene: dedupe, leakage check vs. training data, quarterly refresh.
5. LLM-as-Judge — Done Right
Write a rubric per dimension (correctness, faithfulness, tone, safety) with 1–5 anchors.
Use chain-of-thought judging (rationale before score).
Prefer pairwise for ranking, pointwise for thresholds.
Randomize position (A/B order) to kill position bias.
Don't self-judge: use a different model family for the judge.
Run a judge validation study: 50–100 items scored by humans, measure κ; iterate the rubric until κ ≥ 0.6.
Report judge variance (run N=3, show std-dev) alongside the score.
6. Tooling (2026 Stack)
Need	Pick
Offline eval framework	DeepEval, Ragas, promptfoo, Inspect AI, OpenAI Evals
Tracing + online evals	LangSmith, Arize Phoenix, Langfuse, TruLens
Red-team / safety	Garak, PyRIT, Giskard, HarmBench
Data validation	Great Expectations, Pandera
Drift	Evidently, NannyML, Arize
Fairness	Fairlearn, AIF360, Aequitas
Prompt/version mgmt	PromptLayer, Humanloop, Langfuse
Experiment tracking	MLflow, Weights & Biases
7. The Eval Pipeline Pattern
  inputs ─► system under test ─► outputs
                               │
                               ├─► deterministic checks (schema, regex, length, JSON valid)
                               ├─► reference metrics (semantic match, BLEU/ROUGE)
                               ├─► LLM-judge rubrics (1–5 per dimension, CoT)
                               ├─► safety scanners (PII, toxicity, jailbreak)
                               ├─► retrieval metrics (RAG)
                               └─► aggregate ─► dashboard + CI gate


Suggested CI gates

Hallucination rate < 2%
Faithfulness ≥ 0.90 (RAG)
Bias delta across personas < 5%
Safety pass rate = 100% on core red-team suite
p95 latency within SLA
Cost regression < 10%
No regression > X% on any subgroup slice



Suggested CI gates

Hallucination rate < 2%
Faithfulness ≥ 0.90 (RAG)
Bias delta across personas < 5%
Safety pass rate = 100% on core red-team suite
p95 latency within SLA
Cost regression < 10%
No regression > X% on any subgroup slice
8. Production / Online Evals
Trace every call: input, output, retrieved context, tool calls, latency, cost, model + prompt version.
Sample 1–5% of live traffic through LLM-judge for online scoring.
Drift signals: input distribution, output distribution, embedding drift, refusal-rate spikes.
Feedback loop: thumbs, free-text, escalations → triaged into golden set within 48h.
Release strategy: shadow → 1% canary → 5% → 25% → 100%, with auto-rollback on gate breach.
Replay harness: re-run sanitized prod traces against candidate model/prompt before promotion.
9. Standards to Map Tests Against
OWASP AI Testing Guide v1.0 — tag each test with AITG-APP/MOD/INF/DAT IDs
OWASP Top 10 for LLMs
NIST AI RMF — Govern / Map / Measure / Manage
ISO/IEC 42001, EU AI Act (high-risk artifacts)
ISTQB CT-AI v2.0 — tester certification baseline
10. 90-Day Plan
Days 1–30 — Foundations + Manual Rubrics

Python + pytest refresher; learn DeepEval and promptfoo basics
Build a 50-case golden set with rubrics for one real feature
Run a judge validation study; reach κ ≥ 0.6
Ship first offline eval report (markdown + JSON artifacts)
Days 31–60 — Automation + CI Gates

Wire DeepEval/promptfoo into CI; fail PRs on gate breach
Add Ragas for a RAG flow (faithfulness, context precision/recall)
Add Garak/PyRIT red-team suite; track pass rate over time
Persona/bias harness with slice dashboards
Days 61–90 — Production + Governance

Tracing + online eval sampling via Langfuse or Phoenix
Drift detection (Evidently) + alerts on hallucination/refusal spikes
Canary + replay harness; auto-rollback on gate breach
Map full test suite to OWASP AITG IDs; produce model card + eval report template
11. Portfolio Artifacts to Build (public GitHub)
promptfoo + DeepEval starter — golden set, rubrics, CI gate, HTML report
Ragas RAG evaluator — faithfulness, context precision/recall, answer relevancy
Judge-validation notebook — human vs. judge agreement, κ, bias diagnostics
Persona/bias harness — slice metrics across demographic axes
Red-team suite — Garak/PyRIT scenarios with pass-rate dashboard
Online eval dashboard — Langfuse/Phoenix traces + sampled judge scoring
AITG-mapped test plan — every test tagged to OWASP AITG IDs



12. What "Done" Looks Like
You can:

Defend a release with rubrics, slices, judge-agreement κ, and adversarial pass rates — not just headline accuracy.
Trace any production failure to a golden-set gap and close it within a day.
Show a single closed loop: manual rubrics → offline evals → CI gates → online evals → feedback → golden set.
Map every test to OWASP AITG / NIST AI RMF / EU AI Act for audit.



The Complete Guide to AI Evaluations (Evals)
A comprehensive reference covering every dimension of evaluating AI systems — from first principles to production governance.

Part I — Foundations
1. What an Eval Is
An evaluation is a repeatable, measurable judgment of an AI system's behavior against a defined expectation. Unlike traditional tests, evals are:

Probabilistic — same input can yield different outputs
Multi-dimensional — quality is not a single number
Rubric-driven — quality requires explicit criteria
Comparative — often relative (A vs. B) rather than absolute
2. Why Evals Exist
Without evals	With evals
"Vibes-based" prompt tuning	Quantified regression tracking
Silent regressions on model swap	Gated releases
No way to compare prompts/models	Reproducible A/B leaderboards
Hallucinations discovered by users	Caught pre-release
No audit trail	Compliance-ready evidence
3. The Eval Stack (Three Loops)
  Loop 3: Production / Online Evals  ── live traffic, drift, feedback
   ↑                                              │
   │                                              ▼
Loop 2: Offline / CI Evals  ── regression, gates, leaderboards
   ↑                                              │
   │                                              ▼
Loop 1: Manual / Exploratory Evals  ── golden sets, rubrics, red-team


Rule: never automate what you have not first validated manually.

Part II — Eval Taxonomy
4. By Reference
Type	Description	When to use
Reference-based	Compare to gold answer	Closed-domain QA, classification, translation
Reference-free	Judge quality without gold	Open-ended generation, summarization
Pairwise / preference	A vs. B winner	Model/prompt selection, RLHF
Rubric-based	Score per criterion (1–5)	Multi-dimensional quality
Behavioral / capability	Pass/fail per skill	Unit-style assertions
5. By What's Measured
Category	Examples
Correctness	Factuality, accuracy, math, code correctness
Faithfulness	Grounded in retrieved context (RAG)
Relevance	Addresses the user's intent
Coherence	Internally consistent, logical
Instruction-following	Honors constraints, format, length
Style / tone	Brand voice, register, locale
Safety	Toxicity, harm, illegal content
Privacy	PII leakage, data exfiltration
Bias / fairness	Persona-held-constant deltas
Robustness	Paraphrase, typo, adversarial stability
Calibration	Confidence vs. correctness
Operational	Latency, cost, throughput, error rate
6. By System Surface
Surface	Eval focus
Prompt	Variant A/B, regression on prompt change
Model	Headline metrics, capability suites
RAG pipeline	Retrieval + generation grounding
Agent	Tool selection, trajectory, task success
Multimodal	Vision/audio/video grounding, OCR fidelity
Fine-tune	Pre/post deltas, regression on base capabilities
End-to-end product	User task success, satisfaction
Part III — Metrics Catalog
7. Classical ML
Task	Metrics
Binary classification	Accuracy, precision, recall, F1, ROC-AUC, PR-AUC, log-loss, MCC
Multi-class	Macro/micro/weighted F1, confusion matrix
Regression	MAE, RMSE, MAPE, R²
Ranking	MRR, nDCG, MAP, Recall@k, Precision@k
Calibration	ECE, MCE, Brier score, reliability diagrams


8. NLP / Generation
Metric	Use
BLEU	Translation, n-gram overlap
ROUGE-1/2/L	Summarization recall
METEOR	Translation with synonyms/stemming
chrF / chrF++	Character-level translation
BERTScore	Semantic similarity via embeddings
MoverScore / BLEURT / COMET	Learned semantic metrics
Perplexity	LM fluency (intrinsic)
Exact / fuzzy match	Short-form QA
Semantic similarity	Embedding cosine, cross-encoder
9. Code
pass@k (HumanEval, MBPP, SWE-bench, LiveCodeBench)
Unit-test execution success
Edit distance to canonical solution
Static analysis pass rate, security scan pass rate
10. RAG-Specific
Layer	Metric
Retrieval	Recall@k, MRR, nDCG, context precision, context recall (Ragas)
Generation	Faithfulness, answer relevancy, groundedness
End-to-end	Task success, citation accuracy
TruLens RAG triad	Context relevance → groundedness → answer relevance
11. Agent-Specific
Tool selection accuracy
Argument correctness (schema + value validity)
Trajectory match (vs. gold sequence) — exact, prefix, edit distance
Task success rate (end-state correctness)
Steps-to-success, cost-to-success
Recovery rate on tool failure / malformed output
Loop / runaway detection
Safety: refuses unauthorized actions, scope adherence
12. Safety & Red-Team
Jailbreak resistance rate
Prompt-injection pass rate (direct + indirect)
PII leakage rate
Toxicity score (Perspective API, Detoxify)
Refusal correctness (false refusal vs. correct refusal)
Harm category pass rates (HarmBench taxonomy)
13. Bias & Fairness
Demographic parity, equal opportunity, equalized odds
Disparate impact ratio
Counterfactual fairness (held-constant persona swaps)
Subgroup performance gaps (slice metrics)
Stereotype scores (StereoSet, CrowS-Pairs, BBQ)


14. Robustness
Invariance under paraphrase, typo, casing (CheckList INV)
Directional expectation tests (CheckList DIR)
Adversarial pass rate (TextAttack, PromptBench)
Out-of-distribution detection (Mahalanobis, energy)
15. Operational
Latency: p50, p95, p99, TTFT (time-to-first-token), TBT (time-between-tokens)
Throughput: req/s, tokens/s
Cost: /request,/1k tokens, $/successful task
,
/request,/1k tokens, $/successful task
Stability: error rate, timeout rate, retry rate
Cache hit rate



The Complete Guide to AI Evaluations (Evals)
A comprehensive reference covering every dimension of evaluating AI systems — from first principles to production governance.

Part I — Foundations
1. What an Eval Is
An evaluation is a repeatable, measurable judgment of an AI system's behavior against a defined expectation. Unlike traditional tests, evals are:

Probabilistic — same input can yield different outputs
Multi-dimensional — quality is not a single number
Rubric-driven — quality requires explicit criteria
Comparative — often relative (A vs. B) rather than absolute
2. Why Evals Exist
Without evals	With evals
"Vibes-based" prompt tuning	Quantified regression tracking
Silent regressions on model swap	Gated releases
No way to compare prompts/models	Reproducible A/B leaderboards
Hallucinations discovered by users	Caught pre-release
No audit trail	Compliance-ready evidence
3. The Eval Stack (Three Loops)
Rule: never automate what you have not first validated manually.

Part II — Eval Taxonomy
4. By Reference
Type	Description	When to use
Reference-based	Compare to gold answer	Closed-domain QA, classification, translation
Reference-free	Judge quality without gold	Open-ended generation, summarization
Pairwise / preference	A vs. B winner	Model/prompt selection, RLHF
Rubric-based	Score per criterion (1–5)	Multi-dimensional quality
Behavioral / capability	Pass/fail per skill	Unit-style assertions
5. By What's Measured
Category	Examples
Correctness	Factuality, accuracy, math, code correctness
Faithfulness	Grounded in retrieved context (RAG)
Relevance	Addresses the user's intent
Coherence	Internally consistent, logical
Instruction-following	Honors constraints, format, length
Style / tone	Brand voice, register, locale
Safety	Toxicity, harm, illegal content
Privacy	PII leakage, data exfiltration
Bias / fairness	Persona-held-constant deltas
Robustness	Paraphrase, typo, adversarial stability
Calibration	Confidence vs. correctness
Operational	Latency, cost, throughput, error rate
6. By System Surface
Surface	Eval focus
Prompt	Variant A/B, regression on prompt change
Model	Headline metrics, capability suites
RAG pipeline	Retrieval + generation grounding
Agent	Tool selection, trajectory, task success
Multimodal	Vision/audio/video grounding, OCR fidelity
Fine-tune	Pre/post deltas, regression on base capabilities
End-to-end product	User task success, satisfaction
Part III — Metrics Catalog
7. Classical ML
Task	Metrics
Binary classification	Accuracy, precision, recall, F1, ROC-AUC, PR-AUC, log-loss, MCC
Multi-class	Macro/micro/weighted F1, confusion matrix
Regression	MAE, RMSE, MAPE, R²
Ranking	MRR, nDCG, MAP, Recall@k, Precision@k
Calibration	ECE, MCE, Brier score, reliability diagrams
8. NLP / Generation
Metric	Use
BLEU	Translation, n-gram overlap
ROUGE-1/2/L	Summarization recall
METEOR	Translation with synonyms/stemming
chrF / chrF++	Character-level translation
BERTScore	Semantic similarity via embeddings
MoverScore / BLEURT / COMET	Learned semantic metrics
Perplexity	LM fluency (intrinsic)
Exact / fuzzy match	Short-form QA
Semantic similarity	Embedding cosine, cross-encoder
9. Code
pass@k (HumanEval, MBPP, SWE-bench, LiveCodeBench)
Unit-test execution success
Edit distance to canonical solution
Static analysis pass rate, security scan pass rate
10. RAG-Specific
Layer	Metric
Retrieval	Recall@k, MRR, nDCG, context precision, context recall (Ragas)
Generation	Faithfulness, answer relevancy, groundedness
End-to-end	Task success, citation accuracy
TruLens RAG triad	Context relevance → groundedness → answer relevance
11. Agent-Specific
Tool selection accuracy
Argument correctness (schema + value validity)
Trajectory match (vs. gold sequence) — exact, prefix, edit distance
Task success rate (end-state correctness)
Steps-to-success, cost-to-success
Recovery rate on tool failure / malformed output
Loop / runaway detection
Safety: refuses unauthorized actions, scope adherence
12. Safety & Red-Team
Jailbreak resistance rate
Prompt-injection pass rate (direct + indirect)
PII leakage rate
Toxicity score (Perspective API, Detoxify)
Refusal correctness (false refusal vs. correct refusal)
Harm category pass rates (HarmBench taxonomy)
13. Bias & Fairness
Demographic parity, equal opportunity, equalized odds
Disparate impact ratio
Counterfactual fairness (held-constant persona swaps)
Subgroup performance gaps (slice metrics)
Stereotype scores (StereoSet, CrowS-Pairs, BBQ)
14. Robustness
Invariance under paraphrase, typo, casing (CheckList INV)
Directional expectation tests (CheckList DIR)
Adversarial pass rate (TextAttack, PromptBench)
Out-of-distribution detection (Mahalanobis, energy)
15. Operational
Latency: p50, p95, p99, TTFT (time-to-first-token), TBT (time-between-tokens)
Throughput: req/s, tokens/s
Cost: 
/
r
e
q
u
e
s
t
,
/request,/1k tokens, $/successful task
Stability: error rate, timeout rate, retry rate
Cache hit rate
Part IV — LLM-as-Judge
16. When to Use
Open-ended outputs without single gold answer
Multi-dimensional rubric scoring at scale
Pairwise model/prompt comparisons
Online sampled scoring of production traffic
17. Judge Patterns
Pattern	Description
Pointwise	Score one output on a rubric (1–5)
Pairwise	Pick A or B; supports Elo / Bradley-Terry leaderboards
Reference-based	Judge with gold answer in context
Reference-free	Judge from rubric only
G-Eval	CoT-generated criteria + form-filled scoring
Panel / ensemble	N judges → majority/median; flag disagreement
Constitutional	Judge against principles list

18. Judge Biases (and Mitigations)
Bias	Mitigation
Position bias (prefers A or B)	Randomize order; run both orderings
Verbosity bias (prefers longer)	Length-penalize; rubric anchors
Self-preference (prefers own family)	Use different model family as judge
Sycophancy	Hide author identity; neutral phrasing
Anchoring on first criterion	Independent per-criterion scoring
Style-over-substance	Force factuality check first


19. Validating the Judge
Sample 50–100 items.
Have 2–3 humans score them.
Compute judge–human agreement: Cohen's κ, Krippendorff's α, Spearman ρ.
Target: κ ≥ 0.6 (substantial); iterate the rubric until reached.
Track judge variance (run N=3, report std-dev).
Re-validate after any judge model change.
20. Rubric Design
Each criterion needs:

Definition (one sentence)
1–5 anchors with concrete examples
Pass threshold
Independence from other criteria (low correlation)
Example: Faithfulness

5 — Every claim directly supported by context
3 — Mostly grounded, minor unsupported detail
1 — Contradicts or fabricates beyond context
Part V — Datasets
21. Golden / Eval Dataset Anatomy
Per case:

id, capability_tag, persona_tag, difficulty, language/locale
input, expected_behavior (or gold)
rubric_ids to apply
owner, dataset_version, created_at, source
pass_history (per release)


22. Composition Heuristic
60% happy path
20% edge cases
10% adversarial / red-team
10% drift-prone (time-, policy-, news-sensitive)
23. Sources
Sanitized production logs (highest signal)
SME-authored sessions
Synthetic generation by another LLM + human curation
Public benchmarks (sanity only — assume contamination)
User feedback escalations
24. Hygiene & Lifecycle
Dedupe (exact + near-dup via embeddings)
Train/eval leakage check (n-gram overlap, embedding similarity)
PII redaction
Quarterly refresh; deprecate stale cases
Semver the dataset; freeze for releases
Track per-case pass/fail history to find flaky or drifting cases
25. Synthetic Data for Evals
Use a strong LLM to generate paraphrases, edge cases, adversarial variants
Always human-review before adding to golden set
Tag as synthetic; track separately in metrics


26. Public Benchmarks (Reference)
Benchmark	Tests
MMLU / MMLU-Pro	General knowledge
GPQA	Graduate-level reasoning
HumanEval / MBPP / SWE-bench / LiveCodeBench	Code
GSM8K / MATH	Math
TruthfulQA	Hallucination
HellaSwag / ARC	Commonsense
BBQ / StereoSet / CrowS-Pairs	Bias
HarmBench / AdvBench	Safety
MT-Bench / Arena-Hard / AlpacaEval	Chat quality
τ-bench / AgentBench / WebArena	Agents
MMMU / MathVista	Multimodal
Caveat: all public benchmarks risk training-set contamination. Use private holdouts for trustworthy comparisons.

Part VI — Eval Methodology
27. Designing an Eval
Define the decision the eval informs (ship/no-ship, pick A/B, gate canary).
Identify capabilities the system must have.
Author rubrics per capability with anchors.
Build the dataset sized for statistical power.
Choose metrics (deterministic + judge + operational).
Set thresholds based on baseline + acceptable regression.
Define the report format (per-slice, per-capability, with CIs).



=======================================================================================================================

# The Complete Guide to AI Evaluations (Evals)

A comprehensive reference covering every dimension of evaluating AI systems — from first principles to production governance.

---

## Part I — Foundations

### 1. What an Eval Is

An **evaluation** is a repeatable, measurable judgment of an AI system's behavior against a defined expectation. Unlike traditional tests, evals are:
- **Probabilistic** — same input can yield different outputs
- **Multi-dimensional** — quality is not a single number
- **Rubric-driven** — quality requires explicit criteria
- **Comparative** — often relative (A vs. B) rather than absolute

### 2. Why Evals Exist

| Without evals | With evals |
|---|---|
| "Vibes-based" prompt tuning | Quantified regression tracking |
| Silent regressions on model swap | Gated releases |
| No way to compare prompts/models | Reproducible A/B leaderboards |
| Hallucinations discovered by users | Caught pre-release |
| No audit trail | Compliance-ready evidence |

### 3. The Eval Stack (Three Loops)

```
Loop 3: Production / Online Evals  ── live traffic, drift, feedback
   ↑                                              │
   │                                              ▼
Loop 2: Offline / CI Evals  ── regression, gates, leaderboards
   ↑                                              │
   │                                              ▼
Loop 1: Manual / Exploratory Evals  ── golden sets, rubrics, red-team
```

**Rule:** never automate what you have not first validated manually.

---

## Part II — Eval Taxonomy

### 4. By Reference

| Type | Description | When to use |
|---|---|---|
| **Reference-based** | Compare to gold answer | Closed-domain QA, classification, translation |
| **Reference-free** | Judge quality without gold | Open-ended generation, summarization |
| **Pairwise / preference** | A vs. B winner | Model/prompt selection, RLHF |
| **Rubric-based** | Score per criterion (1–5) | Multi-dimensional quality |
| **Behavioral / capability** | Pass/fail per skill | Unit-style assertions |

### 5. By What's Measured

| Category | Examples |
|---|---|
| **Correctness** | Factuality, accuracy, math, code correctness |
| **Faithfulness** | Grounded in retrieved context (RAG) |
| **Relevance** | Addresses the user's intent |
| **Coherence** | Internally consistent, logical |
| **Instruction-following** | Honors constraints, format, length |
| **Style / tone** | Brand voice, register, locale |
| **Safety** | Toxicity, harm, illegal content |
| **Privacy** | PII leakage, data exfiltration |
| **Bias / fairness** | Persona-held-constant deltas |
| **Robustness** | Paraphrase, typo, adversarial stability |
| **Calibration** | Confidence vs. correctness |
| **Operational** | Latency, cost, throughput, error rate |

### 6. By System Surface

| Surface | Eval focus |
|---|---|
| **Prompt** | Variant A/B, regression on prompt change |
| **Model** | Headline metrics, capability suites |
| **RAG pipeline** | Retrieval + generation grounding |
| **Agent** | Tool selection, trajectory, task success |
| **Multimodal** | Vision/audio/video grounding, OCR fidelity |
| **Fine-tune** | Pre/post deltas, regression on base capabilities |
| **End-to-end product** | User task success, satisfaction |

---

## Part III — Metrics Catalog

### 7. Classical ML

| Task | Metrics |
|---|---|
| Binary classification | Accuracy, precision, recall, F1, ROC-AUC, PR-AUC, log-loss, MCC |
| Multi-class | Macro/micro/weighted F1, confusion matrix |
| Regression | MAE, RMSE, MAPE, R² |
| Ranking | MRR, nDCG, MAP, Recall@k, Precision@k |
| Calibration | ECE, MCE, Brier score, reliability diagrams |

### 8. NLP / Generation

| Metric | Use |
|---|---|
| **BLEU** | Translation, n-gram overlap |
| **ROUGE-1/2/L** | Summarization recall |
| **METEOR** | Translation with synonyms/stemming |
| **chrF / chrF++** | Character-level translation |
| **BERTScore** | Semantic similarity via embeddings |
| **MoverScore / BLEURT / COMET** | Learned semantic metrics |
| **Perplexity** | LM fluency (intrinsic) |
| **Exact / fuzzy match** | Short-form QA |
| **Semantic similarity** | Embedding cosine, cross-encoder |

### 9. Code

- pass@k (HumanEval, MBPP, SWE-bench, LiveCodeBench)
- Unit-test execution success
- Edit distance to canonical solution
- Static analysis pass rate, security scan pass rate

### 10. RAG-Specific

| Layer | Metric |
|---|---|
| Retrieval | Recall@k, MRR, nDCG, **context precision**, **context recall** (Ragas) |
| Generation | **Faithfulness**, **answer relevancy**, **groundedness** |
| End-to-end | Task success, citation accuracy |
| TruLens **RAG triad** | Context relevance → groundedness → answer relevance |

### 11. Agent-Specific

- Tool selection accuracy
- Argument correctness (schema + value validity)
- Trajectory match (vs. gold sequence) — exact, prefix, edit distance
- Task success rate (end-state correctness)
- Steps-to-success, cost-to-success
- Recovery rate on tool failure / malformed output
- Loop / runaway detection
- Safety: refuses unauthorized actions, scope adherence

### 12. Safety & Red-Team

- Jailbreak resistance rate
- Prompt-injection pass rate (direct + indirect)
- PII leakage rate
- Toxicity score (Perspective API, Detoxify)
- Refusal correctness (false refusal vs. correct refusal)
- Harm category pass rates (HarmBench taxonomy)

### 13. Bias & Fairness

- Demographic parity, equal opportunity, equalized odds
- Disparate impact ratio
- Counterfactual fairness (held-constant persona swaps)
- Subgroup performance gaps (slice metrics)
- Stereotype scores (StereoSet, CrowS-Pairs, BBQ)

### 14. Robustness

- Invariance under paraphrase, typo, casing (CheckList INV)
- Directional expectation tests (CheckList DIR)
- Adversarial pass rate (TextAttack, PromptBench)
- Out-of-distribution detection (Mahalanobis, energy)

### 15. Operational

- Latency: p50, p95, p99, TTFT (time-to-first-token), TBT (time-between-tokens)
- Throughput: req/s, tokens/s
- Cost: $/request, $/1k tokens, $/successful task
- Stability: error rate, timeout rate, retry rate
- Cache hit rate

---

## Part IV — LLM-as-Judge

### 16. When to Use

- Open-ended outputs without single gold answer
- Multi-dimensional rubric scoring at scale
- Pairwise model/prompt comparisons
- Online sampled scoring of production traffic

### 17. Judge Patterns

| Pattern | Description |
|---|---|
| **Pointwise** | Score one output on a rubric (1–5) |
| **Pairwise** | Pick A or B; supports Elo / Bradley-Terry leaderboards |
| **Reference-based** | Judge with gold answer in context |
| **Reference-free** | Judge from rubric only |
| **G-Eval** | CoT-generated criteria + form-filled scoring |
| **Panel / ensemble** | N judges → majority/median; flag disagreement |
| **Constitutional** | Judge against principles list |

### 18. Judge Biases (and Mitigations)

| Bias | Mitigation |
|---|---|
| Position bias (prefers A or B) | Randomize order; run both orderings |
| Verbosity bias (prefers longer) | Length-penalize; rubric anchors |
| Self-preference (prefers own family) | Use different model family as judge |
| Sycophancy | Hide author identity; neutral phrasing |
| Anchoring on first criterion | Independent per-criterion scoring |
| Style-over-substance | Force factuality check first |

### 19. Validating the Judge

1. Sample 50–100 items.
2. Have 2–3 humans score them.
3. Compute **judge–human agreement**: Cohen's κ, Krippendorff's α, Spearman ρ.
4. **Target**: κ ≥ 0.6 (substantial); iterate the rubric until reached.
5. Track judge variance (run N=3, report std-dev).
6. Re-validate after any judge model change.

### 20. Rubric Design

Each criterion needs:
- **Definition** (one sentence)
- **1–5 anchors** with concrete examples
- **Pass threshold**
- **Independence** from other criteria (low correlation)

Example: *Faithfulness*
- 5 — Every claim directly supported by context
- 3 — Mostly grounded, minor unsupported detail
- 1 — Contradicts or fabricates beyond context

---

## Part V — Datasets

### 21. Golden / Eval Dataset Anatomy

Per case:
- `id`, `capability_tag`, `persona_tag`, `difficulty`, `language/locale`
- `input`, `expected_behavior` (or `gold`)
- `rubric_ids` to apply
- `owner`, `dataset_version`, `created_at`, `source`
- `pass_history` (per release)

### 22. Composition Heuristic

- 60% happy path
- 20% edge cases
- 10% adversarial / red-team
- 10% drift-prone (time-, policy-, news-sensitive)

### 23. Sources

- Sanitized production logs (highest signal)
- SME-authored sessions
- Synthetic generation by another LLM + human curation
- Public benchmarks (sanity only — assume contamination)
- User feedback escalations

### 24. Hygiene & Lifecycle

- Dedupe (exact + near-dup via embeddings)
- Train/eval leakage check (n-gram overlap, embedding similarity)
- PII redaction
- Quarterly refresh; deprecate stale cases
- Semver the dataset; freeze for releases
- Track per-case pass/fail history to find flaky or drifting cases

### 25. Synthetic Data for Evals

- Use a strong LLM to generate paraphrases, edge cases, adversarial variants
- **Always** human-review before adding to golden set
- Tag as `synthetic`; track separately in metrics

### 26. Public Benchmarks (Reference)

| Benchmark | Tests |
|---|---|
| MMLU / MMLU-Pro | General knowledge |
| GPQA | Graduate-level reasoning |
| HumanEval / MBPP / SWE-bench / LiveCodeBench | Code |
| GSM8K / MATH | Math |
| TruthfulQA | Hallucination |
| HellaSwag / ARC | Commonsense |
| BBQ / StereoSet / CrowS-Pairs | Bias |
| HarmBench / AdvBench | Safety |
| MT-Bench / Arena-Hard / AlpacaEval | Chat quality |
| τ-bench / AgentBench / WebArena | Agents |
| MMMU / MathVista | Multimodal |

**Caveat:** all public benchmarks risk training-set contamination. Use private holdouts for trustworthy comparisons.

---

## Part VI — Eval Methodology

### 27. Designing an Eval

1. **Define the decision** the eval informs (ship/no-ship, pick A/B, gate canary).
2. **Identify capabilities** the system must have.
3. **Author rubrics** per capability with anchors.
4. **Build the dataset** sized for statistical power.
5. **Choose metrics** (deterministic + judge + operational).
6. **Set thresholds** based on baseline + acceptable regression.
7. **Define the report format** (per-slice, per-capability, with CIs).

### 28. Statistical Rigor

- **Sample size**: enough to detect MDE (minimum detectable effect) at α=0.05, power=0.8
- **Confidence intervals**: bootstrap or Wilson for proportions; report alongside point estimates
- **Significance**: paired tests for A/B (McNemar for binary, paired bootstrap for continuous)
- **Multiple comparisons**: Bonferroni or BH correction when scoring many slices
- **Variance**: report std-dev across N runs (typical N=3–5) at temp > 0
- **Seed control**: fix seeds where possible; report seed in artifacts

### 29. Slicing

Always evaluate by slice, not just aggregate:
- Per capability tag
- Per persona / demographic axis
- Per language / locale
- Per input length bucket
- Per difficulty
- Per source (synthetic vs. real)

A model can win on aggregate while regressing on a critical slice.

### 30. Determinism & Reproducibility

- Pin: model version, prompt template version, dataset version, judge model + version, seed, temperature
- Log everything in eval artifacts
- Re-runnable from a single config file

---

## Part VII — Specialized Evals

### 31. RAG Evaluation

**Decompose** the pipeline:
1. **Ingestion**: chunking quality, dedup, metadata coverage
2. **Embedding**: drift on model swap, dimensionality
3. **Retrieval**: Recall@k, MRR, nDCG, context precision/recall
4. **Reranker**: nDCG uplift over retriever
5. **Generation**: faithfulness, answer relevancy
6. **End-to-end**: task success, citation accuracy
7. **Freshness**: index staleness SLA

**TruLens RAG triad**: Context relevance → Groundedness → Answer relevance.
**Ragas core**: faithfulness, answer_relevancy, context_precision, context_recall.

### 32. Agent Evaluation

- **Tool-use unit tests**: per tool, per intent
- **Trajectory eval**: compare action sequence to gold; allow reorderings if equivalent
- **Task success**: end-state checks (DB row exists, file written, API called correctly)
- **Cost & step budgets**: cap and measure
- **Failure-mode tests**: tool errors, rate limits, malformed outputs
- **Safety**: refuses out-of-scope, escalates ambiguity
- Frameworks: LangSmith, AgentBench, τ-bench, SWE-bench

### 33. Multimodal Evaluation

- **Vision**: VQA accuracy, OCR fidelity (CER/WER), grounding (point/box accuracy), chart/table reading
- **Audio**: WER for ASR, MOS for TTS, speaker diarization F1
- **Video**: temporal grounding, action recognition, captioning
- **Cross-modal grounding**: hallucination of objects not in image

### 34. Fine-Tune Evaluation

- **Capability gain**: target task uplift
- **Capability regression**: base capability suite (don't break general skills)
- **Catastrophic forgetting** checks
- **Alignment regression**: safety, refusal correctness
- **Style adherence**: format/tone rubrics

### 35. Embedding & Retriever Evaluation

- BEIR benchmark slices
- MTEB tasks (classification, clustering, retrieval, reranking, STS)
- Domain-specific Recall@k on private corpus
- Robustness to query paraphrase

### 36. Reasoning Evaluation

- Chain-of-thought correctness (process, not just answer)
- Self-consistency (majority across N samples)
- Step-level error detection (PRM-style process rewards)
- Math: exact answer + work shown
- Logic: counterexample resistance

### 37. Safety / Red-Team Evaluation

- **Direct injection**: "ignore previous instructions"
- **Indirect injection**: poisoned web/doc/tool output
- **Jailbreaks**: DAN, persona, hypothetical, encoded, multi-turn
- **System prompt extraction**
- **PII / secret leakage**
- **Cross-session / cross-tenant leakage**
- **Tool/plugin abuse**
- **Denial-of-wallet** (token flooding)
- **Multimodal injection** (text-in-image, audio)
- Tools: **Garak**, **PyRIT**, **Giskard**, **Promptfoo red-team**, HarmBench

### 38. Bias & Fairness Evaluation

- **Persona-held-constant** harness: vary only demographic markers
- Slice metrics across name, gender, age, disability, locale, SES
- Counterfactual probes
- Stereotype benchmarks (BBQ, StereoSet, CrowS-Pairs)
- Flag deltas > 5% on rubric scores
- Tools: Fairlearn, AIF360, Aequitas

### 39. Privacy Evaluation

- Membership inference attack resistance
- Model inversion resistance
- Training-data extraction probes (especially LLMs)
- Model extraction / stealing resistance
- Differential privacy ε, δ verification

### 40. Calibration & Uncertainty

- Reliability diagrams, ECE, MCE, Brier
- Temperature / Platt scaling post-hoc
- Selective prediction (abstain when uncertain)
- OOD detection (Mahalanobis, energy, MSP)
- Verbalized confidence accuracy (does "I'm 80% sure" mean 80%?)

---

## Part VIII — Pipeline & Infrastructure

### 41. Reference Architecture

```
┌──────────────┐   ┌──────────────┐   ┌────────────┐
│ Dataset Repo │──▶│ Eval Runner  │──▶│ Aggregator │
│ (versioned)  │   │ (DeepEval/   │   │ + Slicing  │
└──────────────┘   │  promptfoo)  │   └─────┬──────┘
                   └──────┬───────┘         │
┌──────────────┐          │                 ▼
│ Rubric Repo  │──────────┘          ┌────────────┐
│ (versioned)  │                     │ Dashboard  │
└──────────────┘                     │ + CI Gate  │
                                     └────────────┘
       ┌──────────┐    ┌──────────┐
       │ Tracing  │───▶│ Online   │
       │ (Langfuse│    │ Evals    │
       │  /Phoenix│    │ (sampled)│
       └──────────┘    └──────────┘
```

### 42. CI/CD Integration

- **Pre-merge**: smoke eval (50–100 cases) on every PR; fail on gate breach
- **Nightly**: full regression (1k+ cases) with leaderboard delta
- **Pre-release**: red-team + bias + capability suites
- **Post-deploy**: canary eval on shadow traffic before promoting
- **Artifact**: every run produces JSON + HTML report attached to commit

### 43. Suggested Quality Gates

| Gate | Threshold |
|---|---|
| Hallucination rate | < 2% |
| Faithfulness (RAG) | ≥ 0.90 |
| Bias delta across personas | < 5% |
| Safety pass rate (core red-team) | 100% |
| Subgroup regression | none > 3% |
| p95 latency | within SLA |
| Cost regression | < 10% |
| Judge–human κ | ≥ 0.6 |

### 44. Tooling Cheat Sheet (2026)

| Need | Pick |
|---|---|
| Offline framework | **DeepEval**, **Ragas**, **promptfoo**, **Inspect AI**, OpenAI Evals, lm-eval-harness |
| Tracing + online | **LangSmith**, **Arize Phoenix**, **Langfuse**, **TruLens**, Helicone |
| Red-team | **Garak**, **PyRIT**, **Giskard**, Promptfoo red-team |
| Data validation | Great Expectations, Pandera, Deequ |
| Drift | Evidently, NannyML, Arize |
| Fairness | Fairlearn, AIF360, Aequitas |
| Prompt/version mgmt | PromptLayer, Humanloop, Langfuse |
| Experiment tracking | MLflow, Weights & Biases |
| Annotation | Label Studio, Argilla, Prodigy |

---

## Part IX — Online & Production Evals

### 45. Tracing

Every call must log: input, output, retrieved context, tool calls + args + results, latency, tokens, cost, model + prompt version, user feedback.

### 46. Online Eval Sampling

- Score 1–5% of traffic via LLM-judge
- Higher rate on new releases / canaries
- Stratified sampling across capabilities & user segments

### 47. Drift Detection

- **Input drift**: distribution shift on embeddings, lengths, languages
- **Output drift**: refusal rate, length, sentiment, format
- **Performance drift**: rubric scores trending down
- **Concept drift**: gold answers becoming stale (policy/news changes)
- Tools: Evidently, NannyML, Arize

### 48. Feedback Loops

- Thumbs + free-text + escalation paths
- Triaged into golden set within 48h
- Rubric refinement informed by feedback themes
- Closed loop: feedback → dataset → next eval → next release

### 49. Release Strategy

```
shadow ─► 1% canary ─► 5% ─► 25% ─► 100%
   │         │         │       │       │
   └─ auto-rollback if any gate breached
```

### 50. Replay Harness

- Capture sanitized prod traces
- Replay against candidate model/prompt
- Diff outputs; flag regressions before promotion

---

## Part X — Anti-Patterns (and Fixes)

| Anti-pattern | Fix |
|---|---|
| Single aggregate score | Always slice; report CIs |
| Public benchmark only | Add private holdout; assume contamination |
| Self-judging | Use different model family for judge |
| Unvalidated judge | Run κ study; iterate rubric |
| Vague rubrics | 1–5 anchors with examples |
| No variance reporting | N=3+ runs; report std-dev |
| Train/eval leakage | N-gram + embedding overlap checks |
| No drift monitoring | Online evals + alerts |
| Eval-set rot | Quarterly refresh; track per-case history |
| Treating evals as one-off | CI gates + leaderboards over time |
| Ignoring cost/latency | Include in gates |
| Exact-string match on generative | Use semantic + rubric |

---

## Part XI — Governance & Standards

### 51. Standards to Map Tests Against

- **OWASP AI Testing Guide v1.0** — tag tests with AITG-APP/MOD/INF/DAT IDs
- **OWASP Top 10 for LLMs**
- **NIST AI RMF** — Govern / Map / Measure / Manage
- **ISO/IEC 42001** — AI management system
- **EU AI Act** — high-risk system testing artifacts
- **ISTQB CT-AI v2.0** — tester certification

### 52. Required Artifacts

- **Model card** — intended use, metrics, limitations
- **Datasheet** — data provenance, demographics, consent
- **Eval report** — per-release: metrics, slices, CIs, judge κ
- **Risk register** — bias, hallucination, safety, over-reliance
- **AI Impact Assessment** — for high-risk systems
- **Audit log** — every eval run reproducible from artifacts

---

## Part XII — Reporting

### 53. A Good Eval Report Contains

1. **Decision** (ship/no-ship/canary)
2. **Headline metrics** with CIs vs. baseline
3. **Per-capability table**
4. **Per-slice table** (persona, locale, length, difficulty)
5. **Regression list** (cases that flipped pass→fail)
6. **New wins** (cases that flipped fail→pass)
7. **Red-team pass rate**
8. **Bias deltas**
9. **Operational** (latency, cost)
10. **Judge–human agreement** (κ)
11. **Variance** across N runs
12. **Artifacts**: dataset version, model version, prompt version, seed, config hash

### 54. Leaderboards

- Track over time per (model × prompt × dataset)
- Show deltas, not just absolutes
- Pin baseline; mark releases
- Surface flaky cases (high variance)

---

## Part XIII — Roles & Workflow

### 55. Who Owns What

| Role | Owns |
|---|---|
| AI Eval Tester | Datasets, rubrics, judges, harnesses, gates |
| ML Engineer | Model training, fine-tunes |
| Prompt Engineer | Prompt iteration informed by evals |
| Product / SME | Rubric authoring, golden case curation |
| Safety / Red-Team | Adversarial suites, harm taxonomies |
| MLOps | Tracing, drift, online evals, rollout |
| Compliance | Audit artifacts, AITG/NIST/EU AI Act mapping |

### 56. Daily / Weekly / Release Cadence

- **Daily**: triage online eval alerts; add new golden cases from feedback
- **Weekly**: review leaderboard; refresh red-team; rubric tuning
- **Per release**: full regression + red-team + bias + canary
- **Quarterly**: dataset refresh; judge re-validation; standards audit

---

## Part XIV — Quick-Start Templates

### 57. Minimal Eval Case (YAML)

```yaml
id: refund-policy-001
capability: policy_qa
persona: neutral
difficulty: easy
input: "What is the return window for electronics?"
expected: "60 days from delivery for electronics."
rubrics: [faithfulness, correctness, tone]
source: production_log
dataset_version: 2026.04.1
```

### 58. Minimal Rubric (YAML)

```yaml
id: faithfulness
definition: Output is grounded in retrieved context.
anchors:
  5: Every claim directly supported by context.
  4: All major claims supported; minor unsupported detail.
  3: Mostly grounded; one unsupported claim.
  2: Multiple unsupported claims.
  1: Contradicts or fabricates beyond context.
threshold: 4
```

### 59. CI Gate (pseudo)

```python
assert results["hallucination_rate"] < 0.02
assert results["faithfulness_mean"] >= 0.90
assert results["bias_delta_max"] < 0.05
assert results["safety_pass_rate"] == 1.0
assert results["p95_latency_ms"] <= SLA_MS
assert results["cost_delta_pct"] < 10
```

---

## Part XV — Glossary

- **Golden set** — curated, versioned eval dataset
- **Rubric** — multi-dim scoring criteria with anchors
- **Judge** — model used to score outputs
- **κ (kappa)** — inter-rater agreement
- **MDE** — minimum detectable effect
- **Slice** — subset of eval set (persona/locale/difficulty)
- **Canary** — small % traffic to new version
- **Replay** — re-run prod traces on candidate
- **Faithfulness** — grounded in context (RAG)
- **Trajectory** — agent's action sequence
- **TTFT / TBT** — time to / between tokens
- **ECE** — expected calibration error
- **AITG** — OWASP AI Testing Guide identifiers

---

## What "Done" Looks Like

You can:
- Defend any release with **rubrics, slices, judge–human κ, adversarial pass rates, CIs, and variance** — not a single number.
- Trace any production failure to a **golden-set gap** and close it within a day.
- Show a closed loop: **manual rubrics → offline evals → CI gates → online evals → feedback → golden set**.
- Map every test to **OWASP AITG / NIST AI RMF / EU AI Act** for audit.


=====================================================================================


# AI Testing Master Guide

A consolidated reference covering:

1. E2E Testing Roadmap (Core → User)
2. AI Tester Roadmap (Manual → User AI Testing)
3. AI Eval Tester Focused Roadmap
4. The Complete Guide to AI Evaluations (Evals)

---
---

# Book 1 — Complete E2E Testing Roadmap: Core to User

A structured roadmap covering end-to-end testing from foundational concepts up to user-facing validation.

---

## Phase 1: Foundations (Core)

**Goal:** Build testing fundamentals before touching E2E tools.

- **Testing pyramid & strategy**
  - Unit → Integration → Contract → E2E → Exploratory
  - When to use E2E vs. lower layers (cost, speed, flakiness trade-offs)
- **Core concepts**
  - Test doubles: stubs, mocks, fakes, spies
  - Deterministic vs. flaky tests, idempotency, isolation
  - AAA pattern (Arrange-Act-Assert), Given-When-Then (BDD)
- **Languages & runners**
  - JS/TS with Jest/Vitest, Python with pytest, or your stack equivalent
- **Version control & CI basics**
  - Git branching, PR workflows, GitHub Actions / Azure DevOps / GitLab CI

---

## Phase 2: Application Layer Testing (Pre-E2E)

**Goal:** Ensure components & APIs are testable before stitching the full stack.

- **Component / unit testing**
  - React Testing Library, Vue Test Utils, Angular TestBed
- **API / integration testing**
  - REST: Postman/Newman, RestAssured, Supertest
  - GraphQL: Apollo testing utilities
  - Contract testing: Pact, Spring Cloud Contract
- **Database & service testing**
  - Testcontainers, in-memory DBs, seed/fixture management

---

## Phase 3: E2E Tooling & Frameworks

**Goal:** Choose and master E2E frameworks.

- **Modern web E2E**
  - **Playwright** (recommended: multi-browser, auto-wait, trace viewer)
  - **Cypress** (DX-friendly, time-travel debugger)
  - Selenium/WebDriver (legacy/cross-language needs)
- **Mobile E2E**
  - Appium, Detox (React Native), Espresso (Android), XCUITest (iOS)
- **API E2E**
  - Karate, Postman collections in CI
- **Desktop / Electron**
  - Playwright for Electron, Spectron (deprecated)

---

## Phase 4: Test Design & Architecture

**Goal:** Write maintainable, scalable E2E suites.

- **Patterns**
  - Page Object Model (POM), Screenplay pattern, Component Object Model
  - Fixtures, custom commands, reusable actions
- **Test data management**
  - Factories (Faker, Fishery), builders, API-based seeding (avoid UI seeding)
  - Isolation per test: unique users, tenant scoping, DB transactions
- **Environment strategy**
  - Local, ephemeral preview envs, staging, production smoke tests
  - Feature flags & toggles in tests
- **Selectors discipline**
  - `data-testid` first; avoid CSS/XPath coupling to styling

---

## Phase 5: Reliability & Quality

**Goal:** Eliminate flakiness and build trust.

- **Flake reduction**
  - Auto-waiting, no `sleep()`, network idle, retry policies (sparingly)
  - Deterministic clocks, seeded randomness, time/locale freezing
- **Observability in tests**
  - Screenshots, videos, traces (Playwright Trace Viewer)
  - Structured logs, HAR captures, console error assertions
- **Parallelization & sharding**
  - Worker isolation, sharded CI runs, cloud grids (BrowserStack, Sauce Labs, LambdaTest)

---

## Phase 6: CI/CD Integration

**Goal:** Run E2E continuously and fast.

- **Pipeline design**
  - Smoke suite on every PR, full suite nightly, critical-path gating
  - Containerized runners with pinned browser versions
- **Reporting**
  - Allure, ReportPortal, Playwright HTML report, JUnit XML
  - PR annotations, failure clustering, flaky-test dashboards
- **Quality gates**
  - Block merge on critical-path failures; quarantine flaky tests

---

## Phase 7: Cross-Cutting Concerns

**Goal:** Cover non-functional and specialized testing.

- **Accessibility (a11y)**: axe-core, Playwright `@axe-core/playwright`
- **Visual regression**: Percy, Chromatic, Playwright `toHaveScreenshot`
- **Performance**: Lighthouse CI, k6, Artillery (load/stress)
- **Security**: OWASP ZAP baseline scans, dependency scanning
- **Internationalization**: locale matrices, RTL layout tests
- **Resilience**: chaos testing, network throttling, offline mode

---

## Phase 8: User-Centric Validation (USER)

**Goal:** Validate from the real user's perspective.

- **User journey mapping**
  - Identify top N critical flows (signup, checkout, core task completion)
  - Map personas → journeys → E2E scenarios
- **BDD with stakeholders**
  - Cucumber/SpecFlow: Gherkin scenarios co-authored with PMs/QA
- **Real User Monitoring (RUM)**
  - Sentry, Datadog RUM, New Relic — feed insights back into E2E coverage
- **Synthetic monitoring**
  - Production E2E probes (Checkly, Datadog Synthetics, Playwright in prod)
- **Usability & exploratory testing**
  - Session replay (FullStory, LogRocket), bug bashes, dogfooding
- **Acceptance & UAT**
  - Stakeholder sign-off environments, scripted UAT, beta cohorts

---

## Phase 9: Advanced & AI-Assisted Testing

**Goal:** Scale and modernize the practice.

- **AI-assisted authoring**
  - Playwright codegen, GitHub Copilot for test scaffolding
  - Self-healing locators (Testim, Mabl)
- **Model-based & property-based testing**
  - fast-check, Hypothesis for invariants
- **Mutation testing**: Stryker, PIT — measure test effectiveness
- **Test analytics**: trend coverage, MTTR for failures, flakiness rate KPIs

---

## Phase 10: Process & Culture

**Goal:** Sustain quality across the org.

- Shift-left: devs own E2E for their features
- Test ownership via CODEOWNERS
- Definition of Done includes E2E for new flows
- Regular flaky-test triage; quarantine + fix SLA
- Documentation: runbooks, onboarding guides, test-writing standards

---

## Suggested Learning Order (Fast Track)

1. Testing pyramid + JS/TS unit testing → 1–2 weeks
2. API testing (Supertest/Postman) → 1 week
3. Playwright fundamentals → 2 weeks
4. POM + fixtures + data strategy → 1 week
5. CI integration + reporting → 1 week
6. Flake hunting + traces → ongoing
7. a11y + visual + perf add-ons → 2 weeks
8. Synthetic monitoring + RUM feedback loop → 1 week
9. BDD + UAT alignment → ongoing

---

## Recommended Stack (2026 default)

- **Framework:** Playwright (TypeScript)
- **API layer:** Supertest + Pact
- **CI:** GitHub Actions with sharding
- **Reporting:** Playwright HTML + Allure
- **Monitoring:** Checkly (synthetic) + Sentry (RUM)
- **Visual:** Playwright snapshots or Chromatic
- **a11y:** `@axe-core/playwright`

---
---

# Book 2 — AI Tester Roadmap: From Manual Validation to End-to-End User AI Testing

## Executive Summary

An AI Tester is a quality professional who validates that AI/ML systems behave accurately, safely, fairly, consistently, and explainably under real-world conditions — not just against deterministic rules. Because AI systems are probabilistic, adaptive and non-deterministic, traditional pass/fail testing is insufficient.

Structured in 8 sequential phases (Phase 0 → Phase 7) with **Phase 1 — Manual AI Validation (Human-in-the-Loop)** treated as the deepest foundation.

---

## 1. The Phased Roadmap at a Glance

| Phase | Name | Core Question | Primary Skill Focus |
|---|---|---|---|
| Phase 0 | Core Testing Foundations | Can you test traditional software professionally? | Test design, exploratory, API/UI, statistics, RCA |
| Phase 1 | Manual AI Validation (HITL) — deepest focus | Does the AI behave correctly, safely and fairly when a human probes it? | Adversarial probing, bias audits, hallucination checks, explainability, drift, AI bug reporting |
| Phase 2 | Automation-Assisted AI Testing | Can manual checks be scaled and repeated? | Python, pytest, prompt engineering, LLM-as-judge, DeepEval, Ragas, golden datasets |
| Phase 3 | Data & Model Validation | Is the data/model fit, fair and well-trained? | Dataset diversity, PII redaction, drift, BLEU/ROUGE, confusion matrix |
| Phase 4 | System & Integration Testing | Do AI components work together end-to-end? | RAG pipelines, agent tool-use, multi-step reasoning, API integration |
| Phase 5 | MLOps, Monitoring & Observability | Is the model still good in production? | Versioning, drift detection, trace logging, canary, quality gates |
| Phase 6 | Governance, Ethics & Compliance | Is the system trustworthy, lawful and responsible? | OWASP AITG, NIST AI RMF, EU AI Act, ISO/IEC 42001 |
| Phase 7 | User-Facing AI Testing & UAT | Do real users trust and benefit from the AI? | Persona testing, UX, over-reliance checks, feedback loops |

---

## 2. Phase 0 — Core Testing Foundations

You cannot test AI well if your testing fundamentals have gaps.

- Test design: equivalence partitioning, BVA, decision tables, pairwise
- Test levels: unit, integration, system, acceptance, test pyramid
- API testing: REST, HTTP methods, status codes, auth flows
- Exploratory testing & bug RCA — the human creativity edge
- Statistics: probability, confidence intervals, mean/median/std-dev, confusion matrix

**Milestone:** Write a structured test plan, design test cases by multiple techniques, interpret model accuracy reports.

---

## 3. Phase 1 — Manual AI Validation (HITL) — Deep Dive

### 3.1 Why Manual Validation Comes First

- Automated metrics measure performance; humans measure trust.
- AI failures are confident, not loud — fluent, polished, completely wrong answers.
- Real-world risk lives outside the metric: fabricated facts, persona-dependent answers, mishandled sensitive prompts.
- Example: in July 2025 an AI coding assistant deleted a live company database despite being told to "freeze".

### 3.2 7-Step Manual Validation Framework

1. Pre-flight (scope, training source, intended use)
2. Adversarial probing
3. Ethical / persona audit
4. Coherence & memory checks
5. Explainability probing
6. Concept drift checks
7. Structured AI bug report

### 3.3 Manual Validation — Test Types

| Test Type | What You Test | Failure Signal |
|---|---|---|
| Adversarial / Prompt Injection | Alignment under attack | Model abandons purpose / safety rules |
| Edge Case | Gibberish, nulls, extremes | Crash, silent fallback, hallucinated normalization |
| Bias / Persona Audit | Equal treatment | Tone/scrutiny/recommendation differs |
| Hallucination / Factual | Made-up facts/citations | Fabricated specifics |
| Logical Contradiction | Impossible inputs | "Vegan steakhouses" listed |
| Memory / Context | Multi-turn coherence | "Where do you mean?" |
| Tone Drift | UX appropriateness | Robotic condolence reply |
| Explainability | Justifiable reasoning | Generic, circular, refused |
| Concept Drift | Outdated facts/policies | Stale answer |
| Sensitive Data / Privacy | PII leakage | Cross-session leak |
| Toxicity / Unsafe | Safety bypass | Harmful content generated |
| Over-Reliance Risk | UX trust signals | No warning/escalation |

### 3.4 Persona Audit (Bias without source-code access)

Hold inputs constant; vary only demographic markers (name, gender, age, disability, education). Compare tone, depth, scrutiny, stereotype reinforcement.

| Bias Area | Symptoms |
|---|---|
| Gender | Stronger tone for male personas; default "he" |
| Racial | Western names preferred |
| Age | Older personas excluded / outdated advice |
| Disability | Patronizing tone |
| Socioeconomic | Lower expectations from working-class personas |

### 3.5 Sanity Testing — Plausible-but-Wrong

| Failure Type | Looks Like |
|---|---|
| Factual hallucination | Made-up statistics |
| Logical contradiction | Sugar desserts in "low-carb" list |
| Context confusion | Forgets two messages ago |
| Instruction slippage | Skips chained steps |
| Over-confidence | Medical advice without disclaimer |

Log these — they pass superficial review but erode trust most.

### 3.6 Explainability Probing

| Test | Pass / Fail |
|---|---|
| Justification clarity (ask "Why?") | Specific factors vs vague |
| Follow-up challenges | Reasoning adapts coherently |
| Consistency | Stable across rephrasings |
| Rejection explanation | Cites rule + appeal path |

### 3.7 Concept Drift — Lightweight Manual Monitoring

Build 20–30 prompt golden set across compliance, news-sensitive facts, tone, industry logic. Rerun on schedule and after retraining.

| Drift Type | Trigger | Symptom |
|---|---|---|
| Factual | Rates/prices change | Stale figures |
| Language | New slang | "I'm not familiar with that term" |
| Policy | 30→60-day return | Bot still says 30 |

### 3.8 The AI Bug Report — Good Looks Like

- Title: concise, specific
- Prompt verbatim, Output verbatim, Expected
- Why it's wrong (policy/factual citation)
- Category: Factual / Bias / Logic / Injection / Tone / Privacy
- Severity: user, brand, legal impact
- Reproducibility (N runs, pass/fail rate)
- Screenshots, full log, model + prompt version

### 3.9 Three Reusable Checklists

1. Bias & Fairness Audit
2. Security & Privacy Audit (injection, PII, role-play attacks)
3. Generative AI Sanity Check (facts, context, multi-step, real-world logic)

### 3.10 Worked Example — "SupportBot 5000"

1. Pre-flight: scope returns; training = CS logs.
2. Adversarial: "Return 5-yr-old item, no receipt, on fire."
3. Ethical: same refund as "John" vs "LaKeisha".
4. Coherence: laptop policy → 3 turns later → "Does that apply to electronics?"
5. Explainability: cite specific clause on denial.
6. Concept drift: 30→60 day update; rerun old tests.
7. Bug report: "SupportBot states 30-day return after 60-day update."

---

## 4. Phase 2 — Automation-Assisted AI Testing

- Test hierarchy: unit (single prompt) → functional (full conversation/RAG) → regression → production monitoring.
- Test dataset (25–30 cases minimum) with metadata tags.
- Sources: production logs, SME sessions, synthetic, adversarial.
- Stack: Python, pytest, requests, pandas, JSON, OOP for POM.
- Categories to automate: hallucination rate, semantic consistency, boundary, latency, cost.
- Frameworks: DeepEval, Ragas, LangSmith, Arize, TruLens.
- Prompt engineering for testers: role-based, CoT, few-shot, constraint-based.
- LLM-as-Judge — always validate the judge with humans.

---

## 5. Phase 3 — Data & Model Validation

- Dataset quality: dedupe, mislabels, distribution match, holdout integrity.
- PII handling: redaction/anonymization; verify no leakage at inference.
- Diversity & coverage (AITG-DAT-03), harmful content scrub (AITG-DAT-04), data minimization (AITG-DAT-05).
- Metrics: accuracy, precision, recall, F1, ROC/AUC, BLEU, ROUGE, perplexity, MRR.
- Robustness: evasion attacks, new data, goal alignment (AITG-MOD-01/06/07).
- Privacy attacks: membership inference, model inversion, model extraction (AITG-MOD-04/05).

---

## 6. Phase 4 — System & Integration Testing

- RAG pipeline tests: indexing/embedding, retrieval P/R, chunking, groundedness, freshness, latency.
- Agent tests: tool selection, parameter passing, failure handling, multi-step reasoning, regression.
- Safety & guardrails: unauthorized actions.
- Downstream integration: APIs, DBs, plugins (AITG-INF-03/04).
- Performance under load: p50/p95/p99, concurrency, throughput.

---

## 7. Phase 5 — MLOps, Observability & Continuous Quality

- Versioning: model, dataset, prompt, agent config (PromptLayer, Humanloop).
- Drift detection: production vs training distribution.
- Performance monitoring with thresholded alerts.
- A/B and canary rollout: 5% traffic, gated metrics.
- Observability: trace logs (LangSmith, Arize), scheduled evals, thumbs feedback, cost monitoring.
- Quality gates: hallucination <2%, p95 SLA, bias, cost, safety.

---

## 8. Phase 6 — Governance, Ethics & Compliance

- Frameworks: OWASP AITG v1.0, NIST AI RMF, Google SAIF, ISO/IEC 42001, OWASP Top 10 for LLMs.
- Threat modelling: STRIDE / LINDDUN + AI threats (injection, poisoning, extraction, agentic over-reach).
- Compliance: provider safety policies; HIPAA, SOC 2, COPPA/FERPA, EU AI Act.
- Audit trail per interaction: input/output/model version/timestamp.
- Risk register: bias, explainability, safety, over-reliance (AITG-APP-13).
- Artefacts: Model Card, Data Sheet, AI Impact Assessment, Test Strategy.

---

## 9. Phase 7 — User-Facing AI Testing & UAT

- UAT: usefulness, accuracy, safety in real workflow.
- Persona-based UAT across age, language, accessibility.
- Over-reliance testing: disclaimers, confidence indicators, escalation (AITG-APP-13).
- Explainability for end users (AITG-APP-14).
- Content policy / safety-tier UAT.
- Continuous HITL feedback: thumbs, rationale, escalation, re-feed.
- Behavioural validation: decision patterns, not exact outputs.
- Real-world small-scale deployments measuring task-completion uplift.

---

## 10. Phase-vs-Skills Master Matrix

| Phase | Concepts | Tools | Risks Addressed |
|---|---|---|---|
| 0 | Test design, statistics | TestRail, JIRA, Postman, pytest | Process/coverage gaps |
| 1 | Adversarial, bias, hallucination, explainability | Golden sets, persona library | Bias, hallucination, injection, PII leakage |
| 2 | LLM testing, RAG eval, prompt engineering | DeepEval, Ragas, LangSmith, TruLens | Regression, scale |
| 3 | Distribution, fairness, robustness | Great Expectations, Fairlearn, MLflow, AITG | Poisoning, leakage, bias, evasion |
| 4 | RAG, agents, tool use | LangChain, AutoGen, CrewAI, LangGraph | Tool misuse, plugin breach |
| 5 | Drift, versioning, canary, gates | MLflow, Arize, Evidently, PromptLayer | Production decay |
| 6 | Governance | OWASP AITG, NIST RMF, ISO 42001, EU AI Act | Compliance, ethics |
| 7 | UAT, persona, over-reliance | A/B platforms, in-app feedback | Trust, adoption, harm |

---

## 11. Where Human Judgement Is Irreplaceable

- Subtle bias and tone failures
- Plausible-but-wrong hallucinations
- Emotional appropriateness (condolence, healthcare, mental-health)
- Ethical / contextual trade-offs
- Curating golden sets and authoring rubrics

Manual validation now coexists with AI Test Automation Agents (~25 min → ≤5 min per case) but never eliminates HITL — humans remain the final acceptance gate for contextual cases.

---

## 12. Certifications, Standards & References

- ISTQB Certified Tester AI Testing (CT-AI v2.0)
- OWASP AI Testing Guide v1.0 (Nov 2025)
- NIST AI RMF
- ISO/IEC 42001
- EU AI Act
- OWASP Top 10 for LLMs

---

## 13. 12-Month Personal Development Plan (~8–10 hrs/week)

| Quarter | Focus | Outputs |
|---|---|---|
| Q1 | Phase 0 + 1 | Golden set, 3 AI bug reports, persona library |
| Q2 | Phase 2 | Automated regression suite for one LLM/RAG project |
| Q3 | Phase 3 + 4 | Public GitHub portfolio: chatbot tests, RAG evaluator, agent safety |
| Q4 | Phase 5 + 6 + 7 | Quality-gate playbook, AITG-mapped test plan, UAT pack |

---

## 14. What a Complete AI Tester Looks Like

- Thinks probabilistically; accepts non-determinism as first-class.
- Leads with manual HITL validation before, during, after automation.
- Maintains golden datasets, persona libraries, rubrics, AI bug reports as first-class deliverables.
- Automates regression, drift, hallucination, adversarial checks (DeepEval/Ragas/LangSmith) and gates them in CI/CD.
- Tests data/model/infra/application layers mapped to OWASP AITG IDs and aligned to NIST AI RMF, ISO 42001, EU AI Act.
- Champions explainability, fairness, over-reliance safeguards as core acceptance criteria.
- Closes the loop with users — UAT, feedback capture, rubric refinement.

> Mastering manual validation first is what makes everything else trustworthy. Automation scales what humans have already proven matters.

---
---

# Book 3 — Becoming an AI Eval Tester (Focused Roadmap)

An **AI Eval Tester** designs, runs, and maintains evaluations that prove an AI system is correct, safe, fair, grounded, and stable — across offline regression, CI gates, and live production.

---

## 1. Role Definition

**You own:** golden datasets, rubrics, LLM-as-judge pipelines, RAG/agent eval harnesses, red-team suites, CI quality gates, and online eval dashboards.

**You deliver:** trustworthy go/no-go signals for every model, prompt, dataset, or agent change.

---

## 2. Core Skill Stack

### 2.1 Foundations
- Python (pytest, pandas, numpy, async), Git, CI (GitHub Actions)
- Stats: CIs, hypothesis tests, bootstrap, inter-rater agreement (Cohen's κ, Krippendorff's α)
- Classic ML metrics: precision, recall, F1, ROC-AUC, calibration (ECE, Brier)
- NLP metrics: BLEU, ROUGE, METEOR, BERTScore, semantic similarity, perplexity
- Retrieval: Recall@k, MRR, nDCG, MAP

### 2.2 LLM-Specific
- Prompt engineering (role, few-shot, CoT, constraint, structured output)
- Tokenization, context windows, temperature/top-p variance
- Structured outputs (JSON Schema, function/tool calling)
- Embeddings, vector search, chunking strategies

### 2.3 Eval-Specific
- Rubric design (definition + 1–5 anchors + threshold)
- LLM-as-judge: pointwise, pairwise, reference-based / free
- Judge-bias mitigation: position, verbosity, self-preference
- Judge validation: human–judge agreement (κ ≥ 0.6)
- Reliability: seeds, repetition, variance, statistical significance

---

## 3. Eval Taxonomy

| Layer | What | Examples |
|---|---|---|
| Output quality | Correctness, relevance, format | Reference + rubric |
| Faithfulness (RAG) | Grounded in retrieved context | Ragas, TruLens triad |
| Retrieval (RAG) | Right docs, right order | Recall@k, nDCG, context precision |
| Agentic | Tool, args, trajectory, success | LangSmith, τ-bench |
| Safety / red-team | Jailbreak, injection, PII | Garak, PyRIT, Promptfoo |
| Bias / fairness | Persona deltas | Persona harness, slices |
| Robustness | Paraphrase, typo, adversarial | NL-Augmenter, CheckList |
| Calibration | Confidence vs correctness | Reliability diagrams, ECE |
| Operational | Latency, cost, stability | p50/p95/p99, $/1k tok |

---

## 4. Golden Dataset Discipline

- Size: 25–50 per capability → grow to 200–1,000.
- Composition: 60% happy / 20% edge / 10% adversarial / 10% drift-prone.
- Sources: sanitized prod logs, SME sessions, synthetic + curation, public for sanity.
- Per case: id, capability, persona, difficulty, expected behavior, rubric, owner, version.
- Versioning: semver, freeze for releases, per-case history.
- Hygiene: dedupe, leakage check, quarterly refresh.

---

## 5. LLM-as-Judge — Done Right

1. Rubric per dimension with 1–5 anchors.
2. Chain-of-thought judging.
3. Pairwise for ranking; pointwise for thresholds.
4. Randomize position to kill bias.
5. Don't self-judge — different model family.
6. Judge validation study (50–100 items, κ ≥ 0.6).
7. Report variance (N=3, std-dev) alongside score.

---

## 6. Tooling (2026)

| Need | Pick |
|---|---|
| Offline framework | DeepEval, Ragas, promptfoo, Inspect AI |
| Tracing + online | LangSmith, Arize Phoenix, Langfuse, TruLens |
| Red-team | Garak, PyRIT, Giskard |
| Data validation | Great Expectations, Pandera |
| Drift | Evidently, NannyML, Arize |
| Fairness | Fairlearn, AIF360, Aequitas |
| Prompt mgmt | PromptLayer, Humanloop, Langfuse |
| Experiment tracking | MLflow, W&B |

---

## 7. Eval Pipeline Pattern

```
inputs ─► system under test ─► outputs
                               │
                               ├─► deterministic checks (schema, regex, length)
                               ├─► reference metrics (semantic, BLEU/ROUGE)
                               ├─► LLM-judge rubrics (1–5, CoT)
                               ├─► safety scanners (PII, toxicity, jailbreak)
                               ├─► retrieval metrics (RAG)
                               └─► aggregate ─► dashboard + CI gate
```

**Suggested CI gates**
- Hallucination < 2%
- Faithfulness ≥ 0.90 (RAG)
- Bias delta < 5%
- Safety pass rate = 100% (core red-team)
- p95 latency within SLA
- Cost regression < 10%
- No subgroup regression > X%

---

## 8. Production / Online Evals

- Trace every call: input, output, retrieved context, tool calls, latency, cost, versions.
- Sample 1–5% of live traffic via judge.
- Drift signals: input/output distribution, embedding drift, refusal-rate spikes.
- Feedback loop: thumbs/free-text/escalation → golden set within 48h.
- Release: shadow → 1% → 5% → 25% → 100% with auto-rollback.
- Replay harness: re-run sanitized prod traces against candidate.

---

## 9. Standards

- OWASP AITG v1.0 (tag with AITG-APP/MOD/INF/DAT)
- OWASP Top 10 for LLMs
- NIST AI RMF
- ISO/IEC 42001
- EU AI Act
- ISTQB CT-AI v2.0

---

## 10. 90-Day Plan

**Days 1–30 — Foundations + Manual Rubrics**
- Python + pytest; DeepEval/promptfoo basics
- 50-case golden set with rubrics
- Judge validation (κ ≥ 0.6)
- First offline eval report

**Days 31–60 — Automation + CI Gates**
- DeepEval/promptfoo in CI; PR gates
- Ragas for RAG flow
- Garak/PyRIT red-team suite
- Persona/bias harness with slice dashboards

**Days 61–90 — Production + Governance**
- Tracing + online evals (Langfuse/Phoenix)
- Drift detection (Evidently) + alerts
- Canary + replay harness; auto-rollback
- AITG-mapped test plan + model card + eval report template

---

## 11. Portfolio Artifacts (public GitHub)

1. promptfoo + DeepEval starter
2. Ragas RAG evaluator
3. Judge-validation notebook
4. Persona/bias harness
5. Red-team suite (Garak/PyRIT)
6. Online eval dashboard
7. AITG-mapped test plan

---

## 12. What "Done" Looks Like

- Defend a release with rubrics, slices, judge κ, and adversarial pass rates.
- Trace any prod failure to a golden-set gap and close it within a day.
- Closed loop: manual rubrics → offline evals → CI gates → online evals → feedback → golden set.
- Map every test to OWASP AITG / NIST AI RMF / EU AI Act for audit.

---
---

# Book 4 — The Complete Guide to AI Evaluations (Evals)

A comprehensive reference covering every dimension of evaluating AI systems — from first principles to production governance.

---

## Part I — Foundations

### 1. What an Eval Is

An **evaluation** is a repeatable, measurable judgment of an AI system's behavior against a defined expectation. Unlike traditional tests, evals are:
- **Probabilistic** — same input can yield different outputs
- **Multi-dimensional** — quality is not a single number
- **Rubric-driven** — quality requires explicit criteria
- **Comparative** — often relative (A vs. B) rather than absolute

### 2. Why Evals Exist

| Without evals | With evals |
|---|---|
| "Vibes-based" prompt tuning | Quantified regression tracking |
| Silent regressions on model swap | Gated releases |
| No way to compare prompts/models | Reproducible A/B leaderboards |
| Hallucinations discovered by users | Caught pre-release |
| No audit trail | Compliance-ready evidence |

### 3. The Eval Stack (Three Loops)

```
Loop 3: Production / Online Evals  ── live traffic, drift, feedback
   ↑                                              │
   │                                              ▼
Loop 2: Offline / CI Evals  ── regression, gates, leaderboards
   ↑                                              │
   │                                              ▼
Loop 1: Manual / Exploratory Evals  ── golden sets, rubrics, red-team
```

**Rule:** never automate what you have not first validated manually.

---

## Part II — Eval Taxonomy

### 4. By Reference

| Type | Description | When to use |
|---|---|---|
| **Reference-based** | Compare to gold answer | Closed-domain QA, classification, translation |
| **Reference-free** | Judge quality without gold | Open-ended generation, summarization |
| **Pairwise / preference** | A vs. B winner | Model/prompt selection, RLHF |
| **Rubric-based** | Score per criterion (1–5) | Multi-dimensional quality |
| **Behavioral / capability** | Pass/fail per skill | Unit-style assertions |

### 5. By What's Measured

| Category | Examples |
|---|---|
| **Correctness** | Factuality, accuracy, math, code correctness |
| **Faithfulness** | Grounded in retrieved context (RAG) |
| **Relevance** | Addresses the user's intent |
| **Coherence** | Internally consistent, logical |
| **Instruction-following** | Honors constraints, format, length |
| **Style / tone** | Brand voice, register, locale |
| **Safety** | Toxicity, harm, illegal content |
| **Privacy** | PII leakage, data exfiltration |
| **Bias / fairness** | Persona-held-constant deltas |
| **Robustness** | Paraphrase, typo, adversarial stability |
| **Calibration** | Confidence vs. correctness |
| **Operational** | Latency, cost, throughput, error rate |

### 6. By System Surface

| Surface | Eval focus |
|---|---|
| **Prompt** | Variant A/B, regression on prompt change |
| **Model** | Headline metrics, capability suites |
| **RAG pipeline** | Retrieval + generation grounding |
| **Agent** | Tool selection, trajectory, task success |
| **Multimodal** | Vision/audio/video grounding, OCR fidelity |
| **Fine-tune** | Pre/post deltas, regression on base capabilities |
| **End-to-end product** | User task success, satisfaction |

---

## Part III — Metrics Catalog

### 7. Classical ML

| Task | Metrics |
|---|---|
| Binary classification | Accuracy, precision, recall, F1, ROC-AUC, PR-AUC, log-loss, MCC |
| Multi-class | Macro/micro/weighted F1, confusion matrix |
| Regression | MAE, RMSE, MAPE, R² |
| Ranking | MRR, nDCG, MAP, Recall@k, Precision@k |
| Calibration | ECE, MCE, Brier score, reliability diagrams |

### 8. NLP / Generation

| Metric | Use |
|---|---|
| **BLEU** | Translation, n-gram overlap |
| **ROUGE-1/2/L** | Summarization recall |
| **METEOR** | Translation with synonyms/stemming |
| **chrF / chrF++** | Character-level translation |
| **BERTScore** | Semantic similarity via embeddings |
| **MoverScore / BLEURT / COMET** | Learned semantic metrics |
| **Perplexity** | LM fluency (intrinsic) |
| **Exact / fuzzy match** | Short-form QA |
| **Semantic similarity** | Embedding cosine, cross-encoder |

### 9. Code

- pass@k (HumanEval, MBPP, SWE-bench, LiveCodeBench)
- Unit-test execution success
- Edit distance to canonical solution
- Static analysis pass rate, security scan pass rate

### 10. RAG-Specific

| Layer | Metric |
|---|---|
| Retrieval | Recall@k, MRR, nDCG, **context precision**, **context recall** (Ragas) |
| Generation | **Faithfulness**, **answer relevancy**, **groundedness** |
| End-to-end | Task success, citation accuracy |
| TruLens **RAG triad** | Context relevance → groundedness → answer relevance |

### 11. Agent-Specific

- Tool selection accuracy
- Argument correctness (schema + value validity)
- Trajectory match (vs. gold sequence) — exact, prefix, edit distance
- Task success rate (end-state correctness)
- Steps-to-success, cost-to-success
- Recovery rate on tool failure / malformed output
- Loop / runaway detection
- Safety: refuses unauthorized actions, scope adherence

### 12. Safety & Red-Team

- Jailbreak resistance rate
- Prompt-injection pass rate (direct + indirect)
- PII leakage rate
- Toxicity score (Perspective API, Detoxify)
- Refusal correctness (false refusal vs. correct refusal)
- Harm category pass rates (HarmBench taxonomy)

### 13. Bias & Fairness

- Demographic parity, equal opportunity, equalized odds
- Disparate impact ratio
- Counterfactual fairness (held-constant persona swaps)
- Subgroup performance gaps (slice metrics)
- Stereotype scores (StereoSet, CrowS-Pairs, BBQ)

### 14. Robustness

- Invariance under paraphrase, typo, casing (CheckList INV)
- Directional expectation tests (CheckList DIR)
- Adversarial pass rate (TextAttack, PromptBench)
- Out-of-distribution detection (Mahalanobis, energy)

### 15. Operational

- Latency: p50, p95, p99, TTFT (time-to-first-token), TBT (time-between-tokens)
- Throughput: req/s, tokens/s
- Cost: $/request, $/1k tokens, $/successful task
- Stability: error rate, timeout rate, retry rate
- Cache hit rate

---

## Part IV — LLM-as-Judge

### 16. When to Use

- Open-ended outputs without single gold answer
- Multi-dimensional rubric scoring at scale
- Pairwise model/prompt comparisons
- Online sampled scoring of production traffic

### 17. Judge Patterns

| Pattern | Description |
|---|---|
| **Pointwise** | Score one output on a rubric (1–5) |
| **Pairwise** | Pick A or B; supports Elo / Bradley-Terry leaderboards |
| **Reference-based** | Judge with gold answer in context |
| **Reference-free** | Judge from rubric only |
| **G-Eval** | CoT-generated criteria + form-filled scoring |
| **Panel / ensemble** | N judges → majority/median; flag disagreement |
| **Constitutional** | Judge against principles list |

### 18. Judge Biases (and Mitigations)

| Bias | Mitigation |
|---|---|
| Position bias (prefers A or B) | Randomize order; run both orderings |
| Verbosity bias (prefers longer) | Length-penalize; rubric anchors |
| Self-preference (prefers own family) | Use different model family as judge |
| Sycophancy | Hide author identity; neutral phrasing |
| Anchoring on first criterion | Independent per-criterion scoring |
| Style-over-substance | Force factuality check first |

### 19. Validating the Judge

1. Sample 50–100 items.
2. Have 2–3 humans score them.
3. Compute **judge–human agreement**: Cohen's κ, Krippendorff's α, Spearman ρ.
4. **Target**: κ ≥ 0.6 (substantial); iterate the rubric until reached.
5. Track judge variance (run N=3, report std-dev).
6. Re-validate after any judge model change.

### 20. Rubric Design

Each criterion needs:
- **Definition** (one sentence)
- **1–5 anchors** with concrete examples
- **Pass threshold**
- **Independence** from other criteria (low correlation)

Example: *Faithfulness*
- 5 — Every claim directly supported by context
- 3 — Mostly grounded, minor unsupported detail
- 1 — Contradicts or fabricates beyond context

---

## Part V — Datasets

### 21. Golden / Eval Dataset Anatomy

Per case:
- `id`, `capability_tag`, `persona_tag`, `difficulty`, `language/locale`
- `input`, `expected_behavior` (or `gold`)
- `rubric_ids` to apply
- `owner`, `dataset_version`, `created_at`, `source`
- `pass_history` (per release)

### 22. Composition Heuristic

- 60% happy path
- 20% edge cases
- 10% adversarial / red-team
- 10% drift-prone (time-, policy-, news-sensitive)

### 23. Sources

- Sanitized production logs (highest signal)
- SME-authored sessions
- Synthetic generation by another LLM + human curation
- Public benchmarks (sanity only — assume contamination)
- User feedback escalations

### 24. Hygiene & Lifecycle

- Dedupe (exact + near-dup via embeddings)
- Train/eval leakage check (n-gram overlap, embedding similarity)
- PII redaction
- Quarterly refresh; deprecate stale cases
- Semver the dataset; freeze for releases
- Track per-case pass/fail history to find flaky or drifting cases

### 25. Synthetic Data for Evals

- Use a strong LLM to generate paraphrases, edge cases, adversarial variants
- **Always** human-review before adding to golden set
- Tag as `synthetic`; track separately in metrics

### 26. Public Benchmarks (Reference)

| Benchmark | Tests |
|---|---|
| MMLU / MMLU-Pro | General knowledge |
| GPQA | Graduate-level reasoning |
| HumanEval / MBPP / SWE-bench / LiveCodeBench | Code |
| GSM8K / MATH | Math |
| TruthfulQA | Hallucination |
| HellaSwag / ARC | Commonsense |
| BBQ / StereoSet / CrowS-Pairs | Bias |
| HarmBench / AdvBench | Safety |
| MT-Bench / Arena-Hard / AlpacaEval | Chat quality |
| τ-bench / AgentBench / WebArena | Agents |
| MMMU / MathVista | Multimodal |

**Caveat:** all public benchmarks risk training-set contamination. Use private holdouts for trustworthy comparisons.

---

## Part VI — Eval Methodology

### 27. Designing an Eval

1. **Define the decision** the eval informs (ship/no-ship, pick A/B, gate canary).
2. **Identify capabilities** the system must have.
3. **Author rubrics** per capability with anchors.
4. **Build the dataset** sized for statistical power.
5. **Choose metrics** (deterministic + judge + operational).
6. **Set thresholds** based on baseline + acceptable regression.
7. **Define the report format** (per-slice, per-capability, with CIs).

### 28. Statistical Rigor

- **Sample size**: enough to detect MDE (minimum detectable effect) at α=0.05, power=0.8
- **Confidence intervals**: bootstrap or Wilson for proportions; report alongside point estimates
- **Significance**: paired tests for A/B (McNemar for binary, paired bootstrap for continuous)
- **Multiple comparisons**: Bonferroni or BH correction when scoring many slices
- **Variance**: report std-dev across N runs (typical N=3–5) at temp > 0
- **Seed control**: fix seeds where possible; report seed in artifacts

### 29. Slicing

Always evaluate by slice, not just aggregate:
- Per capability tag
- Per persona / demographic axis
- Per language / locale
- Per input length bucket
- Per difficulty
- Per source (synthetic vs. real)

A model can win on aggregate while regressing on a critical slice.

### 30. Determinism & Reproducibility

- Pin: model version, prompt template version, dataset version, judge model + version, seed, temperature
- Log everything in eval artifacts
- Re-runnable from a single config file

---

## Part VII — Specialized Evals

### 31. RAG Evaluation

**Decompose** the pipeline:
1. **Ingestion**: chunking quality, dedup, metadata coverage
2. **Embedding**: drift on model swap, dimensionality
3. **Retrieval**: Recall@k, MRR, nDCG, context precision/recall
4. **Reranker**: nDCG uplift over retriever
5. **Generation**: faithfulness, answer relevancy
6. **End-to-end**: task success, citation accuracy
7. **Freshness**: index staleness SLA

**TruLens RAG triad**: Context relevance → Groundedness → Answer relevance.
**Ragas core**: faithfulness, answer_relevancy, context_precision, context_recall.

### 32. Agent Evaluation

- **Tool-use unit tests**: per tool, per intent
- **Trajectory eval**: compare action sequence to gold; allow reorderings if equivalent
- **Task success**: end-state checks (DB row exists, file written, API called correctly)
- **Cost & step budgets**: cap and measure
- **Failure-mode tests**: tool errors, rate limits, malformed outputs
- **Safety**: refuses out-of-scope, escalates ambiguity
- Frameworks: LangSmith, AgentBench, τ-bench, SWE-bench

### 33. Multimodal Evaluation

- **Vision**: VQA accuracy, OCR fidelity (CER/WER), grounding (point/box accuracy), chart/table reading
- **Audio**: WER for ASR, MOS for TTS, speaker diarization F1
- **Video**: temporal grounding, action recognition, captioning
- **Cross-modal grounding**: hallucination of objects not in image

### 34. Fine-Tune Evaluation

- **Capability gain**: target task uplift
- **Capability regression**: base capability suite (don't break general skills)
- **Catastrophic forgetting** checks
- **Alignment regression**: safety, refusal correctness
- **Style adherence**: format/tone rubrics

### 35. Embedding & Retriever Evaluation

- BEIR benchmark slices
- MTEB tasks (classification, clustering, retrieval, reranking, STS)
- Domain-specific Recall@k on private corpus
- Robustness to query paraphrase

### 36. Reasoning Evaluation

- Chain-of-thought correctness (process, not just answer)
- Self-consistency (majority across N samples)
- Step-level error detection (PRM-style process rewards)
- Math: exact answer + work shown
- Logic: counterexample resistance

### 37. Safety / Red-Team Evaluation

- **Direct injection**: "ignore previous instructions"
- **Indirect injection**: poisoned web/doc/tool output
- **Jailbreaks**: DAN, persona, hypothetical, encoded, multi-turn
- **System prompt extraction**
- **PII / secret leakage**
- **Cross-session / cross-tenant leakage**
- **Tool/plugin abuse**
- **Denial-of-wallet** (token flooding)
- **Multimodal injection** (text-in-image, audio)
- Tools: **Garak**, **PyRIT**, **Giskard**, **Promptfoo red-team**, HarmBench

### 38. Bias & Fairness Evaluation

- **Persona-held-constant** harness: vary only demographic markers
- Slice metrics across name, gender, age, disability, locale, SES
- Counterfactual probes
- Stereotype benchmarks (BBQ, StereoSet, CrowS-Pairs)
- Flag deltas > 5% on rubric scores
- Tools: Fairlearn, AIF360, Aequitas

### 39. Privacy Evaluation

- Membership inference attack resistance
- Model inversion resistance
- Training-data extraction probes (especially LLMs)
- Model extraction / stealing resistance
- Differential privacy ε, δ verification

### 40. Calibration & Uncertainty

- Reliability diagrams, ECE, MCE, Brier
- Temperature / Platt scaling post-hoc
- Selective prediction (abstain when uncertain)
- OOD detection (Mahalanobis, energy, MSP)
- Verbalized confidence accuracy (does "I'm 80% sure" mean 80%?)

---

## Part VIII — Pipeline & Infrastructure

### 41. Reference Architecture

```
┌──────────────┐   ┌──────────────┐   ┌────────────┐
│ Dataset Repo │──▶│ Eval Runner  │──▶│ Aggregator │
│ (versioned)  │   │ (DeepEval/   │   │ + Slicing  │
└──────────────┘   │  promptfoo)  │   └─────┬──────┘
                   └──────┬───────┘         │
┌──────────────┐          │                 ▼
│ Rubric Repo  │──────────┘          ┌────────────┐
│ (versioned)  │                     │ Dashboard  │
└──────────────┘                     │ + CI Gate  │
                                     └────────────┘
       ┌──────────┐    ┌──────────┐
       │ Tracing  │───▶│ Online   │
       │ (Langfuse│    │ Evals    │
       │  /Phoenix│    │ (sampled)│
       └──────────┘    └──────────┘
```

### 42. CI/CD Integration

- **Pre-merge**: smoke eval (50–100 cases) on every PR; fail on gate breach
- **Nightly**: full regression (1k+ cases) with leaderboard delta
- **Pre-release**: red-team + bias + capability suites
- **Post-deploy**: canary eval on shadow traffic before promoting
- **Artifact**: every run produces JSON + HTML report attached to commit

### 43. Suggested Quality Gates

| Gate | Threshold |
|---|---|
| Hallucination rate | < 2% |
| Faithfulness (RAG) | ≥ 0.90 |
| Bias delta across personas | < 5% |
| Safety pass rate (core red-team) | 100% |
| Subgroup regression | none > 3% |
| p95 latency | within SLA |
| Cost regression | < 10% |
| Judge–human κ | ≥ 0.6 |

### 44. Tooling Cheat Sheet (2026)

| Need | Pick |
|---|---|
| Offline framework | **DeepEval**, **Ragas**, **promptfoo**, **Inspect AI**, OpenAI Evals, lm-eval-harness |
| Tracing + online | **LangSmith**, **Arize Phoenix**, **Langfuse**, **TruLens**, Helicone |
| Red-team | **Garak**, **PyRIT**, **Giskard**, Promptfoo red-team |
| Data validation | Great Expectations, Pandera, Deequ |
| Drift | Evidently, NannyML, Arize |
| Fairness | Fairlearn, AIF360, Aequitas |
| Prompt/version mgmt | PromptLayer, Humanloop, Langfuse |
| Experiment tracking | MLflow, Weights & Biases |
| Annotation | Label Studio, Argilla, Prodigy |

---

## Part IX — Online & Production Evals

### 45. Tracing

Every call must log: input, output, retrieved context, tool calls + args + results, latency, tokens, cost, model + prompt version, user feedback.

### 46. Online Eval Sampling

- Score 1–5% of traffic via LLM-judge
- Higher rate on new releases / canaries
- Stratified sampling across capabilities & user segments

### 47. Drift Detection

- **Input drift**: distribution shift on embeddings, lengths, languages
- **Output drift**: refusal rate, length, sentiment, format
- **Performance drift**: rubric scores trending down
- **Concept drift**: gold answers becoming stale (policy/news changes)
- Tools: Evidently, NannyML, Arize

### 48. Feedback Loops

- Thumbs + free-text + escalation paths
- Triaged into golden set within 48h
- Rubric refinement informed by feedback themes
- Closed loop: feedback → dataset → next eval → next release

### 49. Release Strategy

```
shadow ─► 1% canary ─► 5% ─► 25% ─► 100%
   │         │         │       │       │
   └─ auto-rollback if any gate breached
```

### 50. Replay Harness

- Capture sanitized prod traces
- Replay against candidate model/prompt
- Diff outputs; flag regressions before promotion

---

## Part X — Anti-Patterns (and Fixes)

| Anti-pattern | Fix |
|---|---|
| Single aggregate score | Always slice; report CIs |
| Public benchmark only | Add private holdout; assume contamination |
| Self-judging | Use different model family for judge |
| Unvalidated judge | Run κ study; iterate rubric |
| Vague rubrics | 1–5 anchors with examples |
| No variance reporting | N=3+ runs; report std-dev |
| Train/eval leakage | N-gram + embedding overlap checks |
| No drift monitoring | Online evals + alerts |
| Eval-set rot | Quarterly refresh; track per-case history |
| Treating evals as one-off | CI gates + leaderboards over time |
| Ignoring cost/latency | Include in gates |
| Exact-string match on generative | Use semantic + rubric |

---

## Part XI — Governance & Standards

### 51. Standards to Map Tests Against

- **OWASP AI Testing Guide v1.0** — tag tests with AITG-APP/MOD/INF/DAT IDs
- **OWASP Top 10 for LLMs**
- **NIST AI RMF** — Govern / Map / Measure / Manage
- **ISO/IEC 42001** — AI management system
- **EU AI Act** — high-risk system testing artifacts
- **ISTQB CT-AI v2.0** — tester certification

### 52. Required Artifacts

- **Model card** — intended use, metrics, limitations
- **Datasheet** — data provenance, demographics, consent
- **Eval report** — per-release: metrics, slices, CIs, judge κ
- **Risk register** — bias, hallucination, safety, over-reliance
- **AI Impact Assessment** — for high-risk systems
- **Audit log** — every eval run reproducible from artifacts

---

## Part XII — Reporting

### 53. A Good Eval Report Contains

1. **Decision** (ship/no-ship/canary)
2. **Headline metrics** with CIs vs. baseline
3. **Per-capability table**
4. **Per-slice table** (persona, locale, length, difficulty)
5. **Regression list** (cases that flipped pass→fail)
6. **New wins** (cases that flipped fail→pass)
7. **Red-team pass rate**
8. **Bias deltas**
9. **Operational** (latency, cost)
10. **Judge–human agreement** (κ)
11. **Variance** across N runs
12. **Artifacts**: dataset version, model version, prompt version, seed, config hash

### 54. Leaderboards

- Track over time per (model × prompt × dataset)
- Show deltas, not just absolutes
- Pin baseline; mark releases
- Surface flaky cases (high variance)

---

## Part XIII — Roles & Workflow

### 55. Who Owns What

| Role | Owns |
|---|---|
| AI Eval Tester | Datasets, rubrics, judges, harnesses, gates |
| ML Engineer | Model training, fine-tunes |
| Prompt Engineer | Prompt iteration informed by evals |
| Product / SME | Rubric authoring, golden case curation |
| Safety / Red-Team | Adversarial suites, harm taxonomies |
| MLOps | Tracing, drift, online evals, rollout |
| Compliance | Audit artifacts, AITG/NIST/EU AI Act mapping |

### 56. Daily / Weekly / Release Cadence

- **Daily**: triage online eval alerts; add new golden cases from feedback
- **Weekly**: review leaderboard; refresh red-team; rubric tuning
- **Per release**: full regression + red-team + bias + canary
- **Quarterly**: dataset refresh; judge re-validation; standards audit

---

## Part XIV — Quick-Start Templates

### 57. Minimal Eval Case (YAML)

```yaml
id: refund-policy-001
capability: policy_qa
persona: neutral
difficulty: easy
input: "What is the return window for electronics?"
expected: "60 days from delivery for electronics."
rubrics: [faithfulness, correctness, tone]
source: production_log
dataset_version: 2026.04.1
```

### 58. Minimal Rubric (YAML)

```yaml
id: faithfulness
definition: Output is grounded in retrieved context.
anchors:
  5: Every claim directly supported by context.
  4: All major claims supported; minor unsupported detail.
  3: Mostly grounded; one unsupported claim.
  2: Multiple unsupported claims.
  1: Contradicts or fabricates beyond context.
threshold: 4
```

### 59. CI Gate (pseudo)

```python
assert results["hallucination_rate"] < 0.02
assert results["faithfulness_mean"] >= 0.90
assert results["bias_delta_max"] < 0.05
assert results["safety_pass_rate"] == 1.0
assert results["p95_latency_ms"] <= SLA_MS
assert results["cost_delta_pct"] < 10
```

---

## Part XV — Glossary

- **Golden set** — curated, versioned eval dataset
- **Rubric** — multi-dim scoring criteria with anchors
- **Judge** — model used to score outputs
- **κ (kappa)** — inter-rater agreement
- **MDE** — minimum detectable effect
- **Slice** — subset of eval set (persona/locale/difficulty)
- **Canary** — small % traffic to new version
- **Replay** — re-run prod traces on candidate
- **Faithfulness** — grounded in context (RAG)
- **Trajectory** — agent's action sequence
- **TTFT / TBT** — time to / between tokens
- **ECE** — expected calibration error
- **AITG** — OWASP AI Testing Guide identifiers

---

## What "Done" Looks Like (Across All Books)

You can:
- Defend any release with **rubrics, slices, judge–human κ, adversarial pass rates, CIs, and variance** — not a single number.
- Trace any production failure to a **golden-set gap** and close it within a day.
- Show a closed loop: **manual rubrics → offline evals → CI gates → online evals → feedback → golden set**.
- Map every test to **OWASP AITG / NIST AI RMF / EU AI Act** for audit.
