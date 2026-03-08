---
name: best-minds-optimizer
description: Prompt optimizer that rewrites the user's input through the lens of the world's top domain expert before executing. Activates when the user asks a substantive question, wants strategic advice, needs a deeper take, requests expert-level analysis, or is working through a complex decision. Triggers on phrases like "best minds", "optimize this prompt", "what would [expert] say", "give me a world-class take", or any non-trivial question where expert framing would produce a sharper result. Does NOT trigger on mechanical tasks like file edits, git commands, or simple code operations. When in doubt about whether to trigger, trigger — the skill will self-skip if the input is trivial.
---

# Best Minds — Prompt Optimizer

A pre-processing layer that optimizes prompts before execution. For any substantive question or task, it identifies the world's best domain expert and rewrites the user's prompt through that expert's frameworks — producing sharper, more precise prompts that get better results from the LLM.

The core insight: LLMs are simulators. A prompt framed through Charlie Munger's mental models produces a fundamentally different response than a vague question. This skill applies that insight automatically.

## Pipeline

### Step 1: Clarify Before Optimizing

Before selecting an expert or rewriting, check for critical ambiguities in the user's prompt. If the prompt could go in meaningfully different directions depending on unstated context, **ask 2–3 targeted clarifying questions first** instead of assuming.

Ask when:
- The user's situation, constraints, or goals are unclear (e.g., "Should I build an MVP?" — as what? for whom? with what resources?)
- The answer would change significantly based on context the user hasn't provided
- Assumptions you'd have to make would risk wasting the user's time

Do NOT ask when:
- The prompt is clear enough that expert framing will sharpen it regardless
- Clarification would just delay an obviously useful rewrite
- The user explicitly says "just give me your take"

Format: Use **multiple-choice questions** (2–5 options each) to make it fast for the user to respond. Always include a final option like "Other — tell me more" so users can type their own context if none of the choices fit. Keep to 2–3 questions max. No preamble — just the choices. Get the context, then proceed to Step 2.

### Step 2: Identify the Best Mind

Determine which real-world expert's frameworks would produce the sharpest version of this prompt.

- Name a specific individual, never a generic role ("Peter Thiel" not "a startup expert")
- Choose based on the **problem structure**, not the topic surface — a pricing question might need Munger's "psychology of misjudgment" more than a generic economist
- For cross-domain questions, pick a primary expert and blend in a second expert's framework where it sharpens the prompt

### Step 3: Rewrite the Prompt

Transform the user's raw input into an optimized prompt by applying the expert's:

- **Frameworks**: Their actual mental models and analytical tools
- **Vocabulary**: Domain-precise terminology that unlocks better reasoning
- **Reasoning structure**: How they decompose problems — first principles? Inversion? Systems thinking?
- **Blind spot coverage**: What the expert would insist on considering that the user missed

The optimized prompt should be a self-contained question or instruction — something that would produce an excellent answer even without the skill.

### Step 4: Structured Output and Execution

Emit a JSON metadata block in a fenced code block, then render the display version, then execute.

**JSON Schema:**

```json
{
  "status": "optimized" | "skipped" | "error",
  "expert_profile": {
    "name": "string",
    "rationale": "string — why this expert for this problem",
    "frameworks": ["string — specific mental models or tools applied"]
  },
  "optimized_prompt": "string — full rewritten prompt text",
  "ui_render": "string — Markdown formatted for terminal display",
  "control_flag": {
    "immediate_execute": true | false,
    "user_confirm_required": true | false
  }
}
```

**Behavior by status:**

- **`optimized`** — Emit the JSON block, render `ui_render` to the user as Markdown, then execute `optimized_prompt` and deliver the answer below.
- **`skipped`** — Emit minimal JSON with `status: "skipped"` (or skip JSON entirely for trivial tasks). Proceed with the original prompt unchanged.
- **`error`** — Emit JSON with explanation in `rationale`. Fall back to executing the original prompt.

**Direction-shift pause:** If the rewrite significantly shifts direction from what the user asked, set `user_confirm_required: true` and pause: *"I reframed this through X's lens, which shifts focus to Y. Proceed or adjust?"*

**Markdown fallback:** If JSON generation fails for any reason, fall back to the original Markdown format — show the expert, the optimized prompt, and execute. Never let the JSON protocol block execution.

- If the user says they prefer a different expert, switch and re-optimize

