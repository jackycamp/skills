---
name: dev-loop
description: Coordinate separate planning, implementation, and review agents for an autonomous development loop, including fixes and reasoned pushback on review findings. Use when the user wants an implementation iterated before their final review.
---

# Dev loop

Act as the coordinator. Delegate planning, implementation and testing, and independent review to separate agents. Own the handoffs and repeat the fix/review loop until the work is ready for the user's review or a genuine blocker or budget limit prevents completion.

Proceed through authorized stages without asking the user to approve plans, fixes, or agent disagreements. Make reasonable, reversible decisions and record material assumptions. Ask only when a consequential missing requirement cannot be inferred, a necessary action needs permission, or progress requires information only the user can provide. Keep progress updates brief; deliver the reviewable result at the end.

## Set up the task

- Capture the requirements, acceptance criteria, scope, repository guidance, starting revision, and pre-existing changes. Preserve the user's work.
- Resolve user-selected review skills from the available skill catalog and pass their exact locations to the reviewer. If none are selected, use `thermo-nuclear-code-quality-review` when available, alongside a correctness and test review. If an explicitly required skill cannot be found, report that blocker rather than claim it ran.
- Keep a compact task record in task scratch space: requirements, plan, agent identities, review round, findings and dispositions, check results, and the version of the code they cover. Update it at handoffs so a resumed coordinator can continue.
- Honor a user-provided budget. Otherwise allow at most three full review rounds, including the initial review. Do not reset this limit by spawning another agent. Reaching the limit with unresolved blockers means incomplete work, not approval.

Use the available agent tools to actually spawn and assign these roles. Merely reading another skill or changing personas is not a substitute for separate agents. If delegation is unavailable, disclose the limitation; do not describe a single-agent run as independent review.

Only the implementer edits product code and tests. Keep planning and review read-only by instruction. Sequence dependent stages; avoid reviewing a moving working tree.

## Git workflow

Apply these rules to the coordinator and every delegated agent. Pass the rules and the `asd-ste` skill location with each assignment.

- Never push commits, branches, or tags. Do not publish repository changes through another tool.
- Use the current checkout. Create a worktree only when the user requests one.
- For a new task, create a task branch from local `main` or `master`. If both exist, use the repository default when that default is `main` or `master`. Otherwise prefer `main`. Record the base commit. If neither branch exists, report a blocker.
- When resuming a task, reuse its recorded branch and base commit.
- Preserve pre-existing changes. If a branch switch would overwrite or mix unrelated changes, report the conflict. Do not stash, discard, or commit the user's changes.
- The implementer must commit each coherent implementation step after the relevant checks. The implementer must also commit each change made in response to review. Commit those changes before the next review. Keep review changes in new commits. Do not amend, squash, or rebase commits unless the user requests it.
- Stage only files or hunks that belong to the task.
- Resolve `asd-ste` from the available skill catalog. The implementer must read that skill before the first commit. Apply it to every commit message. Each complete commit message must contain one or two sentences. This limit includes the subject and body. State the concrete change. Add the reason only when useful. For example: `Preserve the collapsed state of other files.` If the skill is unavailable, locate it before proceeding. Report a blocker if the skill cannot be found.
- Leave the task branch for the user's review. Do not merge the branch unless the user requests it.

## 1. Plan

Spawn a planner to inspect the relevant code and turn the requirements into a practical implementation plan and validation checklist. Request the important integration points, behavior choices, and risks, with file references. Keep the plan proportional to the task.

Evaluate the plan yourself and resolve routine choices without a user checkpoint. Consult the planner again only if implementation evidence calls for a material design change.

## 2. Implement and test

Spawn an implementer with the requirements, accepted plan, relevant repository context, and validation checklist. Ask them to implement the behavior and run appropriate checks. They may improve the plan when the code supports a better approach; require a brief explanation of material deviations.

Request a concise handoff: changes made, commits created, acceptance criteria covered, actual check results, and remaining concerns. Record the commit that each check result covers. A claim that checks passed must identify what ran. Keep this implementer for subsequent fixes where possible.

## 3. Review independently

