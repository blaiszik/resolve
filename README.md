# Resolve

Standalone tools that solve real friction in AI development and scientific computing. Each issue is:
- A self-contained project, not a feature request to a large codebase
- Buildable in an afternoon by a skilled developer with AI assistance
- Focused on a specific, repeated frustration

**Want to propose an issue?** [Open an issue](../../issues/new) describing the friction and what a solution might look like.

**Want to solve one?** Pick an issue, build it, and submit a PR linking to your tool.

---

## Quick Reference

| # | Name |
|---|------|| 1 | ssh-jump |
| 2 | bib-clean |
| 3 | cuda-guard |
| 4 | docker-squash-ml |
| 5 | plot-theme |
| 6 | s3-ncdu |
| 7 | api-recorder |
| 8 | pypi-diff |
| 9 | readme-api |
| 10 | git-large-check |
| 11 | checkpoint-bundle |
| 12 | tokcount |
| 13 | nan-sentinel |
| 14 | papenv |
| 15 | crystconv |
| 16 | sqwatch |
| 17 | promptgit |
| 18 | embviz |
| 19 | constaudit |
| 20 | hdf-diff |
| 21 | lablog |
| 22 | molsim |
| 23 | xrdmatch |
| 24 | vasp2qe |
| 25 | jupyckpt |
| 26 | chemstock |
| 27 | ragchunk |
| 28 | run-compare |
| 29 | slurm-insights |
| 30 | structure-view |
| 31 | hpc-skeleton |
| 32 | queue-bot |
| 33 | mol-hover |
| 34 | spec-annotate |
| 35 | cite-fetch |
| 36 | env-diff |
| 37 | traceback-buddy |
| 38 | notebook-clean |
| 39 | param-sweep |
| 40 | dataset-contract |
| 41 | module-map |
| 42 | sim-cache |
| 43 | sample-link |
| 44 | eval-sandbox |
| 45 | gpu-budget |

---

## The Issues

### 1. ssh-jump

**The problem:** Accessing compute nodes often requires multi-hop SSH tunnels (`ssh -> login -> compute`). Setting up port forwarding for Jupyter or VS Code through multiple jumps is a manual `ssh -L` nightmare.

**The solution:** A CLI `ssh-jump node123 --port 8888` that reads your `.ssh/config`, automatically detects the jump host chain, and establishes the tunnel in one command.

---

### 2. bib-clean

**The problem:** `references.bib` files accumulate duplicates, inconsistent formatting (initials vs full names), and missing DOIs. Merging bib files from collaborators is messy.

**The solution:** A CLI `bib-clean refs.bib` that canonicalizes formatting, fetches missing metadata from DOIs, dedupes entries, and sorts the file.

---

### 3. cuda-guard

**The problem:** You `pip install torch` only to find it pulled a version incompatible with the host's NVIDIA driver. You typically find out only when the runtime crashes.

**The solution:** A pre-run check tool. `cuda-guard check` verifies installed Python packages against system `nvidia-smi` version and warns of incompatibilities before execution.

---

### 4. docker-squash-ml

**The problem:** Docker images for ML are massive (10GB+), making CI/CD slow. They often contain unused libraries or cached files.

**The solution:** A tool that profiles a container run, identifies unaccessed files and packages, and rebuilds a minimal image containing only what is needed.

---

### 5. plot-theme

**The problem:** Every Matplotlib/Seaborn plot looks different. Maintaining a consistent style across papers requires copy-pasting `plt.rcParams` blocks.

**The solution:** A CLI and library to store and sync plot themes. `plot-theme apply paper-v1` sets styles. `plot-theme sync` shares them across your projects.

---

### 6. s3-ncdu

**The problem:** S3 buckets fill up with junk, but `aws s3 ls` is hard to read for aggregate usage. It's difficult to find where the space is going.

**The solution:** A TUI (Text User Interface) for S3, similar to `ncdu`. Allows you to navigate buckets, view folder sizes, and delete paths recursively.

---

### 7. api-recorder

**The problem:** Testing code that relies on LLM APIs is expensive and slow. Writing manual mocks for API responses is tedious.

**The solution:** A proxy that records API responses to disk. `api-recorder start` captures traffic; `api-recorder replay` serves the cached responses for offline, free, deterministic testing.