**Conciseness rules — keep output scannable:**
- The executed answer should be **short and structured**: use bullet points, numbered lists, or bold headers — not walls of prose
- Lead with the most important insight or recommendation first
- Cap the answer at **3–5 key points** maximum. Go deeper only if the user asks follow-up questions
- Avoid restating what the user already knows — add new value only

**Always end with a clear next step:**
- End every optimized answer with a concrete **"Next step"** or **"What to do now"** line
- This should be a single actionable sentence the user can act on immediately
- Examples: *"Next step: List your top 3 candidate markets and I'll evaluate each."* or *"What to do now: Run `npx lighthouse` on your homepage and share the report."*

### When to Skip

Do NOT optimize:
- Simple commands: "read this file", "commit this", "list files in src/"
- Unambiguous mechanical tasks: "rename variable X to Y", "add a border to this div"
- Follow-up messages in an ongoing conversation where the prompt is already refined

Only optimize substantive questions, strategic decisions, analysis requests, and complex tasks where expert framing adds real value.

## Examples

**Example 1 — Ambiguous prompt (clarify first):**

User: *"Should I build my startup's MVP as a mobile app or web app first?"*

Response (before optimizing):

> **What's the core user action?**
> a) Marketplace / transactions
> b) Content creation or consumption
> c) Real-time communication
> d) Other — tell me more
>
> **Who's your target user?**
> a) B2B — mostly on desktop at work
> b) B2C — mostly on mobile, on the go
> c) Both and b — use case will determine device
> d) not sure yet - deep user research needed
> e) Other — tell me more
>
> **Do you have existing traction?**
> a) Yes — existing audience or paying customers
> b) No — starting from zero
> c) Some early interest but no revenue yet
> d) Other — tell me more

*User picks options (e.g., "a, b, c"), then proceed to Step 2 → 3 → 4 with that context.*

**Example 2 — Clear prompt (optimize and execute):**

User: *"Should I start a SaaS business?"*

```json
{
  "status": "optimized",
  "expert_profile": {
    "name": "Marc Andreessen",
    "rationale": "Invented the framework for evaluating software market opportunities",
    "frameworks": ["Product vs. feature test", "TAM analysis", "Moat identification", "10x improvement benchmark", "Why-now timing"]
  },
  "optimized_prompt": "Evaluate a SaaS business opportunity: (1) Is this a product or a feature? (2) What is the TAM and can it support a venture-scale outcome? (3) Is there a technical insight or distribution advantage that creates a moat? (4) What does 10x better look like vs. incumbents? (5) Why now — what changed that makes this possible today?",
  "ui_render": "**Expert**: Marc Andreessen — ...\n\n**Optimized prompt**:\n> ...\n\n---",
  "control_flag": {
    "immediate_execute": true,
    "user_confirm_required": false
  }
}
```

Executed answer (concise, scannable, ends with next step):

> 1. **Product vs. feature**: If it solves one narrow problem, it's a feature — someone will bundle it. You need a product.
> 2. **TAM**: SaaS needs a market that can support $100M+ ARR or you're building a lifestyle business (which is fine — just know which).
> 3. **Moat**: Data network effects, switching costs, or a technical insight competitors can't easily replicate.
> 4. **10x test**: If your product isn't 10x better on the axis that matters, you'll lose to inertia.
> 5. **Why now**: New API, regulation change, behavior shift — something must have unlocked this.
>
> **Next step:** Write down your specific SaaS idea and I'll evaluate it against these 5 criteria.

**Example 3 — Skipped (trivial task):**

User: *"Read this file"*

```json
{
  "status": "skipped"
}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Optimizing trivial tasks | Skip — don't add friction where there's no value |
| Changing the user's actual intent | The expert's lens sharpens the question, not redirects it |
| Long preamble before the optimized prompt | One-line expert selection, then the prompt, then execute |
| Famous name over best fit | Choose the expert whose frameworks decompose *this specific problem* best |
| Generic rewrite that any expert could have produced | The optimized prompt should be recognizably shaped by this specific expert's thinking |
| Producing JSON without executing afterward | The JSON is structured metadata — you must still execute `optimized_prompt` and deliver the answer below |
| Assuming intent on ambiguous prompts | Ask 2–3 targeted clarifying questions first — don't guess at context that changes the answer |
| Wall-of-text answers | Keep answers to 3–5 key points with bullets/headers. Let the user ask for more depth |
| No actionable next step | Always end with a concrete "Next step" the user can act on immediately |
