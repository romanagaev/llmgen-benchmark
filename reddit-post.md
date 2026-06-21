# [P] LLMGen: Benchmarking AI platform engineering beyond SWE-bench — 333 LOC/$ at 94/100 composite score

## TL;DR

Built an AI-driven platform engineering system (LLMGen) that delivers full lifecycle software — requirements through deployment — across multiple parallel projects. 44 completed features, 6.8M LOC, 117 days, 94/100 composite score. No existing benchmark measures this scope. Proposing a 5-level benchmark hierarchy where LLMGen sits at Level 5 (Platform Engineering) — a level with no existing evaluation framework.

## The 5-Level Benchmark Hierarchy

| Level | Category | Benchmark | Scope | SOTA |
|-------|----------|-----------|-------|------|
| 1 | Code Completion | Copilot | ~1-10 LOC | — |
| 2 | Coding Agent | SWE-bench | 50-500 LOC | Claude Mythos 93.9% |
| 3 | Spec-Driven Dev | Ship-Bench | 500-5K LOC | Kiro 95% first try |
| 4 | System Construction | SWE-AGI | 1K-10K LOC | 86.4% |
| **5** | **Platform Engineering** | **None** | **6.8M LOC** | **No benchmark** |

All industry conversation happens at Levels 1-4. The most consequential AI engineering work (multi-project platform delivery) has no evaluation framework.

## Architecture

Two-tier, not single-tier like every other tool:
- **Tier 1**: IDE Extension — interactive step-by-step workflows with approval gates
- **Tier 2**: K8s Multi-Agentic Cluster — 24 autonomous agents, custom templates, parallel execution

This is why multi-project orchestration works. Kiro/Cursor/Devin are all single-tier.

## How LLMGen Differs from Everything Else

| Capability | LLMGen | Kiro | Cursor | Devin |
|------------|--------|------|--------|-------|
| Benchmark level | 5 | 3 | 2 | 2-3 |
| Multi-project | 44 parallel | No | No | No |
| 4-tier verification | Yes | No | No | No |
| Config Mgmt System | Yes | No | No | No |
| Cross-index RAG | Yes | No | No | No |
| Resume workflows | Yes | No | No | No |
| JIRA/Confluence | Native | No | No | No |
| Relationship | Orchestrates Cursor | Competitor to Cursor | — | Competitor to Cursor |

Key: LLMGen orchestrates Cursor as a component, does not compete with it.

## Key Results

| Metric | LLMGen | Industry / Benchmark |
|--------|--------|---------------------|
| Composite score | 94/100 | — |
| Cost per 1K LOC | $3.00 | $15-$30 |
| LOC per dollar | 333 | ~50 (6.7x) |
| LOC per feature | 150K | ~2.5K (60x) |
| Feature completion | 100% (44/44) | — |
| SDLC quality | 91/100 | — |
| Parallel orchestration | 44 projects | Unique capability |
| Token efficiency | 40-55% reduction | vs prompt-based |
| Total output | 6.8M LOC, 1,350 commits | — |
| Duration | 117 days | — |
| Tier 2 projection | 1,000 projects, ~2h, ~$250K | vs $150M+ traditional |

### Benchmark Context

- SWE-bench (Level 2): Claude Mythos 93.9%, GPT-5.5 88.7%, Cursor ~67%
- Ship-Bench (Level 3): Kiro 95% first try, OpenSpec 4.0/5, BMAD 3.74/5
- SWE-AGI (Level 4): Best 86.4%
- Platform Engineering (Level 5): No benchmark exists. LLMGen: 94/100.

## Token Efficiency

Step-segregated architecture: 40-55% fewer tokens than prompt-based development. Each step gets fresh context, no accumulation. Structured templates (-20%), step isolation (-25%), policy validation (-15%), prompt archiving (-10%).

## What's Next

- Full methodology paper: [arXiv placeholder]
- Benchmark framework and raw data: [GitHub placeholder]
- Tier 2 validation at 1,000-project scale

Happy to answer questions on methodology, take criticism on the 5-level hierarchy, or discuss what a Level 5 benchmark should look like.

---

*Author: Roman Agaev — creator and architect of LLMGen, author of the normalized benchmark methodology. Designed the platform, built it, then created the measurement framework when no existing benchmark could capture its scope.*

*Tier 2 (1000x parallelism, 24 autonomous agents on K8s) remains in development — looking for the right environment to bring it to production scale. Open to conversations.*
