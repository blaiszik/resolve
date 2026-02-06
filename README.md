<p align="center">
  <img src="resolve.png" alt="Resolve Logo" width="360" height="360">
</p>

<h1 align="center">Resolve</h1>

<p align="center">
  <strong>The open registry for fixing the repeatable failures in AI, science, engineering, and other research workflows.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
  <img src="https://img.shields.io/badge/issues-14_open-red.svg" alt="14 Open Issues">
  <img src="https://img.shields.io/badge/resolved-1-green.svg" alt="1 Resolved">
</p>

---

Researchers and engineers lose weeks each year to problems that should take minutes: broken environments, invisible dataset drift, stalled training runs, and last-minute figure rejections. These are often not hard problems. They are *unowned* problems, and we aim to resolve them.

**Resolve catalogs high-leverage, well-scoped issues across AI and scientific research workflows, estimates the time they waste, and tracks solutions when they appear.** Each issue is proposed in a way to be 1) actionable to the open source community, 2) buildable in a few days, and 3) remove a recurring source of friction.

This repo does **not** host the tools themselves. It provides the shared backlog the ecosystem needs, and seeks to build the community to solve them.

---

## Quick Start

**For Researchers** - Have a recurring friction that wastes your time?
1. [Search existing issues](#open-issues-14) to see if it's already cataloged
2. If not, [propose a new friction](../../issues/new?template=propose-friction.yml)

**For Developers** - Want to make an impact on research?
1. Browse the [open issues](#open-issues-14) below
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

## Open Issues (14)

<details>
<summary><strong>1. hpc-ci</strong> - Reusable GitHub Actions for testing on research clusters (~15 hrs/year)</summary>

> **Impact:** ~15 hours saved per HPC software developer per year

**The friction:** You develop software that targets large research clusters (Delta, Polaris, Frontier, Perlmutter). Your CI runs on GitHub Actions, but your tests only pass on the actual cluster -- specific MPI versions, GPU architectures, module systems, and filesystems that can't be replicated in a container. You merge to main, push a release, and discover it's broken on the cluster your users actually run. Debugging takes hours because you can't reproduce the failure locally.

**Why existing tools don't solve it:** Self-hosted GitHub runners exist but require cluster admin approval and a persistent auth token -- most HPC centers won't allow it. Container-based CI (Docker) can't replicate cluster-specific module systems, interconnects, or GPU configurations. There's no standardized way to run a test suite on a real HPC cluster as part of a GitHub Actions workflow.

**The solution:** A set of reusable GitHub Actions that SSH into target clusters, submit test jobs via the scheduler, and report results back to the PR. Supports SLURM and PBS, with configurable modules and allocations.

```yaml
# .github/workflows/hpc-test.yml
- uses: hpc-ci/slurm-test@v1
  with:
    cluster: delta
    modules: "gcc/11.3.0 cuda/11.8"
    test-cmd: "pytest tests/ -x"
    allocation: my-project
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>2. ink</strong> - Persistent AI context across sessions (~20 hrs/year)</summary>

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
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>3. paperpack</strong> - Modern academic reference management and metadata lookup (~20 hrs/year)</summary>

> **Impact:** ~20 hours saved per academic researcher per year

**The friction:** Managing academic references is painful at every stage. Looking up paper metadata means navigating multiple websites and manually formatting citations. Building a `.bib` file means copy-pasting from Google Scholar, fixing inconsistencies, and dealing with merge conflicts in version control. When you move to a new project, you start from scratch.

**Why existing tools don't solve it:** Google Scholar, ArXiv, and publisher sites all format the same paper differently. APIs exist but require authentication and JSON parsing. BibTeX files are hard to diff and merge. Traditional `.bib` files are locked to one project with no portability. Zotero and Mendeley are GUI-heavy and not git-native.

**The solution:** A CLI-first reference manager with universal identifiers (DOI/ArXiv ID), instant metadata and BibTeX lookup, shareable "packs" of curated references, and lock files for reproducible builds across machines.

```bash
paperpack add arxiv:1706.03762              # add by ArXiv ID
paperpack add doi:10.1038/nature14539       # add by DOI
paperpack add "attention is all you need"   # fuzzy search
paperpack fetch 10.1038/nature14539         # quick metadata lookup without adding
paperpack fetch 10.1038/nature14539 --format bibtex
paperpack build                             # generate .bib from lock file
paperpack diff main..feature-branch         # diff references across branches
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>4. struct-lint</strong> - Scientific structure file validator (~15 hrs/year)</summary>

> **Impact:** ~15 hours saved per computational scientist per year

**The friction:** Computational scientists work with structure files (CIF for crystallography, SDF/SMILES for chemistry, PDB for biology) that are rife with silent errors. CIF files contain partial occupancies, inconsistent symmetry, or missing metadata -- DFT tools fail late or run with a subtly wrong structure. Molecular files contain invalid valence, missing stereochemistry, salts, or duplicates that contaminate entire datasets. In both cases, the errors are discovered far downstream, after wasting compute time or corrupting results.

**Why existing tools don't solve it:** Parsers coerce or drop invalid fields without clear reporting. Toolkits like RDKit can sanitize molecules but don't produce actionable audit trails or batch-level summaries. Validation rules are scattered across domain-specific libraries and enforced inconsistently. There's no unified tool that validates structure files across formats.

**The solution:** A pluggable structure validator that supports multiple scientific file formats (CIF, SDF, SMILES, PDB), reports issues with clear explanations, and outputs clean audit reports and filtered datasets.

```bash
struct-lint input.cif                                  # validate a CIF file
struct-lint molecules.sdf --out clean.sdf --report audit.json
struct-lint input.cif --fix-suggestions
struct-lint molecules.smi --reject invalid.smi
struct-lint protein.pdb --report json
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>5. env-rescue</strong> - Environment snapshot, diagnosis, and rebuild (~20 hrs/year)</summary>

> **Impact:** ~20 hours saved per researcher per year

**The friction:** Research environments break in two directions. *Forward:* you clone a repo and `pip install -r requirements.txt` fails -- CUDA version mismatch, yanked packages, missing system libraries (`libGL.so`), Python version conflicts. You spend hours reading cryptic errors and trying random version combinations. *Backward:* reproducing a result from six months ago fails because `pip freeze` wasn't enough -- OS packages, CUDA drivers, system libs, and env vars are missing. You spend days on version archaeology.

**Why existing tools don't solve it:** Lockfiles capture Python, not GPU drivers, OS packages, or runtime flags. `pip` gives errors but not fixes. Dependency resolvers flag conflicts but not missing system libraries or CUDA mismatches. Containers help, but most research projects don't have one.

**The solution:** A single tool that snapshots working environments, diagnoses broken ones, and rebuilds from manifests. Capture when things work, diagnose when they break.

```bash
# Snapshot a working environment
env-rescue capture --cmd "python train.py" --out run.manifest.json

# Diagnose a broken one
env-rescue diagnose requirements.txt
env-rescue explain "ERROR: No matching distribution found for torch==1.9.0"
env-rescue check --cuda --python 3.11

# Rebuild from a snapshot
env-rescue build run.manifest.json
env-rescue verify run.manifest.json
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>6. data-manifest</strong> - Research artifact versioning and drift detection (~20 hrs/year)</summary>

> **Impact:** ~20 hours saved per researcher per year

**The friction:** Research depends on versioned artifacts -- datasets, interatomic potentials, force fields, pretrained weights -- but none of them have reliable version control. Teams silently use different dataset versions and don't notice until results diverge. A force field file gets updated or renamed without anyone noticing, making old results irreproducible. A schema change invalidates months of experiments. These are all the same underlying problem: critical research inputs changing without detection.

**Why existing tools don't solve it:** DVC and LakeFS exist but require remote storage backends and Git integration overhead -- too heavy for most research projects. Checksums exist but aren't standardized. There's no zero-infrastructure way to fingerprint, lock, and verify research artifacts with their provenance, citations, and parameter metadata.

**The solution:** A zero-infrastructure manifest tool that fingerprints any research artifact (datasets, potentials, weights, configs), records hashes, schema, provenance, and citations, and verifies future state against the locked manifest. Just a JSON sidecar file -- no server, no special Git hooks.

```bash
# Datasets
data-manifest scan data/ --out manifest.json
data-manifest verify data/ --manifest manifest.json
data-manifest diff old.manifest.json new.manifest.json

# Force fields and potentials
data-manifest scan ./potentials --type potentials --out potentials.lock
data-manifest verify potentials.lock --paths ./potentials

# Any research artifact
data-manifest scan ./checkpoints --out weights.manifest.json
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>7. run-watchdog</strong> - Training run monitor and auto-termination (~20 hrs/year)</summary>

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
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>8. eval-lock</strong> - Versioned evaluation packs (~18 hrs/year)</summary>

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
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>9. prompt-budget</strong> - LLM cost profiling and budgeting (~25 hrs/year)</summary>

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
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>10. figlint</strong> - Journal figure compliance checker (~8 hrs/paper)</summary>

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
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>11. notebook-clean</strong> - Jupyter notebook hygiene for version control (~10 hrs/year)</summary>

> **Impact:** ~10 hours saved per researcher per year

**The friction:** Jupyter notebooks are the lingua franca of research, but they're hostile to version control. Outputs, execution counts, and cell metadata pollute diffs so badly that code review is effectively impossible. Researchers accidentally commit 50 MB inline images, API keys in output cells, or entire dataframes. Pull requests touching notebooks are either rubber-stamped or ignored.

**Why existing tools don't solve it:** nbstripout exists but only strips outputs -- it doesn't catch embedded secrets, oversized outputs, or inconsistent cell ordering. nbdime helps with diffs but requires manual setup per repo and doesn't integrate cleanly with GitHub's review UI. There's no single tool that acts as a pre-commit linter combining output stripping, secret detection, and size guards for notebooks.

**The solution:** A pre-commit hook and CLI that strips outputs, detects embedded secrets and large blobs, normalizes metadata, and optionally produces a clean `.py` mirror for review.

```bash
notebook-clean check *.ipynb                    # lint without modifying
notebook-clean strip notebook.ipynb             # strip outputs + metadata
notebook-clean install                          # set up pre-commit hook
notebook-clean diff HEAD~1 analysis.ipynb       # human-readable diff
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>12. slurm-translate</strong> - Cross-scheduler job script translator (~8 hrs/year)</summary>

> **Impact:** ~8 hours saved per researcher per year

**The friction:** Researchers regularly move between HPC clusters that run different schedulers -- SLURM at one university, PBS/Torque at a national lab, LSF at an industry partner. Every move means rewriting batch scripts: `#SBATCH` becomes `#PBS`, `srun` becomes `mpirun`, partition names change, environment module syntax differs. You spend an afternoon hunting through docs for the equivalent flags, then debug silent failures because one scheduler interprets walltime format differently.

**Why existing tools don't solve it:** Each scheduler has its own documentation and its own quirks. There are scattered wiki pages with partial mapping tables, but no executable tool that does the translation. Researchers end up maintaining parallel copies of the same job script.

**The solution:** A CLI that translates job scripts between SLURM, PBS/Torque, and LSF, mapping directives, resource requests, and common environment patterns.

```bash
slurm-translate job.slurm --to pbs --out job.pbs
slurm-translate job.pbs --to slurm --out job.slurm
slurm-translate job.slurm --to lsf --cluster bridges2
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>13. table-extract</strong> - Extract data tables from research paper PDFs (~12 hrs/year)</summary>

> **Impact:** ~12 hours saved per researcher per year

**The friction:** Research papers report key results in tables buried inside PDFs. To compare methods, build meta-analyses, or reuse published data, you manually retype numbers from PDF tables into spreadsheets -- introducing transcription errors and wasting hours. Multi-page tables, merged cells, and footnote markers make it worse.

**Why existing tools don't solve it:** Tabula and Camelot handle simple tables but choke on complex layouts (multi-row headers, spanning cells, mixed units). Copy-paste from PDFs garbles column alignment. There's no tool that handles the common research table formats and outputs clean, labeled data with source provenance.

**The solution:** A CLI that extracts tables from research paper PDFs into CSV/JSON, handling common academic table layouts and preserving column headers and units.

```bash
table-extract paper.pdf                          # list all detected tables
table-extract paper.pdf --table 3 --out results.csv
table-extract paper.pdf --all --format json --out tables/
table-extract paper.pdf --table 2 --verify       # show extracted vs original side-by-side
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

<details>
<summary><strong>14. gpu-claim</strong> - Lightweight shared GPU allocation for research labs (~15 hrs/year)</summary>

> **Impact:** ~15 hours saved per researcher per year

**The friction:** In shared lab workstations without a formal scheduler, GPU allocation is managed via Slack messages: "anyone using GPU 2?" You launch a job, discover someone else is already using that GPU (OOM crash), or you wait hours out of politeness for a GPU that's actually free. `nvidia-smi` shows processes but not who owns them or how long they'll run.

**Why existing tools don't solve it:** Full schedulers (SLURM, Kubernetes) are overkill for a 4-8 GPU lab machine and require admin setup. `nvidia-smi` shows PIDs but not intent -- you can't tell if a GPU is "in use for 5 more minutes" or "running a 3-day training job."

**The solution:** A zero-infrastructure CLI for claiming and releasing GPUs on shared machines, with visibility into who's using what and for how long. No daemon, no admin -- just a shared state file.

```bash
gpu-claim take 0,1 --user alice --hours 4        # claim GPUs 0 and 1
gpu-claim status                                  # show all GPUs, owners, time remaining
gpu-claim release 0,1                             # free GPUs
gpu-claim wait --gpus 2 --notify                  # block until 2 GPUs are free
```

**Status:** 🔴 Open
[💬 Discuss](https://github.com/blaiszik/resolve/discussions) · [🛠️ Start building](../../issues/new?template=start-building.yml)

</details>

---

## Resolved (1)

Solutions that shipped. The builders, the tools, the problems they fixed.

| Issue | Solution | Built by | Links |
|-------|----------|----------|-------|
| **groundhog** - Iterative HPC development with Globus Compute | Simple decorators for running and re-running Python functions on HPC clusters via Globus Compute with automatic remote environment management (uv). No SSH needed. | [Garden-AI](https://github.com/Garden-AI) | [Repo](https://github.com/Garden-AI/groundhog) · [MIT](https://github.com/Garden-AI/groundhog/blob/main/LICENSE) |

<details>
<summary>See the original friction</summary>

> **Impact:** ~25 hours saved per HPC researcher per year

**The friction:** Iterative development on HPC clusters is slow and frustrating. Your local environment differs from remote, so you manually maintain multiple Python virtual environments. Queue times are long, so you delay testing—then fail immediately with `No module named 'numpy'` because environments drifted.

**Why existing tools don't solve it:** The code-iteration loop and environment-iteration loop are independent, but both must be perfect for successful submission. You're constantly context-switching between code vs environment and local vs remote state.

```bash
groundhog run train.py::train_model --cluster delta
groundhog sync --env requirements.txt
groundhog status --job-id abc123
```

**Status:** 🟢 Solved

</details>


---

## Contribute

There are three ways to get involved. Pick the one that fits.

### Report a Papercut
Researchers: that thing that wastes your time every week? Write it up. Use the **["Propose a friction"](../../issues/new?template=propose-friction.yml)** template -- it'll walk you through scoping the problem so a builder can actually fix it.

### Build a Tool
Developers (and developer-researchers): this is where the fun is.

1. **Browse** the open issues above and find one that speaks to you
2. **[Claim it](../../issues/new?template=start-building.yml)** so others know you're on it (and so potential teammates can find you)
3. **Build** a working solution in your own repo -- you own the code, the license, and the credit
4. **[Link it back](../../issues/new?template=link-solution.yml)** when it ships
5. **Get featured** in the Resolved section with your full team credited

**Why this is worth your time:** Every Resolve issue has a defined scope, a clear user, and a measurable impact. You're not building into the void -- researchers already told you they need this. It's a portfolio piece, a collaboration opportunity, and a chance to make someone's workflow meaningfully better, all in one.

### Find Teammates
Some of the best tools come from small teams. Here's how to find yours:

- **Comment on any open issue** -- say what skills you bring and what you're looking for
- **Share an issue with your lab or community** -- the descriptions are self-contained, anyone can read them and jump in
- **Look for complementary skills** -- the magic combo is someone who *has the problem* paired with someone who *loves building CLIs*

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed team-building best practices, including how to divide work, what team sizes work best, and how to ship fast.

### Spread the Word
Star this repo. Share it with your lab, your Slack, your conference hallway track. The more eyes, the faster problems get solved.

---

## Why This Matters

Every hour lost to avoidable friction is an hour not spent on discovery. Resolve turns those hours into a public roadmap and makes it easy for builders to find high-impact problems worth solving.

The tools get built. The builders get credit. The researchers get their time back.

**Let's resolve the papercuts.**

---

<p align="center">
  <a href="LICENSE">MIT License</a> · <a href="CONTRIBUTING.md">Contributing</a> · <a href="SECURITY.md">Security</a>
</p>
