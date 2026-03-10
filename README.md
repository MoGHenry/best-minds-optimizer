# Best Minds — Prompt Optimizer

Every question you ask an LLM gets a generic answer. The same question, rewritten through Charlie Munger's mental models or Blair Enns' pricing frameworks, gets a fundamentally different — and better — one.

This agent skill does the rewriting automatically. You ask a question, it identifies the world's best expert for your specific problem, rewrites your prompt through their frameworks, and delivers the answer in plain English with a concrete next step.

## Before and After

### Pricing Strategy

**You type:** *"I'm thinking about raising prices for my freelance design work but I'm scared of losing clients"*

**Without this skill** — You get blog-post advice: "raise incrementally", "be confident." No frameworks, no reframing.

**With this skill** — Selects **Blair Enns** (author of *Pricing Creativity*). Rewrites your question into 5 diagnostic sub-questions, then delivers:

> **The fear is almost always bigger than the risk.** If you raise prices 30% and lose 20% of clients, you're ahead — fewer clients, more revenue, less management overhead. The clients you lose at higher prices are the ones who haggle on scope, pay late, and drain your energy. Raising prices is a filter, not a risk.

Includes the attrition math, a client segmentation framework (who to raise first), exact language to use in the conversation, and ends with: *"List your top 5 clients — which one would you actually lose at 25% higher?"*

### Hiring Decisions

**You type:** *"We're a 15-person startup that just raised Series A. Should we hire a VP of Engineering or promote our senior engineer who's been with us since day one?"*

**Without this skill** — A balanced pros-and-cons list. Reasonable but doesn't reframe the decision.

**With this skill** — Selects **Ben Horowitz** (*The Hard Thing About Hard Things*). Reframes the entire question:

> **This isn't about loyalty vs. capability. It's about one testable threshold: has she made other engineers better, or is she still the person who fixes the hardest bugs herself?** Managing 3 people is IC-plus. Managing 20+ requires a fundamentally different skill.

Surfaces a third option most founders miss (90-day eval with executive coaching), warns against the worst outcome ("promoting her into an ambiguous VP title while simultaneously recruiting externally"), and gives the next step: *"Have a direct conversation with her this week — tell her what the VP role requires at Series A scale and ask how she'd approach it."*

### Polishing Your Own Writing

**You type:** *"For step 2, MUST breakdown the optimized prompt and then review all available Agent Skills. Check if any of the Agent Skills is helpful for completing the task."*

**With this skill** — Recognizes this is user-authored prose, not a question. Applies a quick polish pass without the full expert pipeline:

> *"After step 1, break down the optimized prompt and review all available Agent Skills. For each skill that matches the task, emit the skill name and a one-sentence reason it was triggered."*

Tightens wording, fixes grammar, improves parallel structure — preserves your voice and intent.

### Trivial Tasks

**You type:** *"Read the package.json file in this directory"*

**With this skill** — Just reads the file. No expert, no rewrite, no friction. The skill knows when to stay out of the way.

## How It Works

Every prompt hits a 4-lane triage, then routes to the appropriate pipeline:

