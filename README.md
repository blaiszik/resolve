<p align="center">
  <img src="resolve.png" alt="Resolve Logo" width="360" height="360">
</p>

<h1 align="center">Resolve</h1>

<p align="center">
  <strong>The open registry for fixing the repeatable failures in AI, science, engineering, and other research workflows.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
  <img src="https://img.shields.io/badge/issues-16_open-red.svg" alt="16 Open Issues">
  <img src="https://img.shields.io/badge/resolved-0-green.svg" alt="0 Resolved">
</p>

---

Researchers and engineers lose weeks each year to problems that should take minutes: broken environments, invisible dataset drift, stalled training runs, and last-minute figure rejections. These are often not hard problems. They are *unowned* problems, and we aim to resolve them.

**Resolve catalogs high-leverage, well-scoped issues across AI and scientific research workflows, estimates the time they waste, and tracks solutions when they appear.** Each issue is proposed in a way to be 1) actionable to the open source community, 2) buildable in a few days, and 3) remove a recurring source of friction.

This repo does **not** host the tools themselves. It provides the shared backlog the ecosystem needs, and seeks to build the community to solve them.

---

## Quick Start

**For Researchers** - Have a recurring friction that wastes your time?
1. [Search existing issues](#open-issues-16) to see if it's already cataloged
2. If not, [propose a new friction](../../issues/new?template=propose-friction.yml)

**For Developers** - Want to make an impact on research?
1. Browse the [open issues](#open-issues-16) below
2. [Claim one](../../issues/new?template=start-building.yml) and start building
3. Get credited when your solution ships

---

## How It Works

```
  RESEARCHER                   RESOLVE                   BUILDER(S)
      |                           |                            |
      |  1. Propose               |                            |
      |-------------------------->|                            |
      |                           |                            |
      |<------------------------->|  2. Discuss                |
      |      (refine scope)       |<-------------------------->|
      |                           |                            |
      |                           |  3. Claim                  |
      |                           |<---------------------------|
      |                           |                            |
      |                           |         4. Build           |
      |                           |         (own repo)         |
      |                           |                            |
      |  5. Problem solved!       |  5. Link solution          |
      |<--------------------------|<---------------------------|
      |                           |                            |
      |                           |     Credit preserved       |
```

1. **Propose** — Researcher submits a friction point via issue template
2. **Discuss** — Community refines scope, confirms no existing solution, validates impact. Teams can form here or within existing projects.
3. **Claim** — An individual or team signals they're starting work
4. **Build** — Solution is created in its own repository
5. **Link** — Solution is linked back; researcher's problem is solved, builders get credit

---

## What Makes Resolve Different

- **Open, vendor-neutral registry** of problems that slow down research and development
- **Concrete, buildable scopes** -- each issue targets a small, impactful MVP
- **Credit preserved** -- solutions are linked and celebrated, not absorbed

---

## Resolution Criteria

A solution is considered "Resolved" when it meets the following:
- Public repo with a clear license
- Minimal setup (works in minutes)
- README with a quickstart and examples
- Demonstrated to solve the stated friction

---

## Open Issues (16)

<details>
<summary><strong>1. HPC continuous integration runners</strong> - Easy and standardized Github actions CI/CD templates for testing software that targets major research clusters</summary>

> **Impact:** Faster regression detection, less futzing with infra.

**The friction:** GitHub Actions is really nice for automated testing. If you are developing software for scientific researchers on large clusters, it is difficult to replicate the exact setup of a cluster you're targeting. It is hard to know if a given package really works on a given cluster.

**Why existing tools don't solve it:** I'm not sure how people solve (or don't solve) this problem now. I think "market research" is part of tackling this.

**The solution:** I'm also not sure. Is there a general solution across clusters? Is there a secure way to handle a long-lived auth token in a GH runner that HPC admins will approve of?

**Status:** 🔴 Open

</details>

<details>
<summary><strong>2. groundhog</strong> - Iterative HPC development with Globus Compute (~25 hrs/year)</summary>

> **Impact:** ~25 hours saved per HPC researcher per year

**The friction:** Iterative development on HPC clusters is slow and frustrating. Your local environment differs from remote, so you manually maintain multiple Python virtual environments. Queue times are long, so you delay testing—then fail immediately with `No module named 'numpy'` because environments drifted.

**Why existing tools don't solve it:** The code-iteration loop and environment-iteration loop are independent, but both must be perfect for successful submission. You're constantly context-switching between code vs environment and local vs remote state.

**The solution:** Simple decorators for running, tweaking, and re-running Python functions on HPC clusters via Globus Compute with automatic remote environment management (powered by uv). Update Python versions or dependencies in your script—no SSH needed.

```bash
groundhog run train.py::train_model --cluster delta
groundhog sync --env requirements.txt
groundhog status --job-id abc123
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>3. ink</strong> - Persistent AI context across sessions (~20 hrs/year)</summary>

> **Impact:** ~20 hours saved per AI-assisted developer per year

**The friction:** Every time you open Claude Code, Gemini CLI, or Codex, your AI wakes up with amnesia. You re-explain your project structure, preferences, and recent work. Context is lost between sessions, and you waste time re-establishing shared understanding.

**Why existing tools don't solve it:** AI tools read files on demand but don't persist learned context, preferences, or project-specific patterns across sessions. There's no standard way to give your AI "memory."

**The solution:** Persistent AI context that learns your project, preferences, and recent work. Your AI remembers—always.

```bash
ink                     # Generate project context
ink steve-jobs          # Use a persona
ink me + steve-jobs     # Compose contexts
ink learn               # Reflect and grow from session
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>4. paperpack</strong> - Modern academic reference management (~15 hrs/year)</summary>

> **Impact:** ~15 hours saved per academic researcher per year

**The friction:** Managing academic references is painful. You copy BibTeX from Google Scholar, manually fix formatting inconsistencies, deal with merge conflicts in version control, and spend hours ensuring collaborators have the same references. When you move to a new project, you start from scratch.

**Why existing tools don't solve it:** Google Scholar, ArXiv, and publisher sites all format the same paper differently. BibTeX files are hard to diff and merge. Traditional `.bib` files are locked to one project with no portability.

**The solution:** A modern reference manager with universal identifiers (DOI/ArXiv ID), automatic BibTeX generation, shareable "packs" of curated references, and lock files for reproducible builds across machines.

```bash
paperpack add arxiv:1706.03762
paperpack add doi:10.1038/nature14539
paperpack add "attention is all you need"
paperpack build
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>5. doi-fetch</strong> - CLI for paper metadata lookup (~8 hrs/year)</summary>

> **Impact:** ~8 hours saved per researcher per year

**The friction:** Looking up paper metadata requires navigating multiple websites, copy-pasting from different sources, and manually formatting citations. When writing scripts or AI prompts, there's no simple way to fetch structured metadata from a DOI.

**Why existing tools don't solve it:** APIs exist but require authentication, parsing JSON responses, and handling rate limits. There's no simple CLI that just gives you what you need.

**The solution:** A Python CLI and Claude Skill that fetches clean metadata from any DOI—title, authors, abstract, BibTeX, and more—in one command.

```bash
doi-fetch 10.1038/nature14539
doi-fetch 10.1038/nature14539 --format bibtex
doi-fetch 10.1038/nature14539 --format json
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>6. cif-validate</strong> - CIF file validator for materials science (~12 hrs/year)</summary>

> **Impact:** ~12 hours saved per materials scientist per year

**The friction:** CIF files from databases or collaborators often contain partial occupancies, inconsistent symmetry, or missing metadata. DFT or structure analysis tools fail late, or worse, run with a subtly wrong structure.

**Why existing tools don't solve it:** Parsers usually coerce or drop invalid fields without a clear report. Validation rules are scattered across libraries and are not enforced consistently.

**The solution:** A strict validator and explainer for CIFs that reports structural and metadata issues before conversion or simulation.

```bash
cif-validate input.cif
cif-validate input.cif --report json
cif-validate input.cif --fix-suggestions
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>7. potential-lock</strong> - Interatomic potential versioning (~10 hrs/year)</summary>

> **Impact:** ~10 hours saved per computational materials researcher per year

**The friction:** Results change because an interatomic potential or force field file was updated, renamed, or swapped without anyone noticing. Reproducing a run becomes guesswork.

**Why existing tools don't solve it:** There is no standard way to lock and verify potential files, citations, and parameter sets across codes.

**The solution:** A manifest tool that fingerprints force field files, pins citations, and verifies a run uses the expected parameters.

```bash
potential-lock capture ./potentials --out potentials.lock
potential-lock verify potentials.lock --paths ./potentials
potential-lock diff old.lock new.lock
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>8. sdf-audit</strong> - Molecular file validation (~9 hrs/year)</summary>

> **Impact:** ~9 hours saved per chemoinformatics researcher per year

**The friction:** Molecular files contain invalid valence, missing stereochemistry, salts, or duplicate structures. These errors contaminate datasets and cause downstream failures.

**Why existing tools don't solve it:** Most toolkits can sanitize molecules, but they do not produce a clear, actionable audit trail or batch-level summaries.

**The solution:** A structure audit CLI that validates SDF/SMILES, flags problems, and outputs a clean report and filtered dataset.

```bash
sdf-audit molecules.sdf
sdf-audit molecules.sdf --out clean.sdf --report audit.json
sdf-audit molecules.smi --reject invalid.smi
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>9. env-replay</strong> - Full environment snapshot and rebuild (~12 hrs/year)</summary>

> **Impact:** ~12 hours saved per researcher per year

**The friction:** Reproducing a result from six months ago fails because `pip freeze` wasn't enough. OS packages, CUDA drivers, system libs, and hidden env vars are missing. You spend days doing version archaeology.

**Why existing tools don't solve it:** Lockfiles capture Python, not GPU drivers, OS packages, or runtime flags. Containers help, but most projects don't have one and researchers still need a fast local recreate.

**The solution:** A one-command environment snapshot + rebuild tool that captures the *full* run context and recreates it later.

```bash
env-replay capture --cmd "python train.py" --out run.manifest.json
env-replay build run.manifest.json            # reconstruct locally
env-replay verify run.manifest.json           # sanity checks + diff
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>10. data-manifest</strong> - Dataset versioning and drift detection (~15 hrs/year)</summary>

> **Impact:** ~15 hours saved per data scientist per year

**The friction:** Teams keep re-downloading datasets or silently using different versions. A single schema change can invalidate months of experiments, but no one notices until results diverge.

**Why existing tools don't solve it:** Checksums exist but aren't standardized, and schema drift alerts are rare for research datasets.

**The solution:** A dataset manifest generator that records hashes, schema, licenses, and provenance and can verify future pulls.

```bash
data-manifest scan data/ --out manifest.json
data-manifest verify data/ --manifest manifest.json
data-manifest diff old.manifest.json new.manifest.json
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>11. run-watchdog</strong> - Training run monitor and auto-termination (~20 hrs/year)</summary>

> **Impact:** ~20 hours saved per ML engineer per year

**The friction:** Long training runs stall, diverge, or hang, but keep burning GPU hours. You often discover the failure the next morning.

**Why existing tools don't solve it:** Monitoring stacks are heavy, and most ML projects don't set up custom alerts for every experiment.

**The solution:** A lightweight watchdog that detects stalls, NaNs, runaway loss, or missing log output and can terminate jobs safely.

```bash
run-watchdog --cmd "python train.py" --kill-on-nan --notify slack
run-watchdog --log train.log --alert-no-output 10m
run-watchdog --slurm <job_id> --auto-cancel
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>12. eval-lock</strong> - Versioned evaluation packs (~18 hrs/year)</summary>

> **Impact:** ~18 hours saved per researcher per year

**The friction:** Evaluation numbers drift because datasets, metrics, or preprocessing change quietly. Teams argue about which score is "real."

**Why existing tools don't solve it:** Evaluation scripts are custom and rarely versioned with datasets and metrics in a single reproducible bundle.

**The solution:** A versioned evaluation pack that pins datasets, metrics, and preprocessing to produce stable, comparable scores.

```bash
eval-lock init --task classification
eval-lock run --model ./checkpoints --pack imagenet_v3
eval-lock verify --pack imagenet_v3 --checksum
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>13. prompt-budget</strong> - LLM cost profiling and budgeting (~25 hrs/year)</summary>

> **Impact:** ~25 hours saved per LLM app developer per year

**The friction:** LLM costs spiral because prompts grow, chains expand, and caching is inconsistent. You only notice after the bill arrives.

**Why existing tools don't solve it:** Token counters are one-off; they don't profile *live* apps or attribute cost to call sites.

**The solution:** An app-level cost profiler that tracks token usage, caching, and per-endpoint budgets across providers.

```bash
prompt-budget wrap -- python app.py
prompt-budget report --top 20 --by endpoint
prompt-budget guard --max-usd 50/day
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>14. figlint</strong> - Journal figure compliance checker (~8 hrs/paper)</summary>

> **Impact:** ~8 hours saved per researcher per paper

**The friction:** Figures are rejected for wrong size, font, DPI, or accessibility issues right before submission deadlines.

**Why existing tools don't solve it:** Plotting libraries create figures but don't enforce journal-specific requirements or accessibility checks.

**The solution:** A figure linter that checks size, DPI, font, color contrast, and journal-specific constraints.

```bash
figlint --journal nature figures/*.png
figlint --check figure.svg --report json
figlint --fix --journal pnas figure.py
```

**Status:** 🔴 Open

</details>

<details>
<summary><strong>15. gardener</strong> - Publish ML models on Garden with AI assistance (~12-48 hrs/year)</summary>

> **Impact:** ~12-48 hours saved per researcher per year

**The friction:** Publishing machine learning models on Garden requires constructing functions through Modal or Groundhog. Even experienced developers need 1-2 days for implementation, while learning curves extend this substantially for newcomers.

**Why existing tools don't solve it:** LLMs are okay at writing Modal apps, but without extra context are not current on the latest APIs or best practices. Groundhog's novelty means large language models lack sufficient training data about it, making manual documentation injection tedious and inefficient.

**The solution:** A Claude Code plugin with contextual knowledge of Garden, Modal, and Groundhog ecosystems. Researchers share research papers and code repositories, and the agent automatically scaffolds and tests deployment functions.

```
/gardener Here is my paper https://arxiv.org/abs/2401.12345 and my code
https://github.com/user/model - help me publish these models on Garden
```

**Status:** 🟡 In Progress · [Related work](https://github.com/Garden-AI/gardener)

</details>

---

## Resolved

*Issues that have working solutions.*


---

## Contribute

### Build a Tool
1. Pick an open issue
2. [Claim it](../../issues/new?template=start-building.yml) so others know you're working on it
3. Build a working solution (in your own repo)
4. Open a PR linking to your repo
5. You and your team get featured here with full credit

Teams can form directly on Resolve (comment on an issue to find collaborators) or within existing projects that want to tackle a friction point.

### Propose a New Issue
Use the **["Propose a friction"](../../issues/new?template=propose-friction.yml)** issue template. We'll ask for:
- The specific recurring friction
- Who it affects
- Evidence that no good solution exists
- A 1-5 day MVP scope
- Estimated time saved per year

### Link a Solution
Use the **["Link a solution"](../../issues/new?template=link-solution.yml)** template to associate a tool or repo with an existing issue.

### Spread the Word
Star this repo. Share it with your lab or team. The more eyes, the faster problems get solved.

---

## Why This Matters

Every hour lost to avoidable friction is an hour not spent on discovery. Resolve turns those hours into a public roadmap and makes it easier for builders to focus on the most valuable fixes.

**Let's resolve the papercuts.**

---

<p align="center">
  <a href="LICENSE">MIT License</a> · <a href="CONTRIBUTING.md">Contributing</a> · <a href="SECURITY.md">Security</a>
</p>
