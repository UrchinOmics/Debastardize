# debastardize

**Audit scientific code and manuscripts for statistical methods named after people who caused serious harm. Using agentic AI, identify those people, learn about the history, and plan alternatives — without touching your files.**

---

## The problem

Every time a researcher writes fisher.test(), they invoke — whether they know it or not — the name of a man who campaigned to sterilize 17% of the British population. When they compute a Pearson correlation, they honor a man who wrote that nations advance "by way of war with inferior races." The foundations of modern statistics were laid by Francis Galton, Karl Pearson, and R.A. Fisher — men whose names are everywhere in scientific code and manuscripts, in correlation coefficients, chi-squared tests, ANOVA, the F-distribution, p-values — and whose legacies the scientific community has largely chosen not to confront. The math works regardless of who developed it. Nobody argues otherwise. But when an equally valid or better alternative exists, researchers deserve to know they have a choice. And when no alternative exists — the entire frequentist paradigm was built by these men, and there is no escaping that — researchers deserve to know whose names they carry forward and what those names represent. We can't always avoid the methods. But we can always be informed, and we can ensure that truth prevails and that these names stay linked to the full record of the person's actions.

Not everyone in this history is a villain, and knowing the stories behind those people is important too. For example, David Blackwell was denied a faculty position at Berkeley in the early 1940s because the department chair's wife refused to have a Black person in her home. A little over a decade later, he was hired as a full professor in that same department — the first Black scholar tenured at UC Berkeley. The year after that, he was appointed its chair. He became the first Black member of the National Academy of Sciences, the namesake of the Rao-Blackwell theorem, and made foundational contributions to game theory, Bayesian statistics, and information theory. He was elected to the American Academy of Arts and Sciences, received the von Neumann Prize and the National Medal of Science, and published across more fields than most statisticians touch in a lifetime. The institutions that once turned him away for the color of his skin now name buildings after him. He became one of the greatest statisticians of the twentieth century.

His story deserves to be told too. 

## What this does

`debastardize.md` is a prompt for agentic AI, inducing an agent to:

1. **Scan** your codebase and/or manuscripts for eponymous statistical methods
2. **Research** the people those methods are named after
3. **Inform** you about their history — the good, the bad, and the complicated — with citations that you can check (so you needn't depend on the accuracy of the AI model)
4. **Plan** alternatives where statistically appropriate replacements exist
5. **Produce** a `history.md` file you can include in your repo, giving context to the people whose work your code builds on

It does not modify your files. It produces a plan. You decide what to do with it.

## Quick start

Clone this repo into or alongside your project:

```bash
git clone https://github.com/UrchinOmics/Debastardize.git
```

Then point your agentic AI tool at your project and give it the prompt:

**Claude Code:**
```bash
cd ~/your-project
claude
# reference debastardize.md as your task
```

**Claude Code (slash command):**
```bash
# Install once — copies the prompt into your project's command directory
mkdir -p .claude/commands
cp path/to/Debastardize/debastardize-command.md .claude/commands/debastardize.md
cp -r path/to/Debastardize/histories/ ./histories/

# Then use it in any session
claude
/project:debastardize
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
| [`debastardize-command.md`](debastardize-command.md) | Same prompt, stripped for use as a Claude Code slash command. |
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

## Agentic AI use in Writing

Claude Opus 4.6 was used to build initial histories, and to format and edit the prompt. Claude Opus 4.6 and OpenAI GPT5.2 codex were used to evlauate performance by evaluating some existing codebases and manuscripts. 

##

The IDEA course in Duke's Biology department, its instructor (Danae Diaz) and co-instructor (Nina Sherwood) and some IDEA committee members (Aeran Coughlin) played a large part in opening my eyes to the history behind these methods, and for that, have my sincere gratitude. :)

## License

[CC BY-SA 4.0](LICENSE) — Use it, fork it, adapt it, but please credit me and keep derivative work accessible to all. 