```
Your question
    │
    ▼
┌─────────────────────────────────┐
│  TRIAGE                         │
│  Which lane fits this prompt?   │
├─────────┬─────────┬──────┬──────┤
│  Skip   │ Polish  │Clarify│Optimize
│         │         │      │      │
│ Proceed │ Quick   │ Ask  │  ▼   │
│ as-is   │ wording │ MCQs │┌─────┴───────────────┐
│         │ pass    │ then ││ LOGIC MAPPING       │
│         │         │  ▼   ││ Classify problem:   │
│         │         │Optimize│ Bottleneck, Resource│
│         │         │      ││ Direction, Execution│
│         │         │      ││ Tradeoff, Diagnosis │
│         │         │      │└─────┬───────────────┘
│         │         │      │      │
│         │         │      │      ▼
│         │         │      │┌─────────────────────┐
│         │         │      ││ EXPERT SELECTION     │
│         │         │      ││ Named individual,    │
│         │         │      ││ mental models, KPIs  │
│         │         │      │└─────┬───────────────┘
│         │         │      │      │
│         │         │      │      ▼
│         │         │      │┌─────────────────────┐
│         │         │      ││ PROMPT REWRITE       │
│         │         │      ││ Expert's frameworks, │
│         │         │      ││ vocabulary, blind    │
│         │         │      ││ spots                │
│         │         │      │└─────┬───────────────┘
│         │         │      │      │
│         │         │      │      ▼
│         │         │      │┌─────────────────────┐
│         │         │      ││ PLAIN-ENGLISH ANSWER │
│         │         │      ││ 3-5 points, concrete │
│         │         │      ││ "Next step" at end   │
│         │         │      │└─────────────────────┘
└─────────┴─────────┴──────┴──────┘
```

**Follow-ups skip the pipeline.** Asking "tell me more about point 3" goes deeper in the same expert's framework without re-running everything.

**Ambiguous prompts get clarification first** — via multiple-choice questions so you can reply with a letter instead of typing paragraphs.

## Architecture

The skill uses progressive disclosure to keep context lean. Only the triage logic loads on every invocation — detailed instructions load on demand:

```
best-minds-optimizer/
├── SKILL.md              ← Entry point: triage + routing (64 lines)
└── references/
    ├── optimize.md       ← Full pipeline: Logic Mapping → Expert
    │                       Selection → Rewrite → Output (225 lines)
    ├── clarify.md        ← Clarification flow, then routes to
    │                       optimize.md (44 lines)
    └── polish.md         ← Quick wording pass for user-authored
                            text (35 lines)
```

A **Skip** loads 64 lines. A **Polish** loads 99. Only a full **Optimize** loads the heavy reference.

## What Makes This Different

**Logic Mapping** — Before picking an expert, the skill classifies your problem's structural shape. A pricing question might look like a "Resource" problem (how to allocate a scarce thing) but actually be a "Direction" problem (which path to take). The classification determines which reasoning framework applies:

| Problem Shape | Framework Applied |
|---------------|-------------------|
| Bottleneck | Theory of Constraints |
| Resource | Opportunity Cost / Capital Allocation |
| Direction | First Principles / Inversion |
| Execution | Process Design / Decomposition |
| Tradeoff | Decision Matrix / Weighted Criteria |
| Diagnosis | Root Cause Analysis / 5 Whys |

**Bilingual Execution** — The optimized prompt uses high-density expert terminology to maximize the LLM's reasoning quality. The answer you read uses plain English. The technical language drives better thinking internally; the clear language is what you actually see.

**Expert Panels** — When a question spans multiple domains (e.g., "How should I price and present my SaaS landing page for conversions?"), the skill selects two complementary experts and synthesizes their frameworks into one coherent answer.

## Output Format

```
Expert: Blair Enns — author of Pricing Creativity
Logic Structure: Tradeoff / Competing Goods
Key Metrics: Average deal size, Client retention rate, Revenue per client

Optimized prompt:
> [High-density technical prompt the LLM reasons over]

---

[Plain-English answer you actually read]

Next step: [One concrete action you can take right now]
```

## Works Across Domains

Not just business strategy. The skill finds the right expert for any substantive question:

- **Learning piano as an adult** — Anders Ericsson (deliberate practice)
- **E-commerce conversion drop** — Peep Laja (conversion optimization)
- **Organizing a garage** — David Allen (systems thinking applied to physical space)
- **Sleep optimization** — Matthew Walker (sleep science)

If a domain has someone who wrote the definitive book or research, the skill finds them.

## Install

```bash
npx skills add https://github.com/MoGHenry/best-minds-optimizer --skill best-minds-optimizer
```

Or browse and install from [skills.sh](https://skills.sh).

## License

MIT
