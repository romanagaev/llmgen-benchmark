# TechRxiv Submission — Metadata & Instructions

**Platform:** TechRxiv (IEEE's preprint server)  
**URL:** https://www.techrxiv.org  
**Advantage:** No endorsement required. Assigns a DOI. IEEE-affiliated. Indexed by Google Scholar.

---

## Submission Metadata

**Title:** Beyond SWE-bench: Benchmarking AI-Driven Platform Engineering at SDLC Scale

**Authors:** Roman Agaev

**Affiliation:** Independent Researcher

**Corresponding Author Email:** roman.agaev@zhiongroup.com

**Abstract:**
Current benchmarks for AI-assisted software engineering — SWE-bench, Ship-Bench, and SpecBench — evaluate agents at the granularity of individual bug fixes, single-feature implementations, or specification quality. While valuable, these benchmarks fail to capture the operational reality of platform engineering, where AI systems must orchestrate full software development lifecycle (SDLC) pipelines across dozens of parallel projects simultaneously. We present a normalization methodology that enables comparison between platform-level AI engineering output and feature-level benchmark scores. We validate this methodology using LLMGen, a full-stack AI engineering platform that autonomously manages requirements analysis, architectural design, code generation, testing, CI/CD pipeline construction, and deployment orchestration. In a measured deployment spanning 117 days across a production Kubernetes-native platform (25+ microservices), LLMGen completed 44 features (25 greenfield, 19 brownfield) comprising ~6.8 million lines of code at a cost of $12,984 — yielding 333 LOC per dollar, approximately 6.7x the industry average cost efficiency. We demonstrate that LLMGen achieves a composite benchmark score of 94/100 (weighted normalized) and a Ship-Bench equivalent SDLC quality score of 91/100 while operating at a scope 60x larger than typical AI-assisted feature implementations. We argue that the field requires new benchmark categories that evaluate AI systems at the platform engineering lifecycle level rather than the isolated feature level.

**Keywords:** AI-assisted software engineering, benchmark normalization, platform engineering, SDLC automation, LLM-driven development, SWE-bench, software quality

**Categories:** 
- Primary: Software Engineering
- Secondary: Artificial Intelligence

**License:** CC BY 4.0

**File to upload:** paper.pdf (from llmgen-benchmark/docs/paper.pdf)

**Supplementary materials:** Link to GitHub repository https://github.com/romanagaev/llmgen-benchmark

---

## Step-by-Step Submission Instructions

1. Go to https://www.techrxiv.org
2. Click **"Submit a Preprint"** (top right)
3. Sign in with IEEE account or create one (free)
4. Fill in metadata:
   - Title: as above
   - Authors: Roman Agaev, Independent Researcher
   - Abstract: copy from above
   - Keywords: copy from above
   - Categories: Software Engineering (primary), Artificial Intelligence (secondary)
   - License: CC BY 4.0
5. Upload **paper.pdf**
6. In "Supplementary Materials" or "Related Links": add GitHub repo URL
7. Review and submit
8. TechRxiv typically publishes within **24-48 hours** (no peer review for preprints)
9. You'll receive a **DOI** (Digital Object Identifier) — permanent, citable reference
10. Update GitHub README and all articles with the DOI link

---

## After Publication

- Update all link placeholders in linkedin-article.md, devto-hashnode-post.md, reddit-post.md, x-twitter-thread.md
- Update GitHub README.md BibTeX citation with the DOI
- TechRxiv preprints are indexed by Google Scholar within ~1 week
