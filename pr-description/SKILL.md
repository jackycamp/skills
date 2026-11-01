---
name: pr-description
description: "Draft, rewrite, or update pull request descriptions with separate feature design, code design, validation, and skills-used sections. Apply ASD-STE-inspired English. Use when preparing a PR or revising its description; not for reviewing the code itself."
---

# PR description

Write a description that explains the final change to a reviewer who did not see the conversation.

Read and apply the sibling [asd-ste skill](../asd-ste/SKILL.md) to all explanatory prose before drafting. If that path is unavailable, locate `asd-ste` through the skill catalog. If the skill is missing, disclose the limitation and use short, literal sentences without claiming that the skill ran.

## Establish the facts

Inspect the complete PR diff against its actual base. Use the current branch, final commit, requirements, and relevant check results. For an existing PR, read its current description and repository template before editing.

Use existing session evidence when it still matches the final code. Do not rerun tests solely because the description changes. Distinguish checks that ran from checks that are proposed or unavailable.

## Write the description

Start with the concrete problem and resulting behavior. Add a short before-and-after example when it helps. Describe the final implementation; omit abandoned approaches and conversational history unless they explain a material tradeoff.

Use the following sections. Preserve required repository sections and user-selected headings; integrate this content without duplicating it.

### Feature design

Explain observable behavior, interactions, and the conditions that change behavior. Include relevant scope limits and tradeoffs. Keep internal architecture details in the code design section.

For changes without a product interface, describe the behavior of the affected API, command, documentation, or workflow. Do not invent interface behavior to fill this section.

### Code design choices

Explain the important implementation choices and their reasons. Cover relevant state ownership, module boundaries, reused code, and update paths. Include file paths or symbols only when they help the reviewer assess a choice.

Describe tests in terms of the behavior they protect. Do not treat test counts or added lines as proof of quality. State material limitations when the evidence supports them.

### Validation

List the checks that actually ran and their results. Preserve exact commands and known test counts. Identify the commit covered when available. Distinguish the complete suite from tests added by the PR.

Report independent review only if a separate reviewer performed it. State its scope and any unresolved findings. Report unavailable checks and relevant risks directly. Never infer results from a successful commit or push.

### Skills used

List every skill actually used to produce, test, document, or review the change, including skills used by delegated agents. Give each exact skill name and its concrete purpose. Include this skill and `asd-ste` when applied to the description.

Use first-use order when the session record establishes that order. Otherwise use a clear workflow order without claiming a verified chronology. Do not list skills merely because they are installed or available. Do not claim independent review from a skill name alone.

## Check the draft

Apply `asd-ste` throughout, including the opening and skill explanations. Preserve commands, symbols, numbers, conditions, exceptions, and scope limits. Split long sentences instead of removing necessary meaning.

Scale detail to the change. Use short paragraphs or parallel bullets. Check that each section adds distinct information and that the title matches the final implementation.

## Publish when authorized

A request to draft or rewrite text does not authorize a push, commit, or remote edit. When the user authorizes PR creation or editing, publish the prepared description within that scope.

Prefer a structured body argument. With `gh`, write the exact description to a temporary file and pass `--body-file`. Preserve real newlines and literal code text.

If the remote description changed since inspection, reconcile the current text before publishing. Verify that the saved description matches the intended text. Report the PR link and any incomplete action.
