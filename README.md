# Verified Research

A Claude skill for research you can check. It finds the evidence, writes the answer, then has someone who didn't write it re-open every cited source and confirm the source says what the answer claims. You get a sourced report with calibrated confidence, plus an honest list of what couldn't be verified.

It works in Claude Code and in the Claude app, and triggers on its own when you ask Claude to research, compare, fact-check, or "find out whether…", even if you never say "research".

## Why

Research agents rarely invent links anymore. Their common failure is a real link that doesn't say what the claim says: a number from a different year, a narrower population, a "may" turned into "does". More searching doesn't fix that; checking does. So this skill treats verification as its own phase, done with fresh eyes, and keeps research inside a budget so the checking stays affordable.

## How it works

It sizes the job first:

| Tier | For | What happens |
|---|---|---|
| Quick | One fact, version, date, "is X still true?" | A few searches, the primary source, a 150–200 word answer with its date |
| Standard | Comparisons, evaluating a tool or vendor, fact-checking a claim | 2–4 researchers in parallel, an audit of every citation, an independent verifier |
| Deep | Surveys, literature reviews, contested or high-stakes decisions | 4–6 researchers, a red team that hunts for the case against, then verification |

Along the way it:

- records every piece of evidence as a card with a **verbatim quote**, the source, its date and a source tier (primary, strong secondary, weak, leads only);
- counts **independent origins**, not URLs, so five articles repeating one press release count once;
- runs **one** gap-filling round, then stops: past that point more searching adds errors faster than accuracy;
- challenges the conclusion with a **red team** before verification, so one verification pass covers everything;
- has a **verifier** label each claim SUPPORTED, PARTIAL, NOT SUPPORTED, CONTRADICTED or UNREACHABLE, and fixes the report to match;
- uses fixed likelihood words (ICD 203) and a separate High / Moderate / Low confidence label;
- ends with **what this means for you**, the disagreements in the evidence, what would change the conclusion, and the gaps.

Without a subagent tool it runs the same phases on its own, with tighter budgets.

## Install

### Claude Code (as a plugin)

In a Claude Code session:

```
/plugin marketplace add JalalD/verified-research
/plugin install verified-research@verified-research
```

Or from your shell:

```bash
claude plugin marketplace add JalalD/verified-research
claude plugin install verified-research@verified-research
```

### Claude Code (as a plain skill)

Copy `skills/verified-research/` into `~/.claude/skills/` (or a project's `.claude/skills/`).

### Claude app (claude.ai and desktop)

1. Download `verified-research.zip` from the [latest release](https://github.com/JalalD/verified-research/releases/latest), or zip the `skills/verified-research` folder yourself.
2. In Claude, go to **Customize › Skills**, click **+**, then **Upload a skill**, and pick the zip.
3. "Code execution and file creation" must be turned on for skills to run.

## Use it

Just ask. For example:

- "Is it true that developers spend 50% of their time debugging? Where does that number come from?"
- "Supabase vs Firebase for a 4-person B2B SaaS team: cost at 20k users, data residency, SSO, lock-in. Which should we pick?"
- "What do the randomized trials say about intermittent fasting versus plain calorie restriction?"
- "What does Jordan's Personal Data Protection Law require of a small online store?"

A Standard or Deep run writes its working files to `research/<date>-<topic>/`: the plan, each researcher's evidence cards, the red team's findings, the claims checklist, the verifier's results and the final `report.md`.

## What's inside

```
skills/verified-research/
├── SKILL.md                     # the workflow: tiers, phases, ground rules
├── references/
│   ├── researching.md           # researcher manual: search craft, source tiers, evidence cards
│   ├── briefs.md                # templates for researcher, verifier and red-team briefs
│   ├── verification.md          # how to check a claim against its source
│   ├── analysis.md              # key assumptions, competing hypotheses, premortem, red team
│   ├── report-templates.md      # report structures, confidence vocabulary, citation rules
│   └── question-types.md        # playbooks: comparison, legal, medical, market, fact-check…
└── scripts/                     # Python 3, standard library only
    ├── evidence.py              # validates evidence cards and summarizes coverage
    ├── audit_report.py          # builds the claims checklist; flags uncited claims and unknown URLs
    └── check_links.py           # live link and quote check (needs internet from the shell)
```

## How it was tested

Built and tuned with Anthropic's skill-creator workflow:

- **Five realistic tasks** (a quick version lookup, tracing a statistic, a vendor comparison, an evidence review, Jordanian data-protection law in Arabic), each run with and without the skill and graded against written checks. With the skill, 98% of checks passed in both test rounds; without it, 74%. See [`evals/`](evals/).
- **Trigger tuning** on 20 requests, half of which sound like research but aren't (interview scripts, summarizing a file you supplied, "investigate" a failing test). The description triggered on 19 of 20 correctly and never on the look-alikes.
- **A live parallel run** (3 researchers, 2 gap-fillers, a red team, 2 verifiers) on a real vendor decision. The red team overturned one recommendation, and the verifiers reworded 12 of 35 claims to match their sources.

The cost: on research questions it takes roughly 1.7–2.1× the time and 1.6–1.8× the tokens of a plain answer, and a parallel run uses several times more. That's the price of the checking. Quick questions stay quick.

## Privacy and network use

- The plugin has no server and no MCP connectors. Nothing is sent to the author or to any service run by the author, and nothing is retained outside your own machine.
- Searches and page reads go through your Claude client's built-in web search and fetch tools.
- `scripts/check_links.py` (optional) sends plain HTTP requests from your machine to the URLs cited in a report, to check that they load and contain the quoted text.
- A Standard or Deep run writes its working notes and report as local files under `research/` in your working folder. Delete them whenever you like.
- If you ask about your own work, it may read your connected apps (Drive, email and similar) through connectors you have already set up, and it never puts private details into web searches.

Full policy: [PRIVACY.md](PRIVACY.md).

## Limits

- It can only cite what it can open. Paywalled papers and sites that block automated access limit what it can verify, and it says so.
- Fetch tools that summarize pages can paraphrase; the skill asks for verbatim text and re-checks wording that matters, but a human should still read the sources behind a high-stakes decision.
- Legal, medical and financial reports are research, not advice.

## License

[MIT](LICENSE)
