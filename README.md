# Best Minds — Prompt Optimizer

An agent skill that automatically rewrites your prompts through the lens of the world's top domain expert before executing them. Instead of getting generic AI answers, you get responses shaped by the frameworks, mental models, and reasoning patterns of the best mind for your specific problem.

## Why Install This?

The same question produces dramatically different answers depending on how it's framed. This skill exploits that by identifying the world's leading expert for your topic and rewriting your prompt through their thinking — automatically, before the LLM responds.

**You ask a question. It clarifies if needed (via quick multiple-choice), picks the best expert, rewrites your prompt, and delivers a sharp, concise answer with a clear next step.**

## Test Results

We ran 3 test cases — with and without the skill — to measure the difference.

---

### Test 1: Business Strategy

**Prompt**: *"I'm thinking about raising prices for my freelance design work but I'm scared of losing clients"*

**Without skill** — Generic advice: "raise incrementally", "grandfather existing clients", "be confident." Reads like a blog post. No attributed frameworks, no named experts, no reframing of the problem.

**With skill** — Identified **Blair Enns** (author of *Pricing Creativity*). Rewrote the prompt to ask 5 precise questions through Enns' frameworks:

> *Am I confusing price sensitivity with a positioning problem? What does my current pricing signal? How is "firing" price-sensitive clients a feature, not a bug? What's the right transition strategy? How do I reframe from "cost of design" to "cost of not solving the business problem"?*

The answer reframed the entire question: **this isn't a pricing problem, it's a positioning problem.** That insight — which comes directly from Enns' work — changes the entire approach.

| Assertion | With Skill | Without Skill |
|-----------|:---------:|:------------:|
| Names a specific expert | Yes (Blair Enns) | No |
| Emits structured JSON with expert profile | Yes | No |
| Shows optimized prompt (via `ui_render`) | Yes (5-part rewrite) | No |
| Executes the optimized prompt | Yes | Yes |
| Expert frameworks visible in answer | Yes ("practitioner's dilemma", value-based pricing) | No |

---

### Test 2: Technical Problem

**Prompt**: *"My Next.js app is slow on mobile, what should I do?"*

**Without skill** — A solid checklist: bundle analysis, image optimization, rendering strategies. Correct but generic — the same advice you'd find in any Next.js performance guide.

**With skill** — Identified **Addy Osmani** (Google Chrome engineering lead, created Lighthouse). Rewrote the prompt into a 7-part diagnostic framework:

> *What do your Core Web Vitals look like? What's your JS bundle size? Are you using the right rendering strategy per route? What's your critical rendering path? Are you using next/image with proper sizing? What does your network waterfall look like on throttled 3G? Are you code-splitting and lazy-loading?*

The answer followed the same diagnostic methodology Osmani uses — measure first, prioritize by impact, and ended with a clear "if you can only do 3 things" prioritization.

| Assertion | With Skill | Without Skill |
|-----------|:---------:|:------------:|
| Names a specific expert | Yes (Addy Osmani) | No |
| Emits structured JSON with expert profile | Yes | No |
| Shows optimized prompt (via `ui_render`) | Yes (7-part diagnostic) | No |
| Executes the optimized prompt | Yes | Yes |
| Expert frameworks visible in answer | Yes (Core Web Vitals methodology, performance budgets) | Partial |

---

### Test 3: Trivial Task (Should Skip)

**Prompt**: *"Read the package.json file in this directory"*

**With skill** — Correctly skipped optimization. No expert, no rewrite. Just read the file.

**Without skill** — Same behavior.

The skill knows when to stay out of the way. Simple commands, file operations, and unambiguous mechanical tasks are executed directly without unnecessary ceremony.

---

## Overall Scores

| Test Case | With Skill | Without Skill | Improvement |
|-----------|:---------:|:------------:|:----------:|
| Business Strategy | 5/5 (100%) | 1/5 (20%) | +80% |
| Technical Problem | 5/5 (100%) | 1.5/5 (30%) | +70% |
| Trivial Task | 3/3 (100%) | 3/3 (100%) | — |
| **Total** | **13/13** | **5.5/13** | **+58%** |

## How It Works

1. **You send a prompt** — any substantive question, decision, or analysis request
2. **If ambiguous, it asks clarifying questions first** — as multiple-choice options (2–5 choices each), always with an "Other — tell me more" escape hatch. Fast to answer, no typing required.
3. **The skill identifies the best expert** — a specific named individual whose frameworks fit your problem
4. **It rewrites your prompt** through that expert's mental models, vocabulary, and reasoning structure
5. **It emits structured JSON** with the expert profile, optimized prompt, and display metadata
6. **It executes the optimized prompt** — concise answer (3–5 key points), always ending with a concrete next step

For trivial tasks (file reads, git commands, simple code edits), the skill emits `status: "skipped"` and proceeds directly.

## Clarification UX

When a prompt is ambiguous, the skill asks multiple-choice questions instead of open-ended ones — keeping the interaction fast:

```
**What's the core user action?**
a) Marketplace / transactions
b) Content creation or consumption
c) Real-time communication
d) Other — tell me more

**Who's your target user?**
a) B2B — mostly on desktop at work
b) B2C — mostly on mobile, on the go
c) Both / not sure yet
d) Other — tell me more

**Do you have existing traction?**
a) Yes — existing audience or paying customers
b) No — starting from zero
c) Some early interest but no revenue yet
d) Other — tell me more
```

You reply with letters (e.g., `a, b, c`) and the skill optimizes from there. If none of the choices fit, type your own context.

## Output Format

The skill outputs a structured JSON block before executing, enabling downstream automation to parse results reliably:

```json
{
  "status": "optimized",
  "expert_profile": {
    "name": "Blair Enns",
    "rationale": "Author of Pricing Creativity — reframes pricing as a positioning problem",
    "frameworks": ["Practitioner's dilemma", "Value-based pricing", "Price-sensitive client segmentation"]
  },
  "optimized_prompt": "...",
  "ui_render": "**Expert**: Blair Enns — ...\n\n**Optimized prompt**:\n> ...\n\n---",
  "control_flag": {
    "immediate_execute": true,
    "user_confirm_required": false
  }
}
```

**Status values:**
- `optimized` — Expert selected, prompt rewritten, answer delivered
- `skipped` — Trivial task, proceeded with original prompt
- `error` — Optimization failed, fell back to original prompt

The `ui_render` field contains the human-readable Markdown displayed in the terminal. The `optimized_prompt` field is what gets executed.

## Install

```bash
npx skills add https://github.com/MoGHenry/best-minds-optimizer --skill best-minds-optimizer
```

Or browse and install from [skills.sh](https://skills.sh).

## Project Structure

```
best-minds-optimizer/
├── SKILL.md        # Skill definition (required)
├── README.md       # Documentation
├── LICENSE          # MIT License
└── .gitignore
```
