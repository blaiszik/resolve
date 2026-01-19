# Resolve

> **The open registry for fixing the repeatable failures in AI, science, engineering, and other research workflows.**

Researchers and engineers lose weeks each year to problems that should take minutes: broken environments, invisible dataset drift, stalled training runs, and last-minute figure rejections. These are often not hard problems. They are *unowned* problems, and we aim to resolve them.

**Resolve catalogs high-leverage, well-scoped issues across AI and scientific research workflows, estimates the time they waste, and tracks solutions when they appear.** Each issue is proposed in a way to be 1) actionable to the open source community, 2) buildable in a few days, and 3) remove a recurring source of friction.

This repo does **not** host the tools themselves. It provides the shared backlog the ecosystem needs, and seeks to build the community to solve them.

---

## What Makes Resolve Different

- **Open, vendor-neutral registry** of problems that slow down research and development
- **Concrete, buildable scopes** -- each issue targets a small, impactful MVP
- **Credit preserved** -- solutions are linked and celebrated, not absorbed

---

## How It Works

1. **Propose a friction point** using the issue template
2. **Validate** the pain and scope with maintainers and the community
3. **Build a tool** (in your own repo) or point to an existing one if you know of a solution
4. **Link the solution** here and move the issue to **Resolved**

## Resolution Criteria

A solution is considered "Resolved" when it meets the following:
- Public repo with a clear license
- Minimal setup (works in minutes)
- README with a quickstart and examples
- Demonstrated to solve the stated friction

---

## Open Issues (12)

### 1. rootstock
> **Impact:** ~20 hours saved per HPC researcher per year

**The friction:** Using Machine Learning Interatomic Potentials (MLIPs) on HPC clusters requires managing complex, conflicting Python environments with CUDA dependencies. Researchers waste hours wrestling with installations, and switching between MACE, CHGNet, or other calculators means rebuilding environments from scratch.

**Why existing tools don't solve it:** Conda/pip environments don't cache well across users or handle GPU library conflicts. Containers add friction for interactive ASE workflows. There's no standard way to share pre-built MLIP environments across a cluster.

**The solution:** Cached, isolated Python environments for MLIPs that communicate via the i-PI protocol over Unix sockets. Researchers use MLIP calculators through the ASE interface without managing dependencies.

```bash
rootstock run mace-mp-0 -- python relax.py
rootstock test chgnet --verify-gpu
rootstock register ./my-env.yaml --root /shared/envs
```

**Status:** 🔴 Open

---

### 2. groundhog
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

---

### 3. grove
> **Impact:** ~15 hours saved per JS/TS developer per year

**The friction:** Modern development often requires working on multiple branches simultaneously—reviewing a PR while fixing a bug, comparing implementations side-by-side, or running parallel AI coding sessions. Git worktrees solve this, but for JavaScript/TypeScript projects they're painful. Each worktree needs its own `node_modules`, meaning slow `npm install` in every worktree, gigabytes of duplicated packages, and manual port management for preview servers.

**Why existing tools don't solve it:** Git worktrees work great for compiled languages, but the JS ecosystem assumes one `node_modules` per repo. No tool handles dependency deduplication, port allocation, and editor/AI tool configuration across worktrees.

**The solution:** A git worktree manager with smart dependency handling—shared caches, automatic port allocation, and preserved tool configurations across worktrees.

```bash
grove create feature-branch
grove list
grove switch feature-branch
grove clean --older-than 7d
```

**Status:** 🔴 Open

---

### 4. ink
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

---

### 5. cif-validate
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

---

### 6. potential-lock
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

---

### 7. sdf-audit
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

---

### 8. env-replay
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

---

### 9. data-manifest
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

---

### 9. run-watchdog
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

---

### 10. eval-lock
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

---

### 11. prompt-budget
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

---

### 12. figlint
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

---

---
## Resolved

*Issues that have working solutions.*

### grove
**The problem:** Git worktrees for JS/TS projects are painful; each needs its own `node_modules`.

**Solved:** [grove](https://github.com/blaiszik/grove) by [@blaiszik](https://github.com/blaiszik)

---

## Contribute

### Build a Tool
1. Pick an open issue
2. Build a working solution (in your own repo)
3. Open a PR linking to your repo
4. Your tool gets featured here with full credit

### Propose a New Issue
Use the **"Propose a friction"** issue template. We'll ask for:
- The specific recurring friction
- Who it affects
- Evidence that no good solution exists
- A 1-5 day MVP scope
- Estimated time saved per year

### Link a Solution
Use the **"Link a solution"** template to associate a tool or repo with an existing issue.

### Spread the Word
Star this repo. Share it with your lab or team. The more eyes, the faster problems get solved.

---

## Why This Matters

Every hour lost to avoidable friction is an hour not spent on discovery. Resolve turns those hours into a public roadmap and makes it easier for builders to focus on the most valuable fixes.

**Let's resolve the papercuts.**
