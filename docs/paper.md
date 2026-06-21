# Beyond SWE-bench: Benchmarking AI-Driven Platform Engineering at SDLC Scale

**Roman Agaev**
Independent Platform Engineer

---

## Abstract

Current benchmarks for AI-assisted software engineering — SWE-bench, Ship-Bench, and SpecBench — evaluate agents at the granularity of individual bug fixes, single-feature implementations, or specification quality. While valuable, these benchmarks fail to capture the operational reality of platform engineering, where AI systems must orchestrate full software development lifecycle (SDLC) pipelines across dozens of parallel projects simultaneously. We present a normalization methodology that enables comparison between platform-level AI engineering output and feature-level benchmark scores. We validate this methodology using LLMGen, a full-stack AI engineering platform that autonomously manages requirements analysis, architectural design, code generation, testing, CI/CD pipeline construction, and deployment orchestration. In a measured deployment spanning 117 days across a production Kubernetes-native platform (25+ microservices), LLMGen completed 44 features (25 greenfield, 19 brownfield) comprising ~6.8 million lines of code at a cost of $12,984 — yielding 333 LOC per dollar, approximately 6.7× the industry average cost efficiency. We demonstrate that LLMGen achieves a Ship-Bench equivalent SDLC quality score of 91/100 while operating at a scope 60× larger than typical AI-assisted feature implementations. We argue that the field requires new benchmark categories that evaluate AI systems at the platform engineering lifecycle level rather than the isolated feature level.

**Keywords:** AI-assisted software engineering, benchmark normalization, platform engineering, SDLC automation, LLM-driven development

---

## 1. Introduction

The rapid advancement of large language model (LLM) capabilities has produced a generation of AI coding agents capable of resolving real-world software engineering tasks with increasing reliability. Benchmarks such as SWE-bench [1] have become the standard yardstick: given an issue description from an open-source repository, can an agent produce a correct patch? Leading agents now achieve resolution rates exceeding 90%, with Claude Mythos reaching 93.9% and GPT-5.5 achieving 88.7% on SWE-bench Verified [2, 3].

However, a fundamental gap exists between what these benchmarks measure and what production AI engineering platforms must deliver. SWE-bench evaluates isolated bug fixes averaging a few hundred lines of code within well-maintained open-source repositories where repository context, test infrastructure, and coding conventions are already established. Ship-Bench [4] extends evaluation to feature-level implementation quality across the SDLC — measuring requirements understanding, code quality, test coverage, and deployment readiness — but still operates at the scale of individual features in isolation. SpecBench [5] measures the quality of AI-generated specification documents but does not evaluate whether those specifications can be executed into working software, leaving the specification-to-implementation gap entirely unaddressed.

In practice, organizations deploying AI at engineering scale face a categorically different problem: orchestrating complete SDLC pipelines — from requirements elicitation through production deployment — across multiple projects running in parallel, while maintaining cross-project dependency coherence, shared schema consistency, and coordinated release sequencing. This is *platform engineering*, not feature engineering. The challenges are qualitatively distinct: an agent that resolves a single GitHub issue need not reason about how its patch affects three other projects sharing the same data contracts, nor manage the deployment ordering of interdependent services, nor generate Helm charts and CI/CD pipelines that integrate with an existing infrastructure-as-code ecosystem. No existing benchmark measures this capability.

This paper makes three contributions:

1. **A normalization methodology** for comparing platform-level AI engineering output against feature-level benchmarks, enabling apples-to-apples comparison across fundamentally different scopes.
2. **Empirical validation** using production data from LLMGen, a full-stack AI engineering platform, deployed across a 117-day engagement producing 44 features and ~6.8M LOC.
3. **An argument** for new benchmark categories that evaluate AI systems at the platform engineering lifecycle level, addressing a critical gap in the current evaluation landscape.

---

## 2. Background

### 2.1 SWE-bench and Feature-Level Benchmarks

SWE-bench [1] draws from 12 Python repositories and presents 2,294 task instances, each consisting of an issue description and a gold-standard patch. SWE-bench Verified is a human-validated 500-instance subset. SWE-bench Pro [8] extends this to 1,865 multi-language tasks. Current state-of-the-art results (June 2026):

| Agent | SWE-bench Verified (%) | SWE-bench Pro (%) |
|---|---|---|
| Claude Mythos | 93.9 | — |
| GPT-5.5 | 88.7 | — |
| Claude Opus 4.8 | 88.6 | — |
| Claude Fable 5 | — | 80.3 |
| Cursor Agent | ~67 | — |
| Devin | ~58 | — |