---

### 8. pypi-diff

**The problem:** Upgrading a library breaks your code, but the changelog is vague or missing. It's hard to know what specifically changed in the API.

**The solution:** `pypi-diff libname 1.0.0 1.1.0` downloads both versions, performs AST analysis, and lists changed functions, classes, and signature modifications.

---

### 9. readme-api

**The problem:** `README.md` examples drift from the actual code. Setting up Sphinx/MkDocs is too heavy for small libraries.

**The solution:** A tool that embeds docstrings into the README. You place markers like `<!-- API: func_name -->`, and the tool injects the current signature and docstring automatically.

---

### 10. git-large-check

**The problem:** Accidentally committing a 200MB checkpoint bloats the git repo history forever.

**The solution:** A pre-commit hook that scans for large files *before* they are added or committed, warns the user, and offers to add them to `.gitignore` or `dvc`.

---

### 11. checkpoint-bundle

**The problem:** You save a PyTorch/JAX checkpoint, but six months later you can't reproduce the results. The checkpoint has weights but not the code version, config, random seeds, or environment that created it.

**The solution:** A CLI that wraps your training script and creates reproducibility bundles. `checkpoint-bundle run train.py` captures the git hash, full config, pip freeze, hardware info, and random seeds alongside every checkpoint. `checkpoint-bundle reproduce ./checkpoint-bundle-20240115/` recreates the environment and resumes exactly.

---

### 12. tokcount

**The problem:** Estimating LLM costs requires running each provider's tokenizer separately. "How many tokens is this document for Claude vs GPT-4 vs Gemini?" requires three different tools.

**The solution:** A universal token counter. `tokcount document.txt` shows token counts and estimated costs across all major providers in one table. Supports stdin, files, and directories. `tokcount --watch ./prompts/` monitors a directory and alerts when prompts exceed thresholds.

---

### 13. nan-sentinel

**The problem:** Training runs for 8 hours, then you wake up to find loss went NaN at hour 2 and wasted 6 hours of GPU time. There's no built-in tripwire.

**The solution:** A lightweight daemon that watches training logs. `nan-sentinel --log train.log --notify slack` monitors for NaN, loss explosions, or training stalls. Sends alerts via Slack, email, or SMS. Works with any framework—just watches stdout/files for patterns.

---

### 14. papenv

**The problem:** Paper code repos have broken dependencies. `pip install -r requirements.txt` fails because packages are unpinned or yanked. Getting a 2-year-old paper's code running takes hours.

**The solution:** A tool that creates working environments from paper repos. `papenv https://github.com/author/paper-code` analyzes the repo, resolves version conflicts using historical PyPI data, and generates a working Dockerfile or conda environment. Maintains a community database of known-working configurations.

---

### 15. crystconv

**The problem:** Converting between CIF, POSCAR, XYZ, and other crystal structure formats silently drops metadata. You lose symmetry information, occupancies, or bibliographic data without warning.

**The solution:** A structure converter that warns loudly. `crystconv input.cif -o output.poscar` converts but prints exactly what information will be lost. `--strict` mode refuses lossy conversions. `crystconv diff a.cif b.cif` compares structures accounting for equivalent representations.

---

### 16. sqwatch

**The problem:** You submit 500 Slurm jobs and have no good way to monitor them. Which failed? Why? `squeue` and `sacct` output is dense and hard to parse.

**The solution:** A TUI dashboard for Slurm. `sqwatch` shows your jobs in a navigable interface. Filter by status, drill into failed jobs to see error logs, resubmit failed jobs with one keypress. `sqwatch --notify discord` alerts you when jobs complete or fail.

---

### 17. promptgit

**The problem:** Prompt engineering involves lots of iteration, but prompts are scattered across codebases with no version history. "What prompt were we using last week?" requires git archaeology.

**The solution:** A prompt versioning system. `promptgit init` creates a prompt repository. `promptgit save summarizer "Summarize the following..."` versions your prompts. `promptgit diff summarizer` shows how a prompt evolved. `promptgit use summarizer@v3` in your code loads a specific version. Tracks which prompt versions were used in production.

---

### 18. embviz

