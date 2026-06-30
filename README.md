# llmgen-benchmark

[![Medium](https://img.shields.io/badge/Medium-Article-12100E?logo=medium&logoColor=white)](https://medium.com/@romanagaev/i-benchmarked-my-ai-engineering-platform-against-swe-bench-1b98b1592cc2)
[![Dev.to](https://img.shields.io/badge/Dev.to-Article-0A0A0A?logo=devdotto&logoColor=white)](https://dev.to/romanagaev/i-built-an-ai-platform-that-delivers-333-loc-per-dollar-heres-how-i-benchmarked-it-1oao)
[![Hashnode](https://img.shields.io/badge/Hashnode-Blog-2962FF?logo=hashnode&logoColor=white)](https://llmgen.hashnode.dev/i-built-an-ai-platform-that-delivers-333-loc-per-dollar-here-s-how-i-benchmarked-it)
[![arXiv](https://img.shields.io/badge/arXiv-2025.XXXXX-b31b1b.svg)](https://arxiv.org/abs/2025.XXXXX)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Benchmark](https://img.shields.io/badge/benchmark-platform--level-blue.svg)](#)

**A normalization methodology for benchmarking AI-driven platform engineering against feature-level benchmarks like SWE-bench and Ship-Bench.**

---

## Key Results

| Metric | Value |
|---|---|
| **Composite benchmark score** | **94/100** (weighted across 5 dimensions) |
| Features completed | 44 (25 greenfield, 19 brownfield) |
| Total LOC | ~6,800,000 |
| SDLC quality score | 91/100 (Ship-Bench equivalent) |
| SpecBench analogous score | ~70% (vs SOTA 44.4%) |
| Cost per 1K LOC | $3.00 (5–10× cheaper than industry) |
| LOC per dollar | 333 (6.7× industry average) |
| Token efficiency | 40–55% reduction vs conversational approaches |
| Throughput | 407K LOC/week, 2.6 features/week |
| Multi-project orchestration | 44 features parallel |
| Feature completion rate | 100% |

---

## Why This Benchmark Exists

AI engineering benchmarks form a 5-level scope hierarchy. **Level 5 has no benchmark.**

| Level | Category | Benchmark | Scope | SOTA |
|---|---|---|---|---|
| 1 | Code Completion | Copilot | Single line/function | — |
| 2 | Coding Agent | SWE-bench | Single issue, 50–500 LOC | Claude Mythos: 93.9% |
| 3 | Spec-Driven Dev | Ship-Bench, SpecBench | Single feature, 500–5K LOC | Kiro: ~95% first-try |
| 4 | System Construction | SWE-AGI | Full system, 1K–10K LOC | 86.4% (GPT-5.3-Codex) |
| **5** | **Platform Engineering** | **None** | **Multi-project, 10⁴–10⁶ LOC** | **LLMGen: 94/100** |

Levels 1–4 don't measure **platform engineering** — orchestrating full SDLC pipelines across dozens of parallel projects with cross-project coherence, 4-tier verification, multi-developer coordination, and deployment orchestration.

```
Levels 1-4:       1 bug fix    │  1 feature   │  1 system
                  ─────────────┼──────────────┼─────────────────
Level 5:          44 features  │  6.8M LOC    │  Full SDLC × N projects
                  parallel     │  117 days    │  Requirements → Deployment
```

This repository provides the **normalization methodology** to bridge the Level 4→5 gap — enabling direct comparison between platform-level output and feature-level benchmark scores.

---

## Methodology Overview

We normalize across three dimensions:

### 1. Scope Normalization
Platform output → per-feature metrics for comparison with feature-level benchmarks.

> 6.8M LOC ÷ 44 features = **~155K LOC/feature** vs industry avg **~2.5K LOC/feature** (60× more scope)

### 2. Quality Normalization
Map the platform's verification pipeline to Ship-Bench SDLC quality categories.

| Ship-Bench Category | Verification Mapping | Score |
|---|---|---|
| Requirements Understanding | Feature spec generation + alignment | 93 |
| Code Quality | Static analysis across 22,359 files | 90 |
| Test Coverage | Project-level E2E generation + pass rate | 90 |
| Deployment Readiness | System-level E2E + orchestration verification | 92 |
| **Weighted Average** | | **91** |

### 3. Cost Normalization
Cost per 1K LOC enables cross-system comparison regardless of absolute scale.

---

## Results Summary

### LLMGen vs. Coding Agents

| Dimension | SWE-bench Agents | LLMGen |
|---|---|---|
| Scope | Single bug fix | 44 features, full SDLC |
| Avg LOC per task | ~200–500 | ~155,000 |
| Verification | Unit test pass | 4-tier (Build → SA → E2E → System E2E) |
| Multi-project | No | Yes (44 parallel) |
| Quality metric | Pass/fail | 91/100 SDLC quality |

### LLMGen vs. SDD Frameworks

| Framework | Score | Cost | Executes Code? |
|---|---|---|---|
| OpenSpec | 4.00/5 | ~$95 | No |
| BMAD Quick | 3.74/5 | ~$75 | No |
| BMAD Full | 3.65/5 | ~$200 | No |
| SpecKit | 2.77/5 | ~$75 | No |
| **LLMGen** | **91/100** | **$295/feature** | **Yes — full SDLC** |

### Cost Comparison

| System | Cost per 1K LOC | Scope per Feature |
|---|---|---|
| Industry average | $15–$30 | ~2,500 LOC |
| **LLMGen** | **$3.00** | **~155,000 LOC** |

### Composite Score Derivation (94/100)

| Benchmark Dimension | Score | Weight | Correlation |
|---|---|---|---|
| SWE-bench equivalent (feature completion) | 100% | 0.25 | 0.6 |
| Ship-Bench (SDLC quality) | 91.8% | 0.30 | 0.8 |
| SpecBench analogous (spec reasoning) | ~70% | 0.15 | 0.5 |
| SWE-AGI equivalent (system construction) | 100% | 0.20 | 0.7 |
| Cost efficiency (vs industry) | 6.7× | 0.10 | 0.9 |

> Composite = Σ(score × weight × correlation) / Σ(weight × correlation) = **94/100**

### Deployment Data

| Parameter | Value |
|---|---|
| Duration | 117 days |
| Commits | 1,350 |
| Files | 22,359 |
| API requests | 11,139 |
| Total AI cost | $12,984 |
| Tokens | 12.6B |
| Cache efficiency | 91.7% |
| Effective cost/1M tokens | $1.03 |
| Estimated cost without cache | ~$189,000 |
| Team size | 15+ developers |

---

## How to Use This Methodology

The normalization framework can be applied to any AI engineering platform:

1. **Measure raw output** — total LOC, features, commits, duration, cost
2. **Normalize scope** — divide total output by feature count; compare against industry ~2.5K LOC/feature baseline
3. **Map quality** — align your verification pipeline to Ship-Bench categories; score each dimension
4. **Normalize cost** — compute cost per 1K LOC; compare against $15–$30 industry range
5. **Report multi-project capability** — document parallel project count, cross-project verification

### Verification Tier Mapping

Map your verification pipeline to the 4-tier model:

| Tier | Description | Your Equivalent |
|---|---|---|
| Tier 1: Build | Compilation, dependency resolution | ___ |
| Tier 2: Static Analysis | Linting, type checking, security scanning | ___ |
| Tier 3: Project E2E | End-to-end tests within project scope | ___ |
| Tier 4: System E2E | Cross-project integration validation | ___ |

---

## Industry Comparison

### SWE-bench Verified Leaderboard (June 2026)

| Agent | SWE-bench Verified | SWE-bench Pro |
|---|---|---|
| Claude Mythos | 93.9% | — |
| GPT-5.5 | 88.7% | — |
| Claude Opus 4.8 | 88.6% | — |
| Claude Fable 5 | — | 80.3% |
| Cursor Agent | ~67% | — |
| Devin | ~58% | — |

> These agents solve individual issues (Level 2). LLMGen operates at Level 5 — full SDLC orchestration across 44 parallel features — making direct percentage comparison non-applicable. The normalization methodology in this repository bridges this evaluation gap.

### LLMGen vs. AI Engineering Tools

| Capability | LLMGen | Kiro (AWS) | JetBrains AI | Cursor | Zed AI |
|---|---|---|---|---|---|
| Architecture | Two-tier (IDE + K8s, 24 agents) | Single-tier | Single-tier | Single-tier | Single-tier |
| Scope | 44 features, 6.8M LOC | 1 feature | 1 feature | 1 file/feature | 1 file/feature |
| Verification | 4-tier (Build→SA→E2E→System) | None | None | None | None |
| Multi-project | Yes (44 parallel) | No | No | No | No |
| Token efficiency | 40–55% reduction | Standard | Standard | Standard | Standard |
| Brownfield support | Full (14-step) | Limited | Ad-hoc | Ad-hoc | Ad-hoc |
| CI/CD generation | Jenkins, ArgoCD, FluxCD | No | No | No | No |
| CMS coordination | Multi-developer | No | No | No | No |
| Requirements sources | JIRA, Confluence, PDF | None | None | None | None |

---

## Publications

- **Medium:** [I Benchmarked My AI Engineering Platform Against SWE-bench — Here's Why Existing Benchmarks Don't Apply Above Level 3](https://medium.com/@roman.agaev/i-benchmarked-my-ai-engineering-platform-against-swe-bench-heres-why-existing-benchmarks-don-t-1b98b1592ec2)
- **Dev.to:** [I Built an AI Platform That Delivers 333 LOC Per Dollar — Here's How I Benchmarked It](https://dev.to/romanagaev/i-built-an-ai-platform-that-delivers-333-loc-per-dollar-heres-how-i-benchmarked-it-1oao)
- **Hashnode:** [I Built an AI Platform That Delivers 333 LOC Per Dollar — Here's How I Benchmarked It](https://llmgen.hashnode.dev/i-built-an-ai-platform-that-delivers-333-loc-per-dollar-here-s-how-i-benchmarked-it)
- **arXiv:** *(pending endorsement)*

---

## Citation

If you use this methodology or data in your research, please cite:

```bibtex
@misc{agaev2026beyond,
  title={Beyond SWE-bench: Benchmarking AI-Driven Platform Engineering at SDLC Scale},
  author={Agaev, Roman},
  year={2026},
  howpublished={\url{https://github.com/romanagaev/llmgen-benchmark}},
  note={Methodology and results available at GitHub repository}
}
```

---

## Author

**Roman Agaev** — Creator and Architect of LLMGen | Benchmark Methodology Author

Designed and built the LLMGen platform (two-tier architecture: IDE extension + K8s multi-agentic cluster with 24 agents), created the SDLC orchestration workflows, and authored the normalized benchmark methodology that maps platform-level AI engineering to established industry standards.

LLMGen's Tier 2 multi-agentic architecture — designed for 1000x parallelism — remains in development. Seeking the right environment to bring this vision to production scale. Open to conversations with organizations interested in AI-driven platform engineering at enterprise scope.

---

## License

MIT License. See [LICENSE](LICENSE) for details.

Benchmark data is anonymized. No proprietary or confidential information is included.