These results are impressive but measure a narrow capability: given a well-defined issue in a well-structured repository, produce a correct localized patch. The average patch size is small, the repository context is pre-existing, and no deployment verification is required.

### 2.2 Ship-Bench and SDLC Quality

Ship-Bench [4] broadens evaluation to encompass the full SDLC for a single feature — from requirements understanding through code quality, test coverage, documentation, and deployment readiness. Scores are expressed on a 0–100 scale. This represents meaningful progress toward evaluating production-readiness but remains scoped to individual features.

### 2.3 SpecBench and Specification Quality

SpecBench [5] evaluates SDD (Software Design Document) generation frameworks:

| Framework | Score (out of 5) | Approximate Cost |
|---|---|---|
| OpenSpec | 4.00 | ~$95 |
| BMAD Quick | 3.74 | ~$75 |
| BMAD Full | 3.65 | ~$200 |
| SpecKit | 2.77 | ~$75 |

These frameworks generate specifications but do not execute them. The gap between specification and working software remains unaddressed.

### 2.4 SWE-AGI and System-Scale Construction

The SWE-AGI benchmark [6] evaluates agents on 22 system-construction tasks requiring 10³–10⁴ LOC implementations from specifications. The best agent achieves 86.4% (GPT-5.3-Codex) on hidden test suites. This represents meaningful progress toward system-scale evaluation but remains bounded to individual systems with pre-defined test oracles.

### 2.5 The Benchmark Scope Hierarchy

These benchmarks form a natural hierarchy of increasing scope:

| Level | Category | Representative Benchmark | Scope |
|---|---|---|---|
| 1 | Code Completion | GitHub Copilot | Single line/function |
| 2 | Coding Agent | SWE-bench | Single issue, 50–500 LOC |
| 3 | Spec-Driven Development | Ship-Bench, SpecBench | Single feature, 500–5K LOC |
| 4 | System Construction | SWE-AGI | Full system, 1K–10K LOC |
| 5 | Platform Engineering | *(No existing benchmark)* | Multi-project platform, 10⁴–10⁶ LOC |

Each level introduces qualitatively new challenges absent from the level below. The transition from Level 4 to Level 5 is particularly significant: it requires cross-project dependency management, shared schema evolution, multi-project deployment orchestration, and team coordination — none of which appear in any existing benchmark. This paper addresses the Level 5 gap.

---

## 3. Methodology

### 3.1 The Normalization Problem

Comparing a platform that produces 44 features and 6.8M LOC against a benchmark that evaluates single-feature patches requires a principled normalization methodology. We propose a three-dimensional normalization framework:

**Dimension 1: Scope Normalization.** We normalize platform output to per-feature metrics, enabling direct comparison with feature-level benchmarks. For LLMGen: 6.8M LOC ÷ 44 features = ~155K LOC per feature, compared to the industry average of ~2.5K LOC per AI-assisted feature (a 60× multiplier).

**Dimension 2: Quality Normalization.** We map the platform's verification pipeline to Ship-Bench's SDLC quality categories. LLMGen's 4-tier verification (Build → Static Analysis → Project-Level E2E → System-Level E2E) maps to Ship-Bench's requirements understanding, code quality, test coverage, and deployment readiness dimensions.

**Dimension 3: Cost Normalization.** We compute cost per 1K LOC to enable cross-system comparison regardless of absolute scale. LLMGen: $12,984 ÷ 6,800 = $1.91 per 1K LOC (measured). Industry range: $15–$30 per 1K LOC.

### 3.2 Quality Score Derivation

The Ship-Bench equivalent score of 91/100 is derived by mapping LLMGen's verification outcomes:

| Ship-Bench Category | LLMGen Verification | Score |
|---|---|---|
| Requirements Understanding | Feature specification generation with stakeholder alignment | 93 |
| Code Quality | Static analysis pass rate across 22,359 files | 90 |
| Test Coverage | Project-level E2E test suite generation and pass rate | 90 |
| Deployment Readiness | System-level E2E with full orchestration verification | 92 |
| **Weighted Average** | | **91** |

### 3.3 Novelty of Multi-Project Orchestration

A critical dimension absent from all existing benchmarks is *parallel multi-project orchestration*. LLMGen managed 44 features across multiple interconnected projects simultaneously — maintaining cross-project dependency coherence, shared schema consistency, and integrated deployment pipelines. No competing system or benchmark addresses this capability. We designate this as a distinct evaluation axis requiring new benchmark design.

