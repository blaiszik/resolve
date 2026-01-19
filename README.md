# Resolve

> **The open registry for fixing the repeatable failures in AI and science workflows.**

Researchers and engineers lose weeks each year to problems that should take minutes: broken environments, invisible dataset drift, stalled training runs, and last-minute figure rejections. These are not hard problems. They are *unowned* problems.

**Resolve catalogs high-leverage, well-scoped issues across AI and scientific research flows, estimates the time they waste, and tracks solutions when they appear.** Each issue is designed to be buildable in a few days and to remove a recurring, global source of friction.

This repo does **not** host the tools themselves. It provides the shared backlog the ecosystem needs.

---

## What Makes Resolve Different

- **Open, vendor-neutral registry** of problems that slow down research and development
- **Concrete, buildable scopes** -- each issue targets a small, sharp MVP
- **Measured impact** -- time saved per year is estimated for every issue
- **Credit preserved** -- solutions are linked and celebrated, not absorbed

---

## How It Works

1. **Propose a friction point** using the issue template
2. **Validate** the pain and scope with maintainers and the community
3. **Build a tool** (in your own repo) or point to an existing one
4. **Link the solution** here and move the issue to **Resolved**

## Resolution Criteria

A solution is considered "Resolved" when it meets the following:
- Public repo with a clear license
- Minimal setup (works in minutes)
- README with a quickstart and examples
- Demonstrated to solve the stated friction

---

## Open Issues (15)

### 1. cif-validate
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

### 2. potential-lock
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

### 3. chem-split
> **Impact:** ~14 hours saved per drug discovery ML researcher per year

**The friction:** Model performance looks great until you discover train/test leakage from random splits. Proper scaffold or time-based splits take hours to implement and are easy to get wrong.

**Why existing tools don't solve it:** Libraries provide splitting utilities, but they rarely report leakage risk or provide audit reports for reviewers.

**The solution:** A dataset splitter that enforces scaffold, temporal, or cluster splits and produces a leakage audit report.

```bash
chem-split data.csv --smiles SMILES --split scaffold
chem-split data.csv --split time --date-col assay_date
chem-split report data.csv --split-file split.json
```

**Status:** 🔴 Open

---

### 4. assay-drift
> **Impact:** ~16 hours saved per drug discovery researcher per year

**The friction:** Assay datasets quietly change units, thresholds, or label definitions between releases. Model results drift and no one knows why.

**Why existing tools don't solve it:** Dataset versioning exists, but there is no focused diff for assay metadata, units, and label semantics.

**The solution:** A semantic diff tool for assay datasets that detects unit changes, threshold shifts, and label redefinitions.

```bash
assay-drift old.csv new.csv --id-col assay_id
assay-drift old.csv new.csv --report json
assay-drift --schema old.yaml new.csv
```

**Status:** 🔴 Open

---

### 5. sdf-audit
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

### 6. env-replay
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

### 7. data-manifest
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

### 8. parquet-diff
> **Impact:** ~10 hours saved per data engineer per year

**The friction:** You change a preprocessing step and now your Parquet output is different. `git diff` is useless, and existing tools flood you with raw values instead of meaningful summaries.

**Why existing tools don't solve it:** Byte-level diffs miss semantic change; statistical diffs are rarely packaged for Parquet/Arrow workflows.

**The solution:** A semantic diff tool for Parquet/Arrow that summarizes schema and statistical drift.

```bash
parquet-diff old.parquet new.parquet
parquet-diff old.parquet new.parquet --stats mean,std,quantiles
parquet-diff --format json old.parquet new.parquet
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

### 10. ckpt-lint
> **Impact:** ~8 hours saved per ML researcher per year

**The friction:** You load a checkpoint and get a cryptic mismatch error or silent missing keys. Debugging which layer or version caused it is a time sink.

**Why existing tools don't solve it:** Framework errors are low-level and don't explain compatibility, architecture diffs, or corruption clearly.

**The solution:** A checkpoint validator that detects shape mismatches, missing keys, corrupted weights, and framework compatibility issues *before* you run.

```bash
ckpt-lint model.pt --model-config config.yaml
ckpt-lint model.pt --diff model_prev.pt
ckpt-lint model.pt --report json
```

**Status:** 🔴 Open

---

### 11. eval-lock
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

### 12. prompt-budget
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

### 13. citation-watch
> **Impact:** ~6 hours saved per researcher per year

**The friction:** Papers get desk-rejected because of missing DOIs, outdated citations, or references to retracted work.

**Why existing tools don't solve it:** Reference managers format citations but don't validate them against current metadata or retraction status.

**The solution:** A citation validator that flags missing metadata, retracted papers, and newer versions.

```bash
citation-watch paper.bib
citation-watch paper.md --format apa
citation-watch paper.bib --report json
```

**Status:** 🔴 Open

---

### 14. figlint
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

### 15. pii-scrub
> **Impact:** ~12 hours saved per research engineer per year

**The friction:** Logs and datasets accidentally contain PII or sensitive data. Scrubbing manually is slow and error-prone, delaying releases and collaborations.

**Why existing tools don't solve it:** General PII scanners are enterprise-grade or too heavy for research workflows and lack dataset-aware redaction.

**The solution:** A lightweight PII detector and redactor that works on common research formats (CSV, JSONL, logs).

```bash
pii-scrub scan data.jsonl
pii-scrub redact data.jsonl --out data.redacted.jsonl
pii-scrub report data.jsonl --format json
```

**Status:** 🔴 Open

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
