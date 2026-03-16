# 4-D Methodology — Structured Prompt Optimization

A universal four-phase process for analyzing and optimizing any prompt. Each phase builds on the previous, ensuring consistent, high-quality output regardless of topic or domain.

## Phase 1: DECONSTRUCT

Extract the structural elements of the user's prompt before any optimization begins.

- **Core intent** — What is the user actually trying to achieve? Strip away surface phrasing to find the underlying goal.
- **Key entities** — Identify the specific people, systems, concepts, or domains involved.
- **Context provided** — What background, constraints, or requirements did the user supply?
- **Context missing** — What's absent that would change the answer? (Audience, timeline, budget, experience level, prior attempts.)
- **Output requirements** — What format, depth, or deliverable does the user expect?

**Integration:** Runs alongside Step 1.5 (Logic Mapping). Logic Mapping identifies problem topology (Bottleneck, Resource, Direction, etc.); Deconstruct extracts entities, constraints, and gaps. Together they produce a complete structural picture before expert selection.

## Phase 2: DIAGNOSE

Audit the deconstructed prompt for quality issues that would weaken the optimized output.

- **Clarity gaps** — Are there ambiguous terms, vague references, or unclear scope?
- **Specificity check** — Does the prompt contain enough concrete detail to produce a targeted answer, or will the response be generic?
- **Completeness audit** — Are critical dimensions missing? (e.g., a pricing question with no mention of market, audience, or competitive context)
- **Complexity assessment** — Does this need a simple direct answer, a structured framework, or a multi-layered analysis?
- **Assumption detection** — What unstated assumptions is the user making that should be surfaced or challenged?

**Integration:** Runs after Deconstruct and before Step 2 (Expert Selection). The diagnosis informs which expert to select (a clarity-heavy prompt may need a different expert than a well-specified one) and what gaps the optimized prompt must fill.

## Phase 3: DEVELOP

Select and apply optimization techniques based on the request type identified during Deconstruct and Diagnose.

### Technique Selection by Request Type

| Request Type | Primary Techniques | Focus |
|---|---|---|
| **Creative** | Multi-perspective exploration, tone emphasis, audience framing | Divergent thinking, voice, emotional resonance |
| **Technical** | Constraint-based specification, precision vocabulary, edge case coverage | Accuracy, completeness, reproducibility |
| **Educational** | Few-shot examples, progressive structure, analogy bridging | Clarity, scaffolded understanding, retention |
| **Complex** | Chain-of-thought decomposition, systematic frameworks, dependency mapping | Logical rigor, step sequencing, completeness |

### Development Steps

1. **Classify request type** — Creative, Technical, Educational, or Complex (may blend two).
2. **Apply primary techniques** — Use the technique set from the table above.
3. **Assign AI role/expertise** — Feed the expert selection from Step 2 into the prompt framing. The expert's specific frameworks, vocabulary, and reasoning style shape the rewrite.
4. **Enhance context** — Fill gaps identified in Diagnose by embedding necessary context, constraints, or scope directly into the optimized prompt.
5. **Implement logical structure** — Order the prompt's sub-questions or instructions in the sequence that produces the best reasoning flow.

**Integration:** This is the core of Step 3 (Prompt Rewrite) in the optimize pipeline. The expert selected in Step 2 provides the "who"; Develop provides the "how" — the structural technique that shapes the rewrite.

## Phase 4: DELIVER

Construct and format the final optimized prompt.

- **Assemble the prompt** — Combine the expert framing, technique structure, enhanced context, and logical flow into a self-contained optimized prompt.
- **Format by complexity** — Simple requests get a focused 3–5 point prompt. Complex requests get a structured multi-part prompt with clear dependencies between sub-questions.
- **Verify completeness** — Check that every gap identified in Diagnose is addressed, every constraint from Deconstruct is included, and the prompt would produce an excellent answer even without the skill.

**Integration:** Maps to Step 4 (Output) in the optimize pipeline. The formatted prompt is presented in the Review Gate for user approval before execution.

---

## Polish Lane Application

When triage routes to **Polish** (not Optimize), apply only Phases 1–2:

- **Deconstruct** — Extract what the user is saying, identify entities and intent.
- **Diagnose** — Check for clarity gaps, ambiguity, redundancy, and missing specificity.

Then apply the standard polish edits (tighten wording, fix grammar, sharpen clarity) informed by what Deconstruct and Diagnose revealed. Skip Phases 3–4 — technique selection and full prompt construction are optimize-only.