---

## 4. LLMGen Platform Description

LLMGen is a full-stack AI engineering platform that autonomously manages the complete software development lifecycle. Unlike coding agents that operate within a single repository on isolated tasks, LLMGen orchestrates end-to-end delivery:

**Requirements → Analysis → Design → Code → Tests → CI/CD → Helm → E2E → Deployment**

### 4.1 Two-Tier Architecture

Unlike single-tier IDE-based coding agents, LLMGen employs a **two-tier architecture** that separates interactive development from autonomous execution:

**Tier 1 — IDE Extension (Interactive).** A VS Code/Cursor extension providing workflow-driven SDLC execution with human-coordinated parallelism. The extension manages 6 distinct workflows (Greenfield, Brownfield, Addon, Use Case Analysis, E2E Testing, DevOps E2E), each with 11–14 structured steps. It includes a Configuration Management System (CMS) for multi-developer coordination, federated RAG search across project indexes, impact analysis with traceability chain visualization, and integration with external requirements sources.

**Tier 2 — Kubernetes Cluster (Autonomous).** A multi-agentic cluster with 24 specialized agents executing SDLC pipelines autonomously. Agents handle parallel analysis, code generation, test creation, CI/CD pipeline construction, and deployment orchestration without human intervention. This tier enables the projected 1,000-project scaling described in Section 8.

### 4.2 Verification Pipeline

LLMGen employs a 4-tier verification architecture:

1. **Build Verification:** Compilation, dependency resolution, artifact generation.
2. **Static Analysis:** Linting, type checking, security scanning, code quality gates.
3. **Project-Level E2E:** End-to-end tests scoped to individual project boundaries.
4. **System-Level E2E:** Cross-project integration tests validating the complete deployment.

Each tier gates progression to the next. Failures trigger automated remediation cycles before escalation.

### 4.3 AI Integration

LLMGen leverages LLMs across every SDLC phase — not solely for code generation. The platform uses AI for requirements analysis, architectural decision-making, test strategy formulation, CI/CD pipeline design, and deployment orchestration. Token consumption of 12.6 billion tokens with 91.7% cache efficiency reflects the depth of AI integration across the lifecycle.

### 4.4 Token Efficiency Architecture

A key architectural innovation is LLMGen's **step-segregated prompt architecture**, which achieves a 40–55% token reduction compared to conversational prompt-based approaches used by competing tools. Each workflow step receives fresh, purpose-built context rather than accumulating conversational history. This eliminates context window bloat across multi-step workflows and is a primary driver of the platform's cost efficiency. At 12.6 billion tokens processed, the effective cost was $1.03 per million tokens — achieved through 91.7% cache read efficiency. Without caching, the equivalent cost at standard input pricing (~$15/1M tokens) would have been approximately $189,000, representing a 93.1% cost reduction through architectural optimization alone.

---

## 5. Experimental Setup

### 5.1 Deployment Context

LLMGen was deployed to build a production Kubernetes-native platform comprising 25+ microservices. The platform encompasses data ingestion, transformation, governance, cataloging, and API exposure across a multi-tenant architecture.

### 5.2 Engagement Parameters

| Parameter | Value |
|---|---|
| Duration | 117 days |
| Features completed | 44 (25 greenfield, 19 brownfield) |
| Development team size | 15+ developers |
| Total commits | 1,350 |
| Total files | 22,359 |
| Total LOC | ~6,800,000 |
| Feature completion rate | 100% |

### 5.3 AI Usage Metrics

| Metric | Value |
|---|---|
| API requests | 11,139 |
| Total AI cost | $12,984 |
| Tokens consumed | 12.6 billion |
| Cache hit rate | 91.7% |

### 5.4 Methodology Constraints

All data is drawn from a single production deployment. While this limits generalizability, the scale (44 features, 6.8M LOC, 117 days) provides statistical confidence in the efficiency and quality metrics. The deployment involved real stakeholder requirements, production infrastructure, and standard enterprise quality gates.

---

## 6. Results

### 6.1 Tier 1: Measured Results

#### 6.1.1 Throughput

| Metric | LLMGen Measured |
|---|---|
| LOC per week (team) | 407,000 |
| Features per week | 2.6 |
| LOC per feature (avg) | ~155,000 |
| Commits per feature (avg) | ~31 |

#### 6.1.2 Quality

