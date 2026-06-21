# LLMGen Benchmark — Publishing Guide

**Author:** Roman Agaev  
**Date:** June 21, 2026  
**Purpose:** Step-by-step instructions for publishing the LLMGen benchmark across all channels.

---

## Publishing Order (Follow This Sequence)

The order matters — each step creates a link that the next step references.

```
Step 1: GitHub repository (creates permanent links)
   ↓
Step 2: arXiv paper (references GitHub repo)
   ↓
Step 3: LinkedIn article (references arXiv + GitHub)
   ↓
Step 4: Dev.to / Hashnode (references arXiv + GitHub)
   ↓
Step 5: Reddit (references arXiv + GitHub + LinkedIn)
   ↓
Step 6: X/Twitter thread (references all above)
```

---

## Step 1: GitHub Repository

**When:** First — do this before everything else.  
**Time needed:** 30 minutes  
**URL will be:** `github.com/roman-agaev/llmgen-benchmark`

### Instructions:

1. Go to https://github.com/new
2. Repository name: `llmgen-benchmark`
3. Description: "Benchmarking AI-Driven Platform Engineering Beyond SWE-bench — Methodology and Results"
4. Set to **Public**
5. Initialize with README — **No** (you'll push your own)
6. License: **MIT** (or Apache 2.0)
7. Create repository

### Upload files:

```bash
mkdir llmgen-benchmark
cd llmgen-benchmark
git init

# Copy these files from benchmark-exposure/:
# - github-readme.md → rename to README.md
# - arxiv-paper.md → put in docs/paper.md
# - llmgen-benchmark-methodology-step-by-step.md (from llm-generation-design/docs/comparisons/ — SCRUB Nokia refs first)
# - llmgen-swe-sdd-benchmark-2026.md (from llm-generation-design/docs/comparisons/ — SCRUB Nokia refs first)

cp github-readme.md README.md
mkdir docs
cp arxiv-paper.md docs/paper.md

git add .
git commit -m "Initial release: LLMGen benchmark methodology and results"
git branch -M main
git remote add origin git@github.com:roman-agaev/llmgen-benchmark.git
git push -u origin main
```

### After pushing:

- Add topics: `ai-benchmark`, `swe-bench`, `platform-engineering`, `sdlc`, `llm`, `ai-coding`
- Add a release: v1.0.0 with title "LLMGen Benchmark — June 2026"
- Note the URL — you'll need it for all subsequent steps

### Before publishing — SCRUB CHECK:

```bash
grep -ri "nokia\|AN DMP\|DataOps\|AT&T\|Dish\|Bain\|Keystone\|SCDEF\|DFSEC" .
```

If ANY match is found, fix it before pushing.

---

## Step 2: arXiv Paper

**When:** After GitHub repo is live (same day or next day).  
**Time needed:** 1-2 hours (formatting + submission)  
**URL will be:** `arxiv.org/abs/26XX.XXXXX`

### Instructions:

1. Go to https://arxiv.org/user/register (if no account)
2. Start new submission: https://arxiv.org/submit
3. **Category:** `cs.SE` (Software Engineering) — primary. Cross-list: `cs.AI`
4. **Title:** "Beyond SWE-bench: Benchmarking AI-Driven Platform Engineering at SDLC Scale"
5. **Authors:** Roman Agaev
6. **Abstract:** Copy from the paper (first 200 words)

### Format requirements:

arXiv accepts PDF. Convert the markdown paper to PDF:

Option A — Use Pandoc:
```bash
pandoc docs/paper.md -o llmgen-benchmark-paper.pdf --pdf-engine=xelatex -V geometry:margin=1in
```

Option B — Use a LaTeX template:
- Convert markdown to LaTeX using pandoc: `pandoc docs/paper.md -o paper.tex`
- Use the `article` or `IEEEtran` document class
- Compile with `pdflatex paper.tex`

Option C — Use Overleaf:
- Go to https://overleaf.com
- Create new project → paste content → compile to PDF

7. Upload the PDF
8. Submit — arXiv reviews in 1-3 business days, then assigns a permanent ID
9. Once published, update GitHub README with the arXiv link

### Update GitHub after arXiv:

Replace `[arXiv paper link]` in README.md with the actual arXiv URL.

---

## Step 3: LinkedIn Article

**When:** 1-2 days after arXiv is live (so you can link to it).  
**Time needed:** 30 minutes  
**URL will be:** `linkedin.com/pulse/...`

### Instructions:

1. Go to LinkedIn → "Write article" (not a post — full article)
2. **Title:** "Why SWE-bench Can't Measure Platform Engineering — And What Can"
3. Copy content from `linkedin-article.md`
4. **Before pasting — update links:**
   - Replace `[GitHub repository link]` → actual GitHub URL
   - Replace `[arXiv paper link]` → actual arXiv URL
5. Format in LinkedIn editor:
   - Use H2 for section headers
   - Use the table feature for comparison tables (or paste as images)
   - Bold key numbers
6. Add cover image: Create a simple graphic with "333 LOC/$ | 94/100 SDLC | 5-Level Benchmark" (use Canva or similar)
7. **Tags/hashtags:** `#AIEngineering #SWEbench #PlatformEngineering #SDLC #AIBenchmark #SoftwareArchitecture`
8. Publish

### Timing tip:

Post on **Tuesday or Wednesday morning** (US time zones) for maximum LinkedIn reach. Avoid weekends and Mondays.

---

## Step 4: Dev.to / Hashnode

**When:** Same day as LinkedIn or 1 day after.  
**Time needed:** 20 minutes per platform  

### Dev.to Instructions:

1. Go to https://dev.to → sign in (GitHub login works)
2. "Create Post"
3. **Title:** "I Built an AI Platform That Delivers 333 LOC Per Dollar — Here's How I Benchmarked It"
4. Copy content from `devto-hashnode-post.md`
5. **Update links** to GitHub and arXiv
6. **Tags:** `ai`, `machinelearning`, `devops`, `benchmark` (max 4 on Dev.to)
7. **Cover image:** Same as LinkedIn or a code-themed image
8. Publish

### Hashnode Instructions:

1. Go to https://hashnode.com → create blog (if needed)
2. "Write a Story"
3. Same title and content as Dev.to
4. **Tags:** `AI`, `Machine Learning`, `Software Engineering`, `Benchmark`
5. Publish

### Cross-posting note:

Use the `canonical_url` feature on both platforms pointing to your GitHub repo to avoid SEO duplication.

---

## Step 5: Reddit

**When:** 1-2 days after LinkedIn (let the article get some traction first).  
**Time needed:** 15 minutes  

### Instructions:

1. Post to **r/MachineLearning**:
   - Title: `[P] LLMGen: Benchmarking AI platform engineering beyond SWE-bench — 94/100 composite, 333 LOC/$`
   - Content: Copy from `reddit-post.md`
   - Update all links
   - Flair: `[Project]`

2. Post to **r/ChatGPTCoding** (same day or next):
   - Title: `I built an AI engineering platform that scores 94/100 on a normalized SWE/SDD benchmark — here's the methodology`
   - Shorter version of the content
   - More developer-focused

3. Post to **r/LocalLLaMA** (optional):
   - Title: `Platform-level AI engineering benchmark: why SWE-bench can't measure full SDLC orchestration`
   - Focus on the methodology angle

### Reddit tips:

- Post between **9-11 AM EST** on weekdays for maximum visibility
- Respond to every comment in the first 2 hours
- Don't self-promote in comments — answer technical questions
- If the post gains traction, edit to add the arXiv link at the top

---

## Step 6: X/Twitter Thread

**When:** Same day as Reddit or 1 day after.  
**Time needed:** 15 minutes  

### Instructions:

1. Open X/Twitter
2. Copy tweets from `x-twitter-thread.md`
3. Post as a thread (write first tweet, then reply to yourself for each subsequent tweet)
4. **Update links** in the final tweet
5. **Tag relevant accounts** in tweet 1 or 2:
   - @OpenAI (Codex)
   - @AnthropicAI (Claude Code)
   - @cursor_ai (Cursor)
   - @cognition_labs (Devin)
   - @awscloud (Kiro)
   - @princeton_nlp (SWE-bench creators)
6. Pin the thread to your profile

### Timing:

Post between **10 AM - 12 PM EST** on Tuesday-Thursday for maximum tech engagement.

---

## Pre-Publication Checklist

Before publishing ANYTHING, verify:

- [ ] **Confidentiality scan passed** — zero matches for Nokia, AN DMP, DataOps, AT&T, Dish, Bain, Keystone, SCDEF, DFSEC, or any internal names
- [ ] **No Nokia email addresses** in any file (roman.agaev@nokia.com → remove or use personal email)
- [ ] **No internal URLs** — no scm.cci.nokia.net, no confluence.ext.net.nokia.com links
- [ ] **No Cursor billing CSV** with Nokia email — if referenced, anonymize
- [ ] **Repository names** — no "dataops-analysis" or "dataOps" references
- [ ] **Person names** — no Nokia colleagues named (Eadan, Zeev, Vijay, Dharma, etc.)
- [ ] **All links updated** — GitHub URL, arXiv URL filled in
- [ ] **Author contact** — use personal email, not Nokia email
- [ ] **Grammar check** — run through Grammarly or similar before publishing

---

## Recommended Timeline

| Day | Action |
|-----|--------|
| **Day 1 (Mon/Tue)** | Create GitHub repo, push code, add topics/release |
| **Day 1** | Submit arXiv paper (takes 1-3 days to process) |
| **Day 3-4 (Wed/Thu)** | arXiv should be live; publish LinkedIn article |
| **Day 3-4** | Publish Dev.to + Hashnode posts |
| **Day 4-5 (Thu/Fri)** | Post on Reddit (r/MachineLearning, r/ChatGPTCoding) |
| **Day 4-5** | Post X/Twitter thread |
| **Day 7+** | Monitor engagement, respond to comments, share across platforms |

---

## After Publishing — Amplify

1. **Comment on related posts** — When someone discusses SWE-bench results, Kiro, or AI coding benchmarks, add a thoughtful comment linking to your methodology
2. **Update LinkedIn headline** — Add "Author: Beyond SWE-bench benchmark" or similar
3. **GitHub stars** — Share the repo in relevant Discord/Slack communities (MLOps, AI Engineering)
4. **Follow-up content** — Write a short follow-up post when you get feedback or questions
5. **Connect with people who engage** — Anyone who comments or shares is a potential contact

---

## Emergency: If Nokia Objects

If Nokia claims the benchmark data is confidential:

1. **The methodology is yours** — You created the normalization framework. Methodologies are not confidential.
2. **The aggregated/anonymized results are publishable** — No customer names, no internal system names, no proprietary data.
3. **The industry comparison data is public** — SWE-bench scores, Kiro/SpecKit data, all from published sources.
4. **What IS Nokia's:** Specific repository URLs, Cursor billing CSV with Nokia email, customer project details, internal org names.
5. **If challenged:** Remove any specific data point they object to; the methodology and framework stand on their own.

---

*This guide is for Roman Agaev's personal use. Do not publish this document.*
