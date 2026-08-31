---
name: clean-final-state
description: Runs a final residue check before delivering code, tests, comments, documentation, PR descriptions, commit messages, and other implementation outputs. Removes traces of rejected or corrected ideas so outputs match the latest accepted requirements. Use before completing any implementation or revision task, when the user rejects or removes an earlier idea (e.g. 不要这个, 删掉, 恢复原来简单的方案), or when preparing commits and PRs.
---

# Clean Final State — Remove Rejected-Idea Residue

## Purpose

When implementing or revising code, optimize for the **desired final state**, not for preserving the history of how we arrived there.

The final code, comments, tests, names, documentation, and PR description should look as though the correct requirement had been understood from the beginning.

## Core Principle

**Do not encode conversation history into the product.**

If the user rejects, removes, or corrects an earlier idea, remove not only the implementation but also unnecessary traces of that idea.

Ask:

> If a new engineer saw this code without seeing our conversation, would anything look oddly defensive, over-explained, or refer to an alternative that does not exist?

If yes, clean it up.

## When an Idea Is Rejected

Suppose an earlier implementation introduced behavior **X**, and the user later says X is unnecessary or should be removed.

Do:

* Remove X.
* Restore the surrounding code to the simplest natural implementation.
* Remove names that exist only to contrast with X.
* Remove comments explaining why X is absent.
* Remove documentation discussing X unless it is genuinely important product knowledge.
* Remove tests whose only purpose is documenting the conversational mistake.
* Rewrite PR titles/descriptions around the intended behavior, rather than around undoing X.

Do **not** turn:

`makeTomatoEgg()`

into:

`makeTomatoEggWithoutDongpoPork()`

Do **not** add:

`// Dongpo pork is intentionally not added here.`

Prefer simply:

`makeTomatoEgg()`

## Target-State Rule

Before finishing, mentally reconstruct the implementation from scratch using only the user's **latest accepted requirements**.

Then compare that clean-room implementation against the current result.

Prefer the version that most closely resembles what you would have written if you had known the final requirements from the start.

## No Correction Scars

Avoid leaving “correction scars” such as:

* comments explaining rejected approaches;
* variable/function names containing `withoutX`, `noX`, `legacyX`, etc. solely because X appeared earlier in the conversation;
* redundant guards against behavior that no longer exists;
* tests for arbitrary rejected ideas;
* PR descriptions narrating every mistaken intermediate attempt;
* documentation explaining why an irrelevant alternative was not chosen.

The repository is not a transcript of the AI conversation.

## Minimal-Diff Rule

When correcting your own unnecessary change:

1. Identify everything introduced because of the rejected assumption.
2. Remove the entire dependency chain of that assumption.
3. Preserve unrelated existing code.
4. Do not compensate for the mistake with additional abstractions, comments, or defensive logic.
5. Re-read the resulting diff from the perspective of someone who never saw the mistake.

Prefer **less code** when the extra code only exists because of an earlier misunderstanding.

## Comments

Comments should explain properties of the system that remain useful to a future engineer.

Do not write comments merely to explain:

* what you previously did wrong;
* what the user told you to remove;
* why an irrelevant implementation is absent.

Bad:

`// We intentionally don't add soy sauce because the user asked us to remove it.`

Good:

No comment at all.

Exception: retain an explanation when there is a **non-obvious invariant, compatibility requirement, security constraint, regression risk, or architectural reason** that future engineers genuinely need to know.

## Tests

Test the intended contract, not arbitrary conversational alternatives.

Bad:

`testDishDoesNotContainDongpoPork()`

when Dongpo pork was merely an AI hallucination.

Good:

`testTomatoEggUsesExpectedIngredients()`

A regression test specifically mentioning the rejected behavior is appropriate only when that behavior represents a realistic recurring bug or important invariant.

## PRs and Commit Messages

Describe the resulting product change.

Avoid narrating AI correction history.

Bad:

`Remove Dongpo pork accidentally added to tomato eggs`

Better:

`Implement tomato-and-egg dish`

If the actual task is genuinely a bug fix in an existing codebase, describing the bug is appropriate. The rule applies to mistakes introduced only during the current implementation process.

## Final Residue Check

Before declaring the task complete, inspect the diff and ask:

1. Does anything mention an idea the user rejected?
2. Does any code exist solely because I previously misunderstood the request?
3. Are there comments that explain the conversation rather than the system?
4. Are names unnecessarily phrased in terms of what the feature is **not**?
5. Did I add defensive logic against a condition that no longer exists?
6. Would this diff look natural to a reviewer who never saw our conversation?

If not, simplify before submitting.

## Default Behavior

When the user says:

* “不要这个”
* “这个没必要”
* “删掉”
* “不用处理这个 case”
* “不要 over-engineer”
* “恢复原来简单的方案”

interpret this as:

> Remove the rejected idea **and its incidental residue**, then produce the cleanest implementation consistent with the latest requirements.

Do not memorialize the rejected idea unless the user explicitly asks for an explanation, migration note, ADR, regression test, or historical documentation.