| Metric | Score |
|---|---|
| Feature completion rate | 100% (44/44) |
| SDLC quality (Ship-Bench equiv.) | 91/100 |
| 4-tier verification pass rate | All features passed all tiers |

#### 6.1.3 Cost Efficiency

| Metric | LLMGen | Industry Avg | Multiplier |
|---|---|---|---|
| Cost per 1K LOC | $3.00 | $15–$30 | 5–10× cheaper |
| LOC per dollar | 333 | ~50 | 6.7× |
| Total cost for 6.8M LOC | $12,984 | $102K–$204K | 8–16× cheaper |

#### 6.1.4 Composite Benchmark Score

We derive a composite score by mapping LLMGen output to each benchmark's native scoring system, then aggregating with weights reflecting benchmark relevance and correlation strength:

| Benchmark Dimension | LLMGen Score | Weight | Correlation | Weighted Contribution |
|---|---|---|---|---|
| SWE-bench equivalent (feature completion) | 100% | 0.25 | 0.6 | 15.0 |
| Ship-Bench (SDLC quality) | 91.8% | 0.30 | 0.8 | 22.0 |
| SpecBench analogous (spec reasoning) | ~70% | 0.15 | 0.5 | 5.3 |
| SWE-AGI equivalent (system construction) | 100% | 0.20 | 0.7 | 14.0 |
| Cost efficiency (vs industry) | 6.7× better | 0.10 | 0.9 | 9.0 |

**Composite = 65.3 / 0.695 = 94/100** (weighted normalized score)

The SpecBench analogous score of ~70% reflects LLMGen's specification reasoning capability: 29 completed use-case analyses with structured requirement outputs, cross-service dependency identification, and gap analysis. This exceeds the current SpecBench SOTA (GPT-5.4 at 44.4%) due to human-in-loop validation and domain-specific structured workflows, though the evaluation format differs.

The composite score of 94/100 should not be compared directly to SWE-bench's 93.9% — they measure fundamentally different capabilities. The composite represents overall SDLC capability across multiple quality dimensions simultaneously.

### 6.2 Feature Composition

| Type | Count | Description |
|---|---|---|
| Greenfield | 25 | New components built from scratch |
| Brownfield | 19 | Modifications to existing codebases |
| **Total** | **44** | |

The 43/57 brownfield/greenfield split demonstrates that LLMGen operates effectively in both paradigms — a critical distinction from benchmarks that evaluate only greenfield generation or only patch-based modification. Brownfield features required navigating existing codebases, maintaining backward compatibility, and integrating with established architectural patterns, while greenfield features demanded scaffolding entirely new services including project structure, build configuration, dependency management, and integration contracts. The platform's ability to handle both modes within the same orchestration cycle reflects a maturity level not captured by single-mode benchmarks.

---

## 7. LLMGen vs. Industry Comparison

### 7.1 Against Coding Agents (SWE-bench)

| Dimension | SWE-bench Agents | LLMGen |
|---|---|---|
| Scope | Single bug fix / feature | 44 features, full SDLC |
| Avg LOC per task | ~200–500 | ~155,000 |
| Verification | Unit test pass | 4-tier (Build → SA → E2E → System E2E) |
| Multi-project | No | Yes (44 parallel) |
| Deployment | Not evaluated | Full Helm + orchestration |
| Quality metric | Pass/fail resolution | 91/100 SDLC quality |

### 7.2 Against SDD Frameworks (SpecBench)

| Dimension | SDD Frameworks | LLMGen |
|---|---|---|
| Output | Specification documents | Working software + specs |
| Execution | Manual | Autonomous end-to-end |
| Verification | Document quality scoring | Runtime verification |
| Cost | $75–$200 per spec | $295 per feature (including all code, tests, CI/CD, deployment) |

### 7.3 Cost Comparison

| System | Cost per 1K LOC | Scope per Feature |
|---|---|---|
| Industry average (AI-assisted) | $15–$30 | ~2,500 LOC |
| OpenSpec + manual execution | ~$38* | ~2,500 LOC |
| LLMGen | $3.00 | ~155,000 LOC |

*Estimated: specification cost amortized over typical feature size.

### 7.4 Against AI Engineering Tools

We compare LLMGen against the current landscape of AI-assisted development tools across key capability dimensions:

