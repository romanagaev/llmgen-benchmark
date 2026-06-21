# SSRN Submission — Metadata & Instructions

**Platform:** SSRN (Social Science Research Network, owned by Elsevier)  
**URL:** https://www.ssrn.com  
**Advantage:** No endorsement required. Assigns a DOI. Wide academic reach. Fast publication. Indexed by Google Scholar.

---

## Submission Metadata

**Title:** Beyond SWE-bench: Benchmarking AI-Driven Platform Engineering at SDLC Scale

**Authors:** Roman Agaev

**Affiliation:** Independent Researcher

**Contact Email:** roman.agaev@zhiongroup.com

**Abstract:**
Current benchmarks for AI-assisted software engineering — SWE-bench, Ship-Bench, and SpecBench — evaluate agents at the granularity of individual bug fixes, single-feature implementations, or specification quality. While valuable, these benchmarks fail to capture the operational reality of platform engineering, where AI systems must orchestrate full software development lifecycle (SDLC) pipelines across dozens of parallel projects simultaneously. We present a normalization methodology that enables comparison between platform-level AI engineering output and feature-level benchmark scores. We validate this methodology using LLMGen, a full-stack AI engineering platform that autonomously manages requirements analysis, architectural design, code generation, testing, CI/CD pipeline construction, and deployment orchestration. In a measured deployment spanning 117 days across a production Kubernetes-native platform (25+ microservices), LLMGen completed 44 features (25 greenfield, 19 brownfield) comprising ~6.8 million lines of code at a cost of $12,984 — yielding 333 LOC per dollar, approximately 6.7x the industry average cost efficiency. We demonstrate that LLMGen achieves a composite benchmark score of 94/100 (weighted normalized) and a Ship-Bench equivalent SDLC quality score of 91/100 while operating at a scope 60x larger than typical AI-assisted feature implementations.

**Keywords:** AI software engineering, benchmark methodology, platform engineering, SDLC automation, LLM-driven development

**JEL Classification:** O33 (Technological Change), L86 (Information and Internet Services)

**Network:** Computer Science Research Network (CompSciRN)

**File to upload:** paper.pdf (from llmgen-benchmark/docs/paper.pdf)

---

## Step-by-Step Submission Instructions

1. Go to https://www.ssrn.com
2. Click **"Submit a Paper"** or go to https://hq.ssrn.com/submissions/CreateNewAbstract.cfm
3. Create account or sign in
4. Select Network: **CompSciRN** (Computer Science Research Network)
5. Fill in:
   - Title: as above
   - Authors: Roman Agaev, Independent Researcher
   - Abstract: copy from above
   - Keywords: copy from above
   - JEL codes: O33, L86
6. Upload **paper.pdf**
7. Submit
8. SSRN typically publishes within **24-72 hours**
9. You'll receive a permanent SSRN URL and abstract page
10. Update GitHub README and all articles with the SSRN link

---

## After Publication

- Update all link placeholders across published content
- SSRN tracks downloads and citations — useful for demonstrating impact
- Papers on SSRN are indexed by Google Scholar within ~1-2 weeks
