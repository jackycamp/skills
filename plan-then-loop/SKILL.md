---
name: plan-then-loop
description: Clarify requirements and obtain approval of a proposed plan before running dev-loop autonomously. Use when the user wants a plan checkpoint before implementation, testing, and independent review.
---

# Plan then loop

Coordinate two phases: planning with the user, then autonomous execution through `dev-loop`.

Resolve `dev-loop` from the available skill catalog. Read its instructions to prepare the handoff. If the skill is unavailable, locate it before proceeding. Report a blocker if it cannot be found. Keep its workflow in that skill instead of duplicating it here.

## 1. Clarify and plan

Keep this phase read-only for the project and Git state. Do not edit product files, create branches or worktrees, stage changes, or create commits. Store planning notes in task scratch space when necessary.

Inspect repository guidance, relevant code, Git status, and the available `main` or `master` base. Record existing changes and the proposed base commit. Inspect the base without switching branches.

Spawn a separate planner with the requirements and repository context. Instruct the planner to inspect code and prepare a practical plan. The planner must not implement changes. Ask for acceptance criteria, integration points, behavior choices, and relevant checks.

Ask the user about missing requirements or choices that materially affect behavior, scope, or validation. Group related questions. State reasonable assumptions for minor choices. Continue independent inspection while answers are pending. Do not treat answers to requirements questions as approval to implement.

Resolve required questions before requesting plan approval. Review the planner's proposal and present a concise plan with:

- The intended behavior and acceptance criteria.
- The implementation approach and relevant files or modules.
- The proposed task branch and base commit.
- The validation approach and selected review skills.
- Material assumptions, scope limits, and tradeoffs.

## 2. Obtain plan approval

Wait for explicit user approval before implementation.

Ask the user to approve the presented plan or request changes. Explain that approval starts the remaining development loop without routine stage approvals. Link this skill when explaining the checkpoint. Quote its requirement: "Wait for explicit user approval before implementation."

End the turn after presenting the plan and requesting approval. Do not start the implementer while approval is pending. Silence, elapsed time, a clarification answer, and an unrelated earlier approval do not satisfy this checkpoint.

If the user requests changes, revise the plan and present the revision for approval. A clear approval of the current plan permits execution. If the reply is ambiguous, clarify before implementation.

Keep a compact task record with the current plan, requirements, base commit, and approval status. Record which plan the user approved. Reuse this record when the conversation resumes.

## 3. Run the approved loop

After approval, apply `dev-loop` with the accepted plan and task record. Reuse the completed planning stage. Start its implementation stage after the required setup. Do not create a second plan checkpoint for the same accepted scope.

Instruct the coordinator to preserve the approved requirements and acceptance criteria. Carry forward the selected review skills, assumptions, and any user-specified budget.

Follow the Git and commit rules in `dev-loop`. These include the push prohibition, branches from `main` or `master`, and worktrees only when requested. They also require incremental implementation commits, new commits for review changes, and short commit messages that follow `asd-ste`.

Run implementation, testing, independent review, and fixes or reasoned pushback under `dev-loop`. Resolve routine design choices and review disagreements without further user checkpoints.

If new evidence requires a material change to the approved scope or behavior, present the revised plan for approval. Routine implementation adjustments within the accepted scope do not require renewed approval. Other blockers and tool permissions follow the active session rules.

Use the completion criteria and final handoff from `dev-loop`. Include material deviations from the approved plan. Report incomplete work honestly if a blocker or budget limit prevents completion.