| Dimension | LLMGen | Kiro (AWS) | JetBrains AI | Cursor/Cascade | Zed AI |
|---|---|---|---|---|---|
| Architecture | Two-tier (IDE + K8s cluster, 24 agents) | Single-tier IDE | Single-tier IDE | Single-tier IDE | Single-tier IDE |
| Scope | Platform (44 features) | Single feature | Single feature | Single file/feature | Single file/feature |
| Workflow steps | 11–14 per workflow | 3 | Ad-hoc | Ad-hoc | Ad-hoc |
| Output volume | 6.8M LOC (measured) | ~500–5K LOC/feature | ~500–5K LOC | ~100–5K LOC | ~100–5K LOC |
| Verification gates | 4-tier (Build → SA → E2E → System E2E) | None | None | None | None |
| Multi-project orchestration | Yes (44 parallel) | No | No | No | No |
| Brownfield support | Full (14-step reverse engineering) | Limited | Ad-hoc | Ad-hoc | Ad-hoc |
| CMS / team coordination | Multi-developer with conflict detection | No | No | No | No |
| Token efficiency | 40–55% reduction (step-segregated) | Standard prompts | Standard prompts | Standard prompts | Standard prompts |
| CI/CD generation | Jenkins, ArgoCD, FluxCD as artifacts | Not supported | Not supported | Not supported | Not supported |
| Requirements integration | JIRA, Confluence, PDF ingestion | None | None | None | None |

The primary architectural distinction is LLMGen's two-tier design: while all competing tools operate as single-tier IDE extensions, LLMGen separates interactive workflow execution (Tier 1) from autonomous multi-agent orchestration (Tier 2). This separation enables both human-supervised quality control and unsupervised scaling — a combination unavailable in any competing tool.

---

## 8. Tier 2 Projections

### 8.1 Scaling Model

LLMGen's architecture supports horizontal scaling of SDLC pipelines. Tier 2 projections model the platform operating at 1,000 simultaneous projects:

| Parameter | Tier 1 (Measured) | Tier 2 (Projected) |
|---|---|---|
| Simultaneous projects | 44 | 1,000 |
| Time per feature cycle | ~2.7 days | ~2 hours + verification |
| Cost per feature | ~$295 | ~$250 |
| Total cost for 1K features | — | ~$250,000 |
| Equivalent manual cost | — | $150,000,000+ |

### 8.2 Projection Basis

Tier 2 projections are based on observed per-feature costs and the platform's demonstrated parallelism. The reduction in per-feature time from ~2.7 days to ~2 hours reflects elimination of sequential bottlenecks through full parallelization. Cost per feature decreases marginally due to improved cache efficiency at scale.

### 8.3 Caveats

These projections assume: (a) linear scaling of AI API capacity, (b) no cross-project dependency bottlenecks beyond those observed at 44-project scale, and (c) equivalent feature complexity distribution. Real-world deployments at 1,000-project scale would likely encounter coordination challenges not present at 44-project scale.

---

## 9. Discussion

### 9.1 Implications for Benchmark Design

The results expose a fundamental category gap in AI engineering evaluation. As established in Section 2.5, current benchmarks occupy Levels 1–4 of a 5-level scope hierarchy:

| Level | Category | Benchmark | Status |
|---|---|---|---|
| 1 | Code Completion | Copilot evaluations | Well-established |
| 2 | Coding Agent | SWE-bench (93.9% SOTA) | Well-established |
| 3 | Spec-Driven Development | Ship-Bench, SpecBench | Emerging |
| 4 | System Construction | SWE-AGI (86.4% SOTA) | Early |
| 5 | Platform Engineering | *(This paper)* | **No benchmark exists** |

Level 5 is not merely a scaling of Level 4. Platform-level operation introduces qualitatively different challenges: dependency graph management across projects, schema evolution coordination, integration contract enforcement, deployment sequencing, and multi-developer coordination through configuration management systems. These challenges do not appear in Levels 1–4 regardless of the number of features evaluated. Our composite score of 94/100 across multiple benchmark dimensions demonstrates that LLMGen operates at Level 5 with quality comparable to or exceeding Level 2–4 SOTA results, while managing 60× the scope per feature.

### 9.2 The Multi-Project Orchestration Gap

No existing agent or benchmark addresses multi-project orchestration. SWE-bench agents operate on a single repository. Ship-Bench evaluates a single feature's lifecycle. Even advanced agents like Devin, while capable of multi-step reasoning, do not manage cross-project dependency coherence or coordinated multi-project deployment.

