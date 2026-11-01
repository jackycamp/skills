---
name: thermo-nuclear-code-quality-review
description: Run an extremely strict maintainability review for abstraction quality, giant files, and spaghetti-condition growth. Use for a thermo-nuclear code quality review, thermonuclear review, deep code quality audit, or especially harsh maintainability review.
disable-model-invocation: true
---

# Thermo-Nuclear Code Quality Review

Use this skill for an unusually strict review focused on implementation quality, maintainability, abstraction quality, and codebase health.

Above all, this skill should push the reviewer to be **ambitious** about code structure. Do not merely identify local cleanup opportunities. Actively search for "code judo" moves: restructurings that preserve behavior while making the implementation dramatically simpler, smaller, more direct, and more elegant.

## Core Prompt

Start from this baseline:

> Perform a deep code quality audit of the current branch's changes.
> Rethink how to structure / implement the changes to meaningfully improve code quality without impacting behavior.
> Work to improve abstractions, modularity, reduce Spaghetti code, improve succinctness and legibility.
> Be ambitious, if there is a clear path to improving the implementation that involves restructuring some of the codebase, go for it.
> Be extremely thorough and rigorous. Measure twice, cut once.

## Non-Negotiable Additional Standards

Apply the baseline prompt above, plus these explicit review rules:

0. **Be ambitious about structural simplification.**

   - Do not stop at "this could be a bit cleaner."
   - Look for opportunities to reframe the change so that whole branches, helpers, modes, conditionals, or layers disappear entirely.
   - Prefer the solution that makes the code feel inevitable in hindsight.
   - Assume there is often a "code judo" move available: a re-organization that uses the existing architecture more effectively and makes the change dramatically simpler and more elegant.
   - If you see a path to delete complexity rather than rearrange it, push hard for that path.

1. **Do not let a PR push a file from under 1k lines to over 1k lines without a very strong reason.**

   - Treat this as a strong code-quality smell by default.
   - Prefer extracting helpers, subcomponents, modules, or local abstractions instead of letting a file sprawl past 1000 lines.
   - If the diff crosses that threshold, explicitly ask whether the code should be decomposed first.
   - Only waive this if there is a compelling structural reason and the resulting file is still clearly organized.

2. **Do not allow random spaghetti growth in existing code.**

   - Be highly suspicious of new ad-hoc conditionals, scattered special cases, or one-off branches inserted into unrelated flows.
   - If a change adds "weird if statements in random places", treat that as a design problem, not a stylistic nit.
   - Prefer pushing the logic into a dedicated abstraction, helper, state machine, policy object, or separate module instead of tangling an existing path.
   - Call out changes that make the surrounding code harder to reason about, even if they technically work.

3. **Bias toward cleaning the design, not just accepting working code.**

   - If behavior can stay the same while the structure becomes meaningfully cleaner, push for the cleaner version.
   - Do not rubber-stamp "it works" implementations that leave the codebase messier.
   - Strongly prefer simplifications that remove moving pieces altogether over refactors that merely spread the same complexity around.

4. **Prefer direct, boring, maintainable code over hacky or magical code.**

   - Treat brittle, ad-hoc, or "magic" behavior as a code-quality problem.
   - Be skeptical of generic mechanisms that hide simple data-shape assumptions.
   - Flag thin abstractions, identity wrappers, or pass-through helpers that add indirection without buying clarity.

5. **Push hard on type and boundary cleanliness when they affect maintainability.**

   - Question unnecessary optionality, `unknown`, `any`, or cast-heavy code when a clearer type boundary could exist.
   - Prefer explicit typed models or shared contracts over loosely-shaped ad-hoc objects.
   - If a branch relies on silent fallback to paper over an unclear invariant, ask whether the boundary should be made explicit instead.

6. **Keep logic in the canonical layer and reuse existing helpers.**

   - Call out feature logic leaking into shared paths or implementation details leaking through APIs.
   - Prefer existing canonical utilities/helpers over bespoke one-offs.
   - Push code toward the right package, service, or module instead of normalizing architectural drift.

7. **Treat unnecessary sequential orchestration and non-atomic updates as design smells when the cleaner structure is obvious.**
   - If independent work is serialized for no good reason, ask whether the flow should run in parallel instead.
   - If related updates can leave state half-applied, push for a more atomic structure.
   - Do not over-index on micro-optimizations, but do flag avoidable orchestration complexity that makes the implementation more brittle.

## Inspect Interfaces, State Ownership, and Test Placement

Follow changed interfaces through their real callers and consumers. Do not assess a signature only at its definition.