**The problem:** You generated embeddings but have no quick way to sanity-check them. Are similar items actually close? Are there obvious clusters? Checking requires writing matplotlib code every time.

**The solution:** An instant embedding visualizer. `embviz embeddings.npy --labels labels.txt` opens an interactive UMAP/t-SNE visualization. Hover for labels, search for items, see nearest neighbors. `embviz compare old.npy new.npy` shows how your embedding space changed. Works in terminal (sixel) or browser.

---

### 19. constaudit

**The problem:** Scientific code has hardcoded physical constants with unknown provenance. Is that `6.626e-34` from CODATA 2018? A textbook? Who knows.

**The solution:** A linter for physical constants. `constaudit scan simulation.py` finds hardcoded constants, identifies what they probably are, and reports which standard values they match (or don't). `constaudit replace simulation.py` rewrites them to use `scipy.constants` with proper citations.

---

### 20. hdf-diff

**The problem:** Scientific data lives in HDF5/NetCDF files, but `git diff` shows nothing useful. You can't easily see what changed between two versions of a dataset.

**The solution:** A semantic diff tool for scientific data formats. `hdf-diff old.h5 new.h5` shows which datasets changed, summarizes numerical differences (mean delta, max delta, shape changes), and highlights structural changes. `git config diff.hdf5.command hdf-diff` integrates with git.

---

### 21. lablog

**The problem:** Lab notebooks are either paper (unsearchable) or heavyweight ELN software (slow, overengineered). Quick notes get lost.

**The solution:** A minimal lab notebook CLI. `lablog "Started synthesis of NiFe-LDH, 80°C, 12h"` timestamps and saves to a local SQLite database. `lablog search "NiFe"` finds entries. `lablog today` shows today's notes. `lablog export --bibtex` for when you write the paper. Syncs via git.

---

### 22. molsim

**The problem:** "Find molecules similar to this one" requires ChemDraw, SciFinder access, or writing RDKit code. Quick similarity searches shouldn't need a full cheminformatics setup.

**The solution:** A molecular similarity search CLI. `molsim "CCO" --db pubchem` searches PubChem for molecules similar to ethanol. Returns SMILES, names, and similarity scores. `molsim cluster molecules.smi` groups a set of molecules by structural similarity. Uses open APIs—no license needed.

---

### 23. xrdmatch

**The problem:** You have XRD peak positions and need to identify the phase. Current tools require expensive software or manual comparison with database PDFs.

**The solution:** A peak-matching CLI using open crystallography databases. `xrdmatch peaks.txt` compares your peaks against COD/AMCSD and ranks possible phases. `xrdmatch --cif sample.cif` simulates the pattern and identifies it. Outputs confidence scores and suggests additional peaks to measure for disambiguation.

---

### 24. vasp2qe

**The problem:** Moving calculations between DFT codes (VASP, Quantum ESPRESSO, CASTEP) requires manually rewriting input files. Each has different conventions and formats.

**The solution:** A DFT input converter. `vasp2qe POSCAR INCAR POTCAR -o pwscf.in` converts VASP inputs to Quantum ESPRESSO format. Handles k-points, pseudopotentials (with mappings), and common INCAR parameters. Warns about settings that don't translate directly. `qe2vasp` goes the other way.

---

### 25. jupyckpt

**The problem:** Jupyter kernel crashes and you lose hours of computed state. Large DataFrames, trained models, processed data—all gone.

**The solution:** A Jupyter state saver. `%load_ext jupyckpt` in your notebook enables automatic checkpointing. Every N minutes (or on demand with `%checkpoint`), it pickles all variables to disk. After a crash, `%restore` brings back your state. Shows what's checkpointed and how much space it uses.

---

### 26. chemstock

**The problem:** Lab reagent inventory lives in unmaintained spreadsheets. "Do we have sodium hydroxide? Where? Is it expired?" requires asking around.

**The solution:** A simple chemical inventory CLI. `chemstock add "NaOH" --location "Cabinet 3" --amount "500g" --expires 2025-06-01` tracks your stock. `chemstock find "sodium"` searches by name or formula. `chemstock expiring` shows what's expiring soon. `chemstock low` shows what's running out. Stores in git-friendly YAML.

---

### 27. ragchunk

**The problem:** Every RAG project starts with "how should I chunk these documents?" You experiment with sizes, overlap, and splitting strategies from scratch each time.

**The solution:** A smart document chunker with good defaults. `ragchunk document.pdf --strategy semantic` chunks intelligently based on content structure. `ragchunk benchmark corpus/ --query queries.txt` tests different strategies against your actual queries and shows retrieval quality. Outputs to formats all vector DBs accept.

---

### 28. run-compare (Web App)

**The problem:** Comparing ML experiment runs means flipping between TensorBoard tabs, or exporting CSVs and making plots manually. Side-by-side comparison of specific runs is tedious.

**The solution:** A local web app for experiment comparison. Drag-and-drop TensorBoard logs, W&B exports, or CSV files. Automatically aligns metrics by step/epoch. Interactive overlaid plots with hover comparison. Diff configs between runs. Shareable via static HTML export. No account required—runs entirely local.

---

### 29. slurm-insights (Web Dashboard)

**The problem:** You request 32GB RAM and 8 hours for every job because you don't know what you actually need. Your jobs wait in queue forever because you're over-requesting resources.

**The solution:** A web dashboard that analyzes your Slurm job history. Shows actual vs. requested resources (memory, time, CPUs). Identifies patterns: "Your `train_*.sh` jobs use 12GB avg but request 64GB." Suggests optimal resource requests. Estimates queue time savings. Runs on the cluster, queries `sacct` directly.

---

### 30. structure-view (Web App)

**The problem:** Visualizing crystal structures requires installing VESTA or other desktop software. Quickly checking a CIF file means downloading it, opening an app, and waiting for it to load.

**The solution:** A web-based crystal structure viewer. Drag-and-drop CIF/POSCAR/XYZ files or paste URLs. Interactive 3D visualization with symmetry operations, bond lengths, and coordination environments. Compare two structures side-by-side with automatic alignment. Export publication-ready figures. No installation, works on any device.

---

### 31. hpc-skeleton (Code Generator)

**The problem:** Every HPC job needs boilerplate: Slurm headers, environment setup, checkpoint handling, logging configuration, signal handling for preemption. You copy-paste from old scripts and hope it still works.

**The solution:** A project generator for HPC workflows. `hpc-skeleton init --cluster nersc --framework pytorch` creates a project with proper Slurm templates, automatic checkpointing, graceful preemption handling, logging setup, and environment management. Templates for common clusters (NERSC, OLCF, local Slurm). Knows each cluster's quirks.

---

### 32. queue-bot (Slack/Discord Bot)

**The problem:** You submit a job, then compulsively check `squeue` every 10 minutes. You want to know when jobs finish, fail, or finally start running—without manual monitoring.

**The solution:** A bot that watches your Slurm jobs and posts updates. `@queuebot watch 12345678` monitors a job. Posts when it starts, finishes, or fails (with exit code and last 20 lines of stderr). `@queuebot status` shows all your pending/running jobs. Runs on cluster login node, posts to your Slack/Discord.

---

### 33. mol-hover (Browser Extension)

**The problem:** Chemistry papers are full of SMILES strings and IUPAC names. You encounter `CC(=O)Oc1ccccc1C(=O)O` and have no idea what molecule it is without leaving the page.

**The solution:** A browser extension that makes molecules interactive. Hover over any SMILES, InChI, or common chemical name to see a 2D structure popup. Shows molecular weight, formula, and links to PubChem. Works on PubMed, arXiv, ChemRxiv, and any webpage. Highlights detected molecules on the page.

---

### 34. spec-annotate (Web App)

**The problem:** Annotating spectroscopy data (NMR, IR, MS, XRD) for group meetings or papers means screenshotting, importing to PowerPoint, and adding arrows manually. Collaboration means emailing files back and forth.

**The solution:** A web app for collaborative spectrum annotation. Upload common spectroscopy formats (JCAMP-DX, Bruker, CSV). Add peak labels, annotations, and regions interactively. Real-time collaboration like Google Docs. Export annotated figures as SVG/PNG. Share via link for group meeting discussions. Comment threads on specific peaks.

---

### 35. cite-fetch

**The problem:** You have a DOI and need a citation. BibTeX for your paper, APA for a report, or just a clean text reference. Each format requires a different website or tool, copy-pasting, and manual cleanup.

**The solution:** A citation fetcher that outputs any format. `cite-fetch 10.1038/nature12373` prints a clean citation. `cite-fetch 10.1038/nature12373 --format bibtex` for LaTeX. Supports DOI, arXiv ID, PubMed ID, or URLs. Batch mode: `cite-fetch --file dois.txt --format bibtex > refs.bib`. Cleans up publisher junk automatically.

---

### 36. env-diff

**The problem:** "It works on my machine." You and a collaborator have different Python environments and something breaks. Figuring out which package version differs means manually comparing `pip freeze` outputs.

**The solution:** A tool that diffs Python environments. `env-diff environment.yml other.yml` shows added, removed, and changed packages. `env-diff --remote user@server` compares your local env to a remote machine. `env-diff requirements.txt --current` shows drift from pinned versions. Highlights likely culprits for common errors.

---

### 37. traceback-buddy (Web App)

**The problem:** Python tracebacks are intimidating, especially for complex ML frameworks. You get 50 lines of JAX internals and have no idea what you actually did wrong. Stack Overflow requires translating your error into searchable terms.

**The solution:** A web app that explains Python errors. Paste your traceback, get a plain-English explanation of what went wrong and how to fix it. Uses LLMs with context about common libraries (PyTorch, NumPy, pandas). Shows similar errors from GitHub issues. Highlights the line that's actually your fault vs. library internals. No login required.

---

### 38. notebook-clean

**The problem:** Jupyter notebooks grow into 2000-line monsters. Functions that should be importable modules are trapped in cells. Refactoring is manual, tedious, and breaks execution order.

**The solution:** A notebook refactoring tool. `notebook-clean analyze notebook.ipynb` identifies functions/classes that could be extracted. `notebook-clean extract notebook.ipynb --to src/` pulls them into proper Python modules, adds imports, and updates the notebook. `notebook-clean lint notebook.ipynb` warns about common issues: cells that depend on later cells, undefined variables, unused imports.

---

### 39. param-sweep

**The problem:** Running hyperparameter sweeps means writing scripts to generate all config combinations, naming output directories consistently, and tracking which runs you've completed. You rewrite this boilerplate for every project.

**The solution:** A parameter sweep generator. Define a base config and parameter ranges: `param-sweep config.yaml --vary "lr:[1e-4,1e-3,1e-2]" --vary "batch_size:[32,64,128]"` generates all 9 configs with systematic naming. `param-sweep status sweep/` shows which configs have completed runs. `param-sweep next sweep/` prints the next config to run. Integrates with Slurm: `param-sweep submit sweep/ --template slurm.sh`.

---

### 40. dataset-contract

**The problem:** Data teams hand off Parquet/JSON dumps with unwritten assumptions. A partner silently renames a column, switches units, or broadens a categorical domain and your training job fails hours into a run.

**The solution:** A CLI that snapshots dataset "contracts." `dataset-contract capture s3://bucket/train.parquet --profile 50k` infers schema, units, null thresholds, histogram fingerprints, and referential links. `dataset-contract check new.parquet contract.yaml` runs in CI, blocking releases when invariants drift. Generates human-readable diffs for reviewers and JSON/Great-Expectations exports for existing tooling.

---

### 41. module-map

**The problem:** Reproducing an HPC workflow from someone else's `module load ...` snippet is trial-and-error. Modules pull in hidden dependencies, clash on compiler toolchains, and break when you move clusters.

**The solution:** `module-map trace train.sh` records every module load, resolves dependency trees via `module spider`, and outputs a portable manifest (YAML + DOT). `module-map replay manifest.yaml --cluster slac` reorders loads to match the target site, warns about ABI mismatches, and proposes container images when modules don't exist remotely. Integrates with documentation generators so papers can cite an exact software stack.

---

### 42. sim-cache

**The problem:** You rerun the same expensive CFD/MD simulation because the exact input deck or mesh hash isn't tracked. Hours of GPU/CPU time are burned just to rediscover the same result.

**The solution:** `sim-cache run --cmd "mpirun ./solver" --inputs deck.yaml mesh.msh --metrics lift,drag` fingerprints every input file, code git hash, and solver flags. Results land in a content-addressed cache with metadata. `sim-cache query --near deck.yaml --lift 0.8-0.82` lets collaborators discover prior runs, compare metrics, or warm-start new simulations using cached fields.

---

### 43. sample-link

**The problem:** Physical samples, sequencing data, and analysis notebooks drift apart. A vial labeled "NiFe-42" has three spreadsheets, two FASTQ folders, and no canonical mapping.

**The solution:** A CLI/web hybrid for durable sample IDs. `sample-link issue NiFe --operator ali` generates globally unique IDs, QR/Datamatrix labels, and a metadata stub. `sample-link attach NiFe-042 data/fastq/` links files, instruments, and processing steps; `sample-link search NiFe electrolyte` surfaces every derived artifact. Exports to ELNs or LIMS when one eventually appears.

---

### 44. eval-sandbox

**The problem:** Spinning up a trustworthy LLM evaluation harness takes a day of boilerplate—dataset loaders, prompt templates, metrics, judge models. Quick what-if experiments never happen.

**The solution:** A local-first web app. Drag in a JSONL dataset, define prompt templates and guardrails, then point at OpenAI, local vLLM, or custom HTTP backends. `eval-sandbox bench qa.jsonl --judge gpt-4o-mini --metrics bleu,faithfulness` runs hermetic, reproducible evals, tracks prompt/model versions, and exports leaderboards plus error sets you can re-test later.

---

### 45. gpu-budget

**The problem:** GPU time feels free until invoices land. Teams overrun budgets because there is no live view of cluster usage versus spend, especially when mixing on-prem Slurm, cloud spot fleets, and managed services.

**The solution:** `gpu-budget ingest slurm sacct.csv aws-billing.csv` normalizes job logs and cloud bills into a unified ledger. The web dashboard shows cost per project, researcher, and model family, forecasting burn-rate from queued jobs. Alerts in Slack when a project exceeds allocations, and exports finance-friendly summaries each month.

---

## Resolved

These issues have been solved by the community or existing tools.

### unitcalc

**The problem:** Scientific code silently mixes units. You add a value in eV to one in Hartree and get nonsense. Bugs hide for months.

**The solution:** A unit-aware calculator CLI. `unitcalc "5 eV + 0.1 Hartree in kcal/mol"` does the conversion. Refuses to add incompatible units. Knows physical constants with proper units. Can lint Python files to find suspicious dimensionless arithmetic: `unitcalc lint simulation.py`.

---

### arxiv-plus (Browser Extension)

**The problem:** ArXiv paper pages are bare-bones. You constantly open new tabs to find the code repo, check citations, see if there's a blog post, or find the OpenReview discussion.

**The solution:** A browser extension that enhances arXiv pages. Automatically detects and links to GitHub repos (via Papers With Code API). Shows citation count. Adds one-click BibTeX copy. Links to Semantic Scholar, Connected Papers, and OpenReview if available. Shows if your institution has the published version.

---

### tensor-shapes (VS Code Extension)

**The problem:** Tensor shape errors are the most common bugs in ML code. You stare at `x = model(x)` and have no idea what shape `x` is without adding print statements and re-running.

**The solution:** A VS Code extension that shows tensor shapes inline. Runs your code once in debug mode to capture shapes, then displays them as inline annotations: `x = model(x)  # [32, 768]`. Updates as you edit. Hover for full dtype and device info. Works with PyTorch, TensorFlow, JAX, and NumPy.

---

### grove

**The problem:** Modern development often requires working on multiple branches simultaneously—reviewing a PR while fixing a bug, comparing implementations, or running parallel AI coding sessions. Git worktrees solve this, but for JS/TS projects they're painful: each worktree needs its own `node_modules`, meaning slow installs, gigabytes of duplication, and losing your editor/AI tool configs.

**The solution:** A git worktree manager with smart dependency handling. `grove create feature-x` creates a worktree with shared `node_modules` (via pnpm store or symlinks when lockfiles match). Automatically copies `.claude/`, `.cursor/`, `.vscode/`, and other tool configs. `grove list` shows all worktrees with their branches and status. `grove serve feature-x --port 3001` runs dev servers on different ports. First-class support for Claude Code, Cursor, and other AI tools.

**Solved:** [grove](https://github.com/blaiszik/grove) by [@blaiszik](https://github.com/blaiszik)

---

### license-guardian

**The problem:** You're about to release a dataset/model bundle but can't tell whether mixing CC-BY-SA text, GPL code, and proprietary weights is legally compatible. Manual audits are painful and error-prone.

**The solution:** `license-guardian scan ./release` crawls source, dataset metadata, and embedded archives, fingerprints likely licenses, and produces a compatibility matrix keyed to your distribution plan (research-only, commercial, internal). Highlights blocking clauses (e.g., share-alike contagion), suggests remediation (relabel, request exceptions), and emits SPDX plus human-readable reports for legal review.

**Solved:** [ScanCode Toolkit](https://github.com/nexB/scancode-toolkit) by [@nexB](https://github.com/nexB)

---

### notebook-ci

**The problem:** Research notebooks ship with stale outputs and hidden state. CI pipelines usually skip them because they require GPUs or long runtimes, so regressions surface only when a collaborator reruns cells.

**The solution:** A CLI for hermetic notebook tests. `notebook-ci run notebooks/ --matrix cuda=11.8 python=3.11` provisions disposable envs, pre-downloads datasets, executes notebooks with timeouts, and surfaces diffed outputs inline in PR comments. Caches expensive steps (like model checkpoints) across runs and emits HTML artifacts for reviewers.

**Solved:** [nbmake](https://github.com/ReviewNB/nbmake) by [@ReviewNB](https://github.com/ReviewNB)

---

### paperdiff

**The problem:** Comparing v1 and v2 of a manuscript means eyeballing two PDFs. Figure updates, reference renumbering, and small textual edits slip past reviewers.

**The solution:** `paperdiff v1.pdf v2.pdf` aligns PDFs, matches figures/paragraphs even when pagination shifts, and outputs a heatmap PDF that highlights insertions, deletions, and moved blocks. CLI flags let you ignore bibliography changes or focus on equations only. Generates markdown summaries suitable for arXiv submission notes.

**Solved:** [diff-pdf](https://github.com/vslavik/diff-pdf) by [@vslavik](https://github.com/vslavik)

---

## The Bin (Low Feasibility/Quality)

Ideas that are likely too complex, niche, or effectively solved by standard admin tools.

### synthparse

**The problem:** Synthesis procedures are buried in paper methods sections as prose. Extracting temperatures, times, and quantities for your own use is manual and error-prone.

**The solution:** A synthesis procedure parser. `synthparse methods.txt` extracts structured data: reagents, quantities, temperatures, durations, and steps. Outputs JSON or a step-by-step protocol. `synthparse --compare a.txt b.txt` highlights differences between procedures. Uses LLMs for parsing, with human-reviewable output.

---

### gpu-fleet (Menubar App)

**The problem:** You have access to multiple machines with GPUs (lab servers, cloud instances, your desktop). Checking what's available requires SSH-ing into each one and running `nvidia-smi`.

**The solution:** A lightweight menubar/tray app that monitors GPU availability across machines. Shows memory usage, running processes, and temperature at a glance. Click to SSH directly into a free machine. Alerts when a GPU becomes free. Works over SSH with minimal setup—no daemon required on remote machines.

---

### driver-sync

**The problem:** Multi-node GPU fleets accumulate mismatched NVIDIA drivers, CUDA toolkits, and MIG firmware. One stale node makes your container fail only after the scheduler lands a job there.

**The solution:** A lightweight daemon plus CLI. `driver-sync agent` reports driver/toolkit hashes from each node; `driver-sync status` highlights drift versus the blessed stack, links to install commands, and can cordon offending nodes via Slurm/Kubernetes annotations. Generates upgrade plans and calendars, so you can roll drivers without guessing who will be disrupted.

---

## Contributing

### Proposing an Issue
Open an issue describing:
- The friction (what's painful today)
- The shape of a solution (what would the tool do)
- Why existing tools fall short

### Solving an Issue
1. Build the tool
2. Open a PR adding a link under the issue description
3. Move it to "Resolved" with credit