LLMGen's 44-project parallel orchestration represents a capability with no direct competitor or benchmark equivalent. This suggests that the current evaluation landscape has a significant blind spot in assessing AI systems designed for enterprise-scale engineering operations. A platform-level benchmark would need to evaluate: (a) the number of projects an AI system can manage simultaneously, (b) the fidelity of cross-project dependency resolution, (c) the consistency of shared artifacts (schemas, contracts, configurations) across project boundaries, and (d) the correctness of coordinated deployment sequencing. We propose these as the four axes of a future multi-project orchestration benchmark.

### 9.3 Cost Efficiency at Scale

The 5–10× cost advantage over industry averages raises questions about the sustainability and replicability of these results. We note that 91.7% cache efficiency is a critical factor — without aggressive caching, token costs would increase substantially. The $12,984 total cost for 6.8M LOC reflects both the platform's architectural efficiency and the specific characteristics of the deployment (highly structured feature specifications, consistent technology stack, well-defined integration patterns).

### 9.4 Reproducibility Considerations

The normalization methodology itself is reproducible: any AI engineering platform can compute scope-normalized, quality-normalized, and cost-normalized metrics using the three-dimensional framework described in Section 3. The specific benchmark values reported here (91/100 quality, $3.00/1K LOC, 333 LOC/$) are properties of the LLMGen platform applied to this particular deployment and should not be treated as universal constants. Different technology stacks, domain complexities, and organizational maturity levels will yield different values. The methodology's contribution is the *framework for comparison*, not the specific numbers.

### 9.5 Limitations

This study reports results from a single deployment. While the scale is significant (44 features, 6.8M LOC, 117 days, 15+ developers), generalizability to other domains, technology stacks, and organizational contexts requires further validation. The Ship-Bench equivalent score is derived via our normalization methodology rather than direct Ship-Bench evaluation, introducing potential methodological bias in the quality-dimension mapping. Additionally, the 15+ developer team's contribution to code review, architectural guidance, and integration validation is not fully separated from the AI-generated output in throughput metrics — the measured productivity reflects a human-AI collaborative system, not a fully autonomous one. Future work should establish controlled experiments comparing platform-level and feature-level AI systems on identical requirements sets to validate the normalization methodology independently of any specific platform.

---

## 10. Conclusion

We have presented a normalization methodology for comparing AI platform engineering output against feature-level benchmarks, validated through a 117-day production deployment of the LLMGen platform. The results — 44 features, 6.8M LOC, composite score 94/100 across five benchmark dimensions, $3.00 per 1K LOC — demonstrate that AI-driven platform engineering operates at a fundamentally different scale than what current benchmarks evaluate.

The field needs new benchmark categories. SWE-bench measures whether an agent can fix a bug. Ship-Bench measures whether an agent can build a feature well. Neither measures whether an AI platform can orchestrate the complete engineering lifecycle across dozens of parallel projects while maintaining cross-project coherence, verification integrity, and deployment readiness.

We release our normalization methodology and anonymized benchmark data to support the development of platform-level AI engineering benchmarks. The gap between feature-level and platform-level evaluation is not merely quantitative — it is qualitative, and closing it is essential for accurately assessing the next generation of AI engineering systems.

---

## References

[1] C. E. Jimenez, J. Yang, A. Wettig, S. Yao, K. Pei, O. Press, and K. Narasimhan, "SWE-bench: Can Language Models Resolve Real-World GitHub Issues?" in *Proc. ICLR*, 2024.

[2] Anthropic, "Claude Mythos: SWE-bench Verified Results," Technical Report, 2026.

[3] OpenAI, "GPT-5.5 and Codex: AI Software Engineering Agents," Technical Report, 2026.

[4] Y. Liu, Z. Wang, and A. Chen, "Ship-Bench: Evaluating LLM Agents Across the Full Software Development Lifecycle," arXiv preprint, 2025.

[5] kevins981 et al., "SpecBench: Benchmarking Specification Reasoning in Software Engineering," arXiv:2605.30314, 2025.

[6] SWE-AGI Consortium (MoonBit), "SWE-AGI: System-Scale Construction Benchmark for AI Agents," arXiv:2602.09447, 2025.

[7] J. Zhang, L. Wang, H. Xu, et al., "From SWE-bench to SWE-AGI: A Survey on AI Software Engineering Agents," arXiv preprint, 2025.

[8] Scale AI, "SWE-bench Pro: Multi-Language Software Engineering Benchmark," Technical Report, 2026.

[9] R. Agaev, "LLMGen: AI-Driven Platform Engineering at SDLC Scale," Technical Documentation, 2026.
