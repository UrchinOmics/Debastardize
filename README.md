# debastardize

**Audit scientific code and manuscripts for statistical methods named after people who caused serious harm. Using agentic AI, identify those people, learn about the history, and plan alternatives — without touching your files.**

---

## The problem

A lot of the statistical methods in use today were built by awful people. The methods named for these awful people and the scientific community's history of ignoring the problematic people in our past mean that researchers today unknowlingly honor the names of monsters: people like Francis Galton, Karl Pearson, and R.A. Fisher. Their names are everywhere in scientific code and manuscripts — in correlation coefficients, chi-squared tests, exact tests, ANOVA, the F-distribution, p-values.

The math works regardless of who developed it. But when an equally valid or better alternative exists, researchers deserve to know they have a choice. Researchers should be aware of the dark history of these people, and when no alternative exists, researchers deserve to know whose name - and what history - they carry forward. We often have no choice - the entire frequentist paradigm is ____, for example. But we can always be informed, and we can work to ensure that their names remain linked to their actions. 

History also has some hidden gems for us and researchers deserve to know about them too - people like David Blackwell, who was initially denied a position at Berkeley in the early 1940s because the department chair's wife wouldnt have a black person in her home, and who, despite that, was hired in 1955 as a full professor in the Berkeley department of statistics, becoming first Black scholar tenured at UC Berkeley. He was appointed department chair of that same department the following year. He became the first Black member of the National Academy of Sciences, and is the namesake of the Rao-Blackwell theorem. He made foundational contributions to game theory, Bayesian statistics, and information theory. He was appointed to the the American Academy of Arts and Sciences, received the von Neumann Prize, the National Medal of Science, and published across more fields than most statisticians touch in a lifetime. Once turned away for the color of his skin, he became one of the greatest statisticians of the twentieth century. 

## What this does

`debastardize` is a prompt for agentic AI, inducing an agent to:

1. **Scan** your codebase and/or manuscripts for eponymous statistical methods
2. **Research** the people those methods are named after
3. **Informs** you about their history — the good, the bad, and the complicated — with citations that you can check (so you needn't depend on the accuracy of the AI model).
4. **Plans** alternatives where statistically appropriate replacements exist. 
5. **Produces** a `history.md` file you can include in your repo, giving context to the people whose work your code builds on. 

It does not modify your files. It produces a plan. You decide what to do with it.

## Quick start

Clone this repo into or alongside your project:

```bash
git clone https://github.com/YOUR_USERNAME/debastardize.git
```

Then point your agentic AI tool at your project and give it the prompt:

**Claude Code:**
```bash
cd ~/your-project
claude
# reference debastardize.md as your task
```

**OpenAI Codex:**
```bash
codex --instructions path/to/debastardize.md
```

**Cursor / Aider / other agents:**
Open your project, add `debastardize.md` to the agent's context or system prompt, and ask it to begin.

The agent will ask you two questions — what to scan and what your field is — then get to work. Everything about what to *do* with the findings comes after you've seen them.

## What's in this repo

| File | What it is |
|---|---|
| [`debastardize.md`](debastardize.md) | The prompt. Give this to your agent. |
| [`histories/`](histories/) | Biographical entries for known figures — problematic and otherwise. One file per person. The agent consults these as needed, avoiding API refusal to discuss difficult historical topics. |
| [`histories/values.txt`](histories/values.txt) | Tone and style guide for biographical entries. Used by subagents when researching new figures. |
| `README.md` | You're reading it. |
| `LICENSE` | CC BY-SA 4.0. |

## What the agent produces

After scanning and presenting histories of the people behind your methods, the agent is directed to write new files (never modifying yours):

| Output | Description |
|---|---|
| `history.md` | Always produced. A standalone acknowledgment of every eponymous method in your project and who those people were. Commit it alongside your code if you choose. |
| `debastardize_plan.md` | For code: a table of findings and suggested changes with inline diffs. If you choose, you can have an agent execute this plan, though you should carefully and critically evaluate it. |
| `debastardize_manuscript/` | For manuscripts: suggested edits, footnotes, `references.ris`, and educational summaries. |

## Scope

This is not limited to eugenicists. The agent flags anyone with a documented history of:

- Eugenics and forced sterilization campaigns
- Scientific racism and white supremacy
- Nazism, holocaust denial, or apologism
- Sexual predation
- Malignant misogyny
- Colonial exploitation framed as science
- Data fabrication in service of persecution

It also tells you about the people who did no harm — because a brewer who invented the t-test to make better beer is a story worth knowing too.

## Philosophy

This is about learning our shared history first and foremost, and secondly about empowering researchers to make informed choices. "Product of their time" is not a defense when the person was an outlier among contemporaries, held institutional power that amplified their views, or continued advocating harmful positions after those positions were widely rejected. and while someone may well be a product of their time, their time has no bearing on the fact that we have a choice here and now about how we confront their legacies.

The agent's most important job is education. If you review the history and decide to change nothing, that's your call. The information was the point.

## Contributing

This is a living document. If you know of a figure who should be added to `histories/` — problematic or otherwise — open a PR with sources. If you find a better statistical alternative for a flagged method, suggest it. If you've used this on your own codebase and have feedback on the prompt, open an issue.

## License

[CC BY-SA 4.0](LICENSE) — Use it, fork it, adapt it, but please credit me and keep derivative work accessible to all. 
