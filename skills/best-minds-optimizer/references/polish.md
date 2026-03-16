# Polish Pipeline — Quick Wording Pass

When triage selects the **Polish** lane, follow these instructions. Do NOT run the full optimization pipeline (no expert selection, no logic mapping, no bilingual execution).

## When to Polish

The user has provided text they authored — instructions, descriptions, config prose, skill definitions, prompts, or documentation — and it would benefit from a wording pass. Signals:

- The task is mechanical (edit a file, update a config) but the *content* is prose the user composed
- The user explicitly asks to "polish", "clean up", "improve wording", or similar
- The user presents their own draft and asks for feedback or refinement

## Pre-Polish Analysis (4-D Phases 1–2)

Before editing, apply the first two phases of the 4-D methodology (`references/methodology.md`):

1. **Deconstruct** — Extract the user's core intent, key entities, and what context is provided vs. missing.
2. **Diagnose** — Check for clarity gaps, ambiguity, redundancy, and missing specificity.

Use what you find to inform the polish edits below. Skip Phases 3–4 (Develop/Deliver) — those are optimize-only.

## How to Polish

Apply these edits while preserving the user's intent and voice:

1. **Tighten wording** — Remove filler words, unnecessary qualifiers, and redundant phrases
2. **Fix grammar** — Correct errors without changing tone
3. **Remove redundancy** — Merge overlapping sentences or points
4. **Improve parallel structure** — Make lists and sequences consistent in form
5. **Sharpen clarity** — Replace vague language with precise terms
6. **Preserve voice** — The output should still sound like the user, not a rewrite

## Output

Emit `status: "polished"` and present the improved text. If the changes are substantial, briefly note what you changed (1–2 sentences max).

## Example

User: *"For step 2, MUST breakdown the optimized prompt and then review all available Agent Skills. Check if any of the Agent Skills is helpful for completing the task. Emit a list of invoked Agent Skills and one-sentence reason why it's triggered. Then ask permission from User whether to invoke all or a portion of them or rethink with the user clarification"*

Polished output:
> *"After step 1, break down the optimized prompt and review all available Agent Skills. For each skill that matches the task, emit the skill name and a one-sentence reason it was triggered. Then ask the user for permission — they may invoke all, a subset, or none, or provide clarification that changes the selection."*

Changes: tightened phrasing, fixed grammar ("breakdown" → "break down"), removed redundancy, improved parallel structure in the permission options.