- **Inspect parameter growth.** Compare changed signatures and call sites. Look for repeated argument groups, adjacent boolean arguments, and values passed through layers that barely use them. Identify the resulting ambiguity or coupling before demanding a change. A short list of distinct, typed arguments can remain clearer than a context object.
- **Check existing structures first.** Consider whether an existing options or request type already owns the new inputs. A new context or renderer should express a cohesive responsibility, reduce repeated plumbing, or share useful work. Reject argument bags that merely hide the same complexity, and avoid passing a whole application or broad context into a narrow helper.
- **Trace state ownership and updates.** Identify the authoritative owner of each new state value. For stored copies and derived snapshots, inspect why they exist, when they refresh, and what prevents inconsistency. For parallel collections, inspect the identity or ordering invariant that keeps entries aligned. Do not demand that mutable UI state move into immutable source data merely because both describe the same entity. Recommend an ownership change only when its concrete benefit justifies the cost.
- **Assess test placement.** Consider module size, repository conventions, shared fixtures, and the effort needed to navigate between tests and implementation. Keep a small test module inline when a separate file adds no concrete benefit. Separate tests when their size or responsibility makes that clearer. Neither arrangement is an automatic finding.

Apply these checks as design judgments, not mechanical rules. Argument counts, boolean parameters, derived state, and separate test files are evidence to inspect, not proof of a defect. Passing tests establish behavior only within their coverage; they do not establish that an interface or ownership boundary is maintainable.

## Primary Review Questions

For every meaningful change, ask:

- Is there a "code judo" move that would make this dramatically simpler?
- Can this change be reframed so fewer concepts, branches, or helper layers are needed?
- Does this improve or worsen the local architecture?
- Did the diff add branching complexity where a better abstraction should exist?
- Did a previously cohesive module become more coupled, more stateful, or harder to scan?
- Is this logic living in the right file and layer?
- Did this change enlarge a file or component past a healthy size boundary?
- Are there repeated conditionals that signal a missing model or missing helper?
- Is the implementation direct and legible, or does it rely on special cases and incidental control flow?
- Is this abstraction actually earning its keep, or is it just a wrapper?
- Did the diff introduce casts, optionality, or ad-hoc object shapes that obscure the real invariant?
- Is this logic living in the canonical layer, or did the diff leak details across a boundary?
- Is this orchestration more sequential or less atomic than it needs to be?

## What to Flag Aggressively

Escalate findings when you see:

- A complicated implementation where a cleaner reframing could delete whole categories of complexity.
- Refactors that move code around but fail to reduce the number of concepts a reader must hold in their head.
- A file crossing 1000 lines due to the PR, especially if the new code could be split out.
- New conditionals bolted onto unrelated code paths.
- One-off booleans, nullable modes, or flags that complicate existing control flow.
- Feature-specific logic leaking into general-purpose modules.
- Generic "magic" handling that hides simple structure and makes the code harder to reason about.
- Thin wrappers or identity abstractions that add indirection without simplifying anything.
- Unnecessary casts, `any`, `unknown`, or optional params that muddy the real contract.
- Copy-pasted logic instead of extracted helpers.
- Narrow edge-case handling implemented in the middle of an already busy function.
- Refactors that technically pass tests but make the code less modular or less readable.
- "Temporary" branching that is likely to become permanent debt.
- Bespoke helpers where the codebase already has a canonical utility for the job.
- Logic added in the wrong layer/package when it should live somewhere more central.
- Sequential async flow where obviously independent work could stay simpler and clearer with parallel execution.
- Partial-update logic that leaves state less atomic than necessary.

## Preferred Remedies

When you identify a code-quality problem, prefer suggestions like:

- Delete a whole layer of indirection rather than polishing it.
- Reframe the state model so conditionals disappear instead of getting centralized.
- Change the ownership boundary so the feature becomes a natural extension of an existing abstraction.
- Turn special-case logic into a simpler default flow with fewer exceptions.
- Extract a helper or pure function.
- Split a large file into smaller focused modules.
- Move feature-specific logic behind a dedicated abstraction.
- Replace condition chains with a typed model or explicit dispatcher.
- Separate orchestration from business logic.
- Collapse duplicate branches into a single clearer flow.
- Delete wrappers that do not meaningfully clarify the API.
- Reuse the existing canonical helper instead of introducing a near-duplicate.
- Make type boundaries more explicit so the control flow gets simpler.
- Move the logic to the package/module/layer that already owns the concept.
- Parallelize independent work when that also simplifies the orchestration.
- Restructure related updates into a more atomic flow when partial state would be harder to reason about.

Do not be satisfied with "maybe rename this" feedback when the real issue is structural.
Do not be satisfied with a merely cleaner version of the same messy idea if there is a plausible path to a much simpler idea.

## Review Tone

Be direct, serious, and demanding about quality.
Do not be rude, but do not soften major maintainability issues into mild suggestions.
If the code is making the codebase messier, say so clearly.
If the implementation missed an opportunity for a dramatic simplification, say that clearly too.

Good phrases:

- `this pushes the file past 1k lines. can we decompose this first?`
- `this adds another special-case branch into an already busy flow. can we move this behind its own abstraction?`
- `this works, but it makes the surrounding code more spaghetti. let's keep the behavior and restructure the implementation.`
- `this feels like feature logic leaking into a shared path. can we isolate it?`
- `this abstraction seems unnecessary. can we just keep the direct flow?`
- `why does this need a cast / optional here? can we make the boundary more explicit instead?`
- `this looks like a bespoke helper for something we already have elsewhere. can we reuse the canonical one?`
- `i think there's a code-judo move here that makes this much simpler. can we reframe this so these branches disappear?`
- `this refactor moves complexity around, but doesn't really delete it. is there a way to make the model itself simpler?`

