# Contributing to Resolve

Resolve is a **registry**, not a repository. We don't host the tools - we track what needs to be built and celebrate what gets built.

## Proposing a New Issue

Before proposing, verify:
- [ ] **No existing solution** - Search PyPI, GitHub, and the web
- [ ] **Specific friction** - Problem is concrete, not vague
- [ ] **Buildable scope** - A skilled developer could build an MVP in 1-5 days
- [ ] **Clear value** - Impact estimate is defensible

### Issue Format

Prefer the GitHub **"Propose a friction"** issue template. If you must draft manually, use this format:

```markdown
### N. tool-name
> **Impact:** ~X hours saved per [user type] per year

**The friction:** [1-3 sentences. Be specific and visceral.]

**Why existing tools don't solve it:** [1-2 sentences.]

**The solution:** [1 sentence + CLI examples]

**Status:** 🔴 Open
```

## Building a Tool

When you build a solution:
1. Create your own repo
2. Open a PR here updating the issue status to "🟡 In Progress" or "🟢 Solved"
3. Link to your repo

We'll feature working solutions in the **Resolved** section with full credit.

## The Resolve Standard

Tools that earn a place in Resolve should aim for:
- **Zero Configuration** - Works instantly
- **Beautiful Output** - CLIs are user interfaces
- **Speed** - Faster than thought
- **Privacy** - Data stays local unless explicitly needed

## Status Legend

| Status | Meaning |
|--------|---------|
| 🔴 Open | No known solution |
| 🟡 In Progress | Someone is working on it |
| 🟢 Solved | Working solution exists |