Spawn a separate reviewer with the requirements, task diff and comparison base, relevant code, and test evidence. Give them the selected review skill paths and explicitly instruct them to read and apply those skills. Do not prime the initial review with the implementer's defense of their design.

Review the full diff from the recorded base commit to the current `HEAD`. Include all implementation and review commits. Verify that no task changes remain uncommitted before review starts.

Require correctness, regressions, and meaningful test coverage to be assessed even when the selected skill concentrates on maintainability. Review the complete task diff and distinguish pre-existing issues from problems introduced or materially worsened by this work.

Ask for high-confidence, actionable findings. Each finding needs a stable ID, location, concrete consequence or structural cost, and the improvement needed. Distinguish blocking findings from optional suggestions. An empty findings list is a valid result; do not require criticism to justify the review.

Apply strict review skills seriously, while treating their presumptive blockers as rebuttable in this workflow. A size threshold, preferred abstraction, or ambitious refactor is evidence to examine, not automatic proof that the implementation must change. User requirements and authorized scope govern the outcome.

## 4. Fix or push back

Send the findings to the implementer. Give them explicit freedom to choose one response per finding:

- **Fix:** accept the finding, change the implementation, and run relevant checks.
- **Alternative:** address the underlying problem with a simpler or better-fitting change and explain why it works.
- **Contest:** explain why the criticism is mistaken, unnecessary, disproportionate, or outside scope. Cite the relevant code, invariant, requirement, reproduction, or test evidence, and explain the cost and benefit of the proposed change.

Do not reward compliance for its own sake or require a refactor merely to satisfy the reviewer. Equally, "tests pass," "it works," personal preference, and time already spent are insufficient on their own to rebut a demonstrated bug or a concrete maintainability regression. Existing tests may miss the problem, and passing tests do not settle a structural argument.

Have the reviewer inspect fixes and alternatives and respond to contested findings. They should withdraw or downgrade a finding when the rebuttal is sound, or substantiate it with a specific failure, invariant violation, or structural cost. Require a meaningful benefit and proportionate scope for demanded refactors. Do not ask either agent to defend its earlier position at all costs.

Resolve disagreements as coordinator:

- Prefer the smallest sufficient change that meets the requirements and preserves a maintainable design. Accept a larger restructuring when its concrete benefit justifies its cost and scope.
- Allow one focused rebuttal and response per disputed finding. If the agents still disagree, inspect the evidence and decide; request a narrowly targeted check if it would resolve a material uncertainty. Do not bounce the same arguments indefinitely.
- You may overrule an unsupported or disproportionate review demand with a specific rationale, even if the reviewer still prefers it. You may not waive a user requirement, ignore a demonstrated unresolved defect, or substitute an opinion for required verification. Material uncertainty about a blocking issue remains unresolved until investigated.
- Record each finding as fixed, addressed by an alternative, withdrawn, overruled with reasons, or unresolved. Deferring a real blocking issue does not make the task complete.

Keep resolved findings closed unless new evidence or subsequent edits invalidate the resolution. Do not let an optional suggestion become an unrelated redesign. If new evidence requires replanning, return to the planner without silently expanding the user's assignment.

## 5. Verify and hand off

After changes, rerun the relevant checks and have the reviewer reassess the complete final task diff, including regressions introduced by fixes. A review round can contain the focused rebuttal exchange; another code revision requires another review round. Any subsequent product-code or test edit invalidates the affected check and review evidence.

Declare the task ready only when:

- Acceptance criteria are satisfied and required checks pass on the final code.
- All implementation steps and review changes are committed on the task branch.
- Independent review covers the final task diff.
- Every blocking finding is fixed or has a justified non-blocking disposition under the rules above. A coordinator override is a documented decision, not reviewer approval.

If a blocker or round limit prevents this, report the work as incomplete with the concrete unresolved issue and needed next step. Never invent test results, a clean review, or agreement between agents.

Return one concise handoff for the user's review: what changed, what was verified, material assumptions or limitations, and any significant review disagreements and their resolutions. Include the task branch, base commit, and final commit. Routine agent chatter stays out of the final answer.