## Output Expectations

Within each severity tier, prioritize findings in this order:

1. Structural code-quality regressions
2. Missed opportunities for dramatic simplification / code-judo restructuring
3. Spaghetti / branching complexity increases
4. Boundary / abstraction / type-contract problems that make the code harder to reason about
5. File-size and decomposition concerns
6. Modularity and abstraction issues
7. Legibility and maintainability concerns

Do not flood the review with low-value nits if there are larger structural issues.
Prefer a smaller number of high-conviction comments over a long list of cosmetic notes.

When interfaces, state ownership, or test organization materially change, include a brief structural assessment with file or symbol references. Explain why the resulting boundaries are acceptable, or connect each finding to a concrete consequence and a proportionate improvement. An empty findings list is valid, but it does not replace this assessment when these areas changed. Keep this evidence concise; do not add a boilerplate checklist for untouched areas.

## Report Format

Follow the report structure used by [rust-review](https://github.com/morganlinton/BeautifulRust/blob/main/skills/rust-review/SKILL.md). Use `Rust review` for Rust code and `Code quality review` for other code. Use the language of the reviewed code in fix examples. This format does not require loading another skill or applying Rust-specific rules to other languages.

~~~markdown
# Rust review: <what was reviewed>

<One paragraph: what the code does, library or application, overall verdict, and the
single most important thing to fix. Say whether clippy and tests were run.>

## Must fix

### 1. <Short title> (`src/file.rs:42`)

**Rule:** <rule name and source>

**Problem:** <what is wrong and the concrete consequence, one or two sentences>

**Fix:**

```rust
<the smallest code change that fixes it>
```

## Should fix

### 2. <Short title> (`src/file.rs:84`)

<Use the same Rule, Problem, and Fix fields.>

## Consider

### 3. <Short title> (`src/file.rs:126`)

<Use the same Rule, Problem, and Fix fields.>

## What's already good

- <2 to 4 specific things the code does well, with file or symbol references>
~~~

- **Must fix:** defects that block acceptance, including correctness or safety failures and demonstrated structural problems under the Approval Bar. A size threshold or a preferred abstraction alone is insufficient; explain the blocking consequence.
- **Should fix:** actionable improvements with a concrete maintenance or usability cost that does not block acceptance. Explain that cost.
- **Consider:** optional style, naming, and polish, or a trade-off without enough evidence to demand a change.

Use the same finding fields in every tier. Number findings consecutively across tiers so they have stable IDs during review and rebuttal. Keep empty tiers and write `None.`; do not invent findings to fill them. If there are no findings, state that directly in the opening paragraph instead of inventing a most important fix.

For each finding, cite `file:line` or a clearly bounded scope when no single line applies. Cite an actual rule source, such as a named section of this skill, a repository rule, a documented invariant, a language guideline, or a lint. Do not invent a rule citation or present a preference as an external requirement.

Show a minimal fix using the relevant code, configuration, or documentation syntax. For a cross-cutting change that cannot be expressed honestly in a small snippet, give precise steps and affected symbols rather than a speculative rewrite. Group repeated instances into one finding and list their locations. Explain documented trade-offs before assigning severity; deliberate choices can still cause blocking defects.

Keep the opening paragraph concise. For non-Rust code, report the relevant lint or static-analysis tools instead of clippy. State which checks ran, their results, and any checks that did not run; summarize warnings instead of pasting raw tool output. Include the structural assessment required above in the opening paragraph, findings, or referenced strengths, as appropriate.

Always include `What's already good`. Prefer two to four concrete strengths and explain what to preserve. If fewer strengths are supported by the code, report only those; do not add generic praise to meet a quota.

## Approval Bar

Do not approve merely because behavior seems correct.
The bar for approval is:

- no clear structural regression
- no obvious missed opportunity to make the implementation dramatically simpler when such a path is visible
- no unjustified file-size explosion
- no obvious spaghetti-growth from special-case branching
- no obviously hacky or magical abstraction that makes the code harder to reason about
- no unnecessary wrapper/cast/optionality churn obscuring the real design
- no clear architecture-boundary leak or avoidable canonical-helper duplication
- no missed opportunity for an obvious decomposition that would materially improve maintainability

Treat these as presumptive blockers unless the author can justify them clearly:

- the PR preserves a lot of incidental complexity when there is a plausible code-judo move that would delete it
- the PR pushes a file from below 1000 lines to above 1000 lines
- the PR adds ad-hoc branching that makes an existing flow more tangled
- the PR solves a local problem by scattering feature checks across shared code
- the PR adds an unnecessary abstraction, wrapper, or cast-heavy contract that makes the design more indirect
- the PR duplicates an existing helper or puts logic in the wrong layer when there is a clear canonical home

If those conditions are not met, leave explicit, actionable feedback and push for a cleaner decomposition.
