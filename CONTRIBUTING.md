# Contributing to Resolve

Resolve is a **registry**, not a repository. We don't host the tools -- we track what needs to be built and celebrate what gets built. Think of it as a community-curated backlog of research papercuts that are *just waiting* for someone like you to fix them.

Whether you're a researcher who's tired of losing hours to avoidable friction, or a developer looking for a meaningful project with real users on day one -- you're in the right place.

---

## Why Contribute?

Building a Resolve solution isn't just community service. It's one of the highest-leverage things you can do for your own career and skills:

**For builders and developers:**
- **Ship something real.** Every Resolve issue has a defined scope, a clear user, and a measurable impact. No guessing whether anyone needs it -- they already told you they do.
- **Build your portfolio.** A working tool with real users is worth more than a hundred toy projects. Resolve solutions get linked, starred, and cited.
- **Find your people.** Some of the best collaborations start with "I'll take the CLI, you take the parser." Resolve is where builders meet.
- **Learn by doing.** Tackling a problem outside your usual domain (HPC? cheminformatics? figure compliance?) stretches your skills in ways that are hard to get otherwise.

**For researchers:**
- **Turn frustration into action.** That thing that wastes your time every week? Write it up. Someone *will* build it.
- **Shape the tool you'll actually use.** By proposing and refining the friction, you're designing the solution from the user's perspective.
- **Connect with builders.** The developer who solves your problem might become your next collaborator.

**For teams:**
- **Hackathon-ready scopes.** Every issue is designed to be solvable in 1-5 days -- perfect for a sprint, a hackathon, or a weekend with your lab mates.
- **Shared credit.** Everyone who contributes gets named. We don't absorb your work -- we celebrate it.

---

## How to Propose a New Friction

Found a recurring problem that makes you want to flip a table? Good. Channel that energy.

### Before you propose, check:
- [ ] **No existing solution** -- Search PyPI, GitHub, and the web. If something *almost* works, explain the gap.
- [ ] **Specific friction** -- "Data pipelines are annoying" is not a friction. "SDF files with invalid valence silently corrupt downstream datasets" is.
- [ ] **Buildable scope** -- A skilled developer (or small team) could build an MVP in 1-5 days.
- [ ] **Clear value** -- You can estimate how many hours per year this saves, and the number isn't made up.

### Submitting your proposal

Use the **["Propose a friction"](../../issues/new?template=propose-friction.yml)** issue template. It'll walk you through everything.

If you prefer to draft manually, follow this format:

```markdown
### N. tool-name - Short description (~X hrs/year)
> **Impact:** ~X hours saved per [user type] per year

**The friction:** [1-3 sentences. Be specific and visceral. Make the reader wince.]

**Why existing tools don't solve it:** [1-2 sentences. Name names.]

**The solution:** [1 sentence + CLI examples]

**Status:** 🔴 Open
```

The best proposals make the reader think: *"Oh, I've had that exact problem."*

---

## How to Build a Solution

This is the fun part. You found an issue that speaks to you. Now let's fix it.

### Solo or team -- both welcome

Some issues are perfect for a solo weekend sprint. Others benefit from a team with mixed skills (a domain expert + a CLI developer, for example). There's no wrong way to do it.

### The process

1. **Claim it.** Use the ["Start building"](../../issues/new?template=start-building.yml) template so others know the issue is being worked on. This prevents duplicate effort and lets potential collaborators find you.
2. **Build it.** Create your solution in your own repo. You own the code, the license, and the credit.
3. **Ship it.** When it works, use the ["Link a solution"](../../issues/new?template=link-solution.yml) template to connect your repo back to Resolve.
4. **Get featured.** We'll add you and your team to the Resolved section with full credit.

### What makes a good solution

Before linking your solution, check that it:

- **Actually solves the friction** -- Someone with the original problem can install it and use it today. Not "in theory." Today.
- **Has a clear README** -- Installation, basic usage, and at least one real example. Bonus points for a GIF or screenshot.
- **Works without extensive setup** -- Minimize dependencies and configuration. If it takes longer to install than to use, something went wrong.
- **Has a license** -- MIT, Apache-2.0, BSD, etc. Open source is the whole point.

---

## Forming a Team

Some of the best tools come from small teams where each person brings a different angle. Here's what we've seen work well:

### Finding teammates

- **Comment on the issue.** The simplest way. Drop a comment like "I'm interested in tackling this -- anyone want to team up?" on any open issue.
- **Bring it to your community.** Share a Resolve issue with your lab, your Slack, your Discord. The issue description is designed to be self-contained -- anyone can read it and understand the problem.
- **Look for complementary skills.** The best Resolve teams pair people who *have the problem* with people who *love building CLIs*. A researcher who knows the domain + a developer who knows the tooling = a tool that actually gets used.

### Team best practices

**Start with a quick sync.** Before writing any code, spend 30 minutes aligning on scope. The issue description is your starting point, but you'll want to agree on:
- What's in the MVP and what's "later"
- Who owns which pieces
- How you'll communicate (a shared channel, a GitHub project board, async in issues -- whatever works)

**Divide by capability, not by file.** Instead of splitting the codebase arbitrarily, divide by what each person is best at:
- One person handles the core logic (parsing, validation, computation)
- One person handles the CLI interface, packaging, and distribution
- One person writes tests and documentation from the user's perspective

**Ship the smallest thing first.** Resolve issues are already scoped to 1-5 day MVPs, but it's still tempting to gold-plate. Resist. Get a working `v0.1` out, link it back to Resolve, and iterate based on real feedback. A rough tool that exists beats a polished tool that doesn't.

**Give credit generously.** Add all contributors to your README, your release notes, and your Resolve PR. Use co-authored commits. Tag people in the solution link. Recognition is free and it keeps teams together.

### Team sizes that work

| Size | Best for |
|------|----------|
| **Solo** | Well-scoped issues where you have both the domain knowledge and the dev skills. Weekend projects. |
| **Pair (2)** | One researcher + one developer. One backend + one CLI/UX. The most common and often the most effective. |
| **Small team (3-4)** | Broader-scope issues that span multiple domains or need parallel work streams. Hackathon teams. |
| **Existing project** | An open source project that wants to absorb a Resolve issue into their roadmap. Bring the issue to your community and build it where the infrastructure already exists. |

---

## Status Legend

| Status | Meaning |
|--------|---------|
| 🔴 Open | No known solution -- ready for someone to pick up |
| 🟡 In Progress | Someone (or a team) is building it -- check the issue for details |
| 🟢 Solved | Working solution exists -- linked in the Resolved section |

---

## Code of Conduct

Be kind, be specific, be constructive. We're all here because we've been frustrated by the same kinds of problems. That shared frustration is what makes this community work -- let's keep it focused on fixing things, not on gatekeeping who gets to fix them.

If you see something that could be better about Resolve itself (the templates, the process, this document), open a PR. We iterate on the registry just like you iterate on the tools.
