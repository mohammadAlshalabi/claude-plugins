---
name: test-plan
description: Use when the user wants to create a test plan / QA plan / test strategy document for a feature, change, or release — especially when they want it published to Google Docs. Triggers on "/test-plan", "write a test plan", "QA plan for this", "test strategy doc", "create a test plan in Google Docs". Drafts the plan grounded in the actual code plus the user's input, then publishes to Google Docs if that connector is available, otherwise writes a local Markdown file with import instructions.
---

# Test Plan Generator

Produce a structured test plan and publish it. Work in three phases: **gather**,
**draft**, **publish**. Don't skip the gather phase — a test plan invented
without looking at the real code or asking about scope boundaries is just filler.

The section structure to follow is in `references/template.md` (bundled next to
this skill). Read it and use it as the skeleton. Drop sections that genuinely
don't apply rather than padding them.

## Phase 1 — Gather (interactive + grounded in code)

Pull what you can from the codebase, then ask the user only for what you can't
infer.

1. **Inspect the code.** Identify what's being tested from the actual repo:
   - If there's an active branch/diff, base the plan on the changed surface
     (`git diff`, changed files). If the user named a feature/area, locate and
     read the relevant code.
   - Note existing tests and the test framework(s) in use — the strategy section
     should build on what's already there, not invent a parallel stack.
   - Note real risk areas you can see (external calls, auth, data migrations,
     concurrency, money, anything hard to reverse).
2. **Ask the user for the gaps** — keep it to a short, batched set of questions,
   don't interrogate. The things that are genuinely hard to infer from code:
   - **Scope boundaries** — especially what's deliberately *out* of scope.
   - **Risk priorities** — which failures would actually hurt (user-facing?
     revenue? data loss?), so the risk register is ranked meaningfully.
   - **Depth / audience** — quick sprint checklist vs. formal sign-off doc; who
     reads it (just the team, or stakeholders/QA/compliance).
   - Owners / dates only if they want the schedule and responsibilities sections
     filled rather than left as placeholders.

   If the user gave a thorough description up front, don't re-ask what they
   already answered.

## Phase 2 — Draft

Fill the template from `references/template.md` using the gathered material.
Ground every section in specifics:

- Real feature names, real file/module names, real flows — not generic
  "the system does X" boilerplate.
- **Test cases** should be concrete and derived from the actual behavior and
  edge cases you saw in the code, each with clear expected results.
- **Risk register** ranked by the user's stated priorities, with likelihood ×
  impact and a real mitigation per row.
- Leave a clearly-marked `<placeholder>` only where information genuinely isn't
  available yet (e.g. an owner the user didn't name) — don't fabricate names,
  dates, or coverage numbers.

Show the draft (or a tight summary of it) to the user before publishing, so
they can correct scope/risks. This is cheaper than fixing it in the Doc.

## Phase 3 — Publish (detect, then fall back)

**First, check whether a Google Drive create-file tool is available in your
current tool set** (a tool whose name ends in `Google_Drive__create_file`, or an
equivalent Drive/Docs "create file" MCP tool).

- **If available → create a native Google Doc.** Call the create-file tool with:
  - `title`: e.g. `Test Plan — <feature name>`
  - `textContent`: the full plan as Markdown
  - `contentMimeType`: `text/markdown`
  - Leave conversion enabled (do **not** set `disableConversionToGoogleType`) so
    Drive converts the Markdown into a real Google Doc with headings/tables.
  - Return the resulting Doc link to the user.
  - Optionally place it in a folder if the user names one (`parentId`).

- **If NOT available → write a local Markdown file and explain the import.**
  - Write the plan to `test-plan-<feature>.md` in the working directory (or a
    path the user chooses).
  - Tell the user how to get it into Google Docs: in Google Drive, **New →
    File upload** the `.md` (or **Docs → File → Open**), and Drive will convert
    Markdown to a Doc; or paste with "Paste from Markdown" enabled.
  - Note *why* it fell back: the Google Drive connector isn't enabled in this
    session. In Claude Code it can be added as an MCP server; in Claude
    Desktop/web via the Google Drive connector in settings.

Whichever path ran, tell the user plainly which one happened and give them the
link or file path.

## Portability note

This skill's core (gather → draft → local Markdown) works anywhere Skills run —
Claude Code, Desktop, and web. Only the direct Google Docs creation depends on
the Google Drive connector being enabled in that surface. Keep the local
Markdown fallback intact so the skill is never a dead end.
