# /cc-commands-jbk:live-release-GC-summary

## Purpose
Generate a **GCS / Sales Rep–facing summary** of everything in the current development → live release. Save it as a markdown file in `docs/releases/`.

This is the **plain-language, audience-tuned counterpart** to `/cc-commands-jbk:live-pr` (which produces the developer/release PR summary). Where `live-pr` summarizes everything for engineering, this command filters to what GCS and sales reps actually need to know and writes it like you're talking to a colleague — no jargon, no internal cleanup, no developer-only changes.

## Audience & Voice
Write for **GCS team members and sales reps**. They are not engineers. They don't care about migrations, refactors, internal cleanup, or backend services. They care about:
- What can they now do that they couldn't before?
- What changed in the proposal editor / Design Suite / quiz flows / chooser modals that they'll see?
- What will their clients and buyers see?
- Are there feature flags that hide things from buyers but show them to admins?
- Is anything launching imminently that needs hands-on prep?

**Voice rules:**
- Conversational, second person ("you'll see…", "click X to…").
- Skip features that are not user-facing (refactors, schema changes, dead-code removal, MCP tool plumbing, log/timeout tuning, test additions, etc.).
- Be **more descriptive (ELI5)** for features that ARE user-facing — don't just rephrase the changelog title, explain what it does and why it matters.
- For **brand-new features** (first time GCS/admins are seeing them), call out **NEW**, frame as a debut, and avoid wording that implies "this used to be there." Example: ❌ "the old picker is gone" ✅ "we're introducing a brand-new product category".
- For **upcoming launches** (feature flagged off for buyers, but visible to admins), explicitly call out the launch timing and what GCS should do this week to prepare. Surface any "PREVIEW ONLY" badges.
- Group related features. If a Genie change is part of a larger launch (e.g. sleeve promotion as part of the Sleeves debut), fold it into the parent feature rather than listing it separately.

## Process

### 1. Identify everything in the release
```bash
git fetch origin
git diff origin/live...origin/development --name-only -- docs/change_log/
```

### 2. Read the relevant change log entries
Focus on `docs/change_log/completed/` and `docs/change_log/pending/` files that touched the diff. Read enough of each to understand:
- What user-facing surface changed (proposal editor, chooser modal, genie quiz, Design Suite, admin pages, etc.)
- Who's affected (admins/agents/GCS, buyers, dev leads, quality team, template designers)
- Whether it's launching now, behind a flag, or admin-only

**Skip entirely** any changelog whose Impact/Overview is purely:
- "Internal only — no user-facing changes"
- Schema/migration/refactor without user-visible behavior change
- Test infrastructure, CI, build/deploy tooling
- MCP tool implementation details (unless GCS uses the chatbot directly)
- Performance/logging/timeout adjustments
- Bug fixes only visible to engineers

### 3. ASK before writing — confirm launch context
Before drafting, **ask the user** about anything that affects framing:
- "Is [Feature X] launching imminently? When?"
- "Is this the first time GCS/admins are seeing [Feature Y], or has it been visible before?"
- "Should buyers see [Feature Z] yet, or is it admin-preview-only?"

These details change the voice of the entire section. Don't guess.

### 4. Group by audience-relevant theme
Suggested grouping (omit any sections with no entries):
- **NEW: [Big launching feature]** — call out preview/launch timing, what GCS can do today
- **NEW: [Other big launching feature]**
- **[Feature improvements to existing tools]** — e.g. modal facelifts, new tabs, new buttons
- **[Quick wins / fixes worth knowing about]** — only if visible (e.g. occasion title fix)
- **[For specific roles]** — e.g. quality team, dev leads — only if relevant to anyone reading

### 5. Write each section
Per feature:
- **One-line description** of what it is, in plain English.
- **Bulleted list** of what GCS can actually *do* with it.
- **Heads up** callout for visibility flags, timing, restrictions, gotchas.
- **"Take this week to..."** action prompt for anything launching soon.

### 6. Save the file
Write to `docs/releases/YYYY-MM-DD_GCS_Sales_Rep_Release_Notes.md` (use today's date — get it from `bash date +%Y-%m-%d` or context).

Confirm the file path back to the user.

## Output
- The markdown file at `docs/releases/YYYY-MM-DD_GCS_Sales_Rep_Release_Notes.md`
- A one-line confirmation of where it was saved

## Reference Example
See `docs/releases/2026-04-15_GCS_Sales_Rep_Release_Notes.md` (the 2026-04-15 release, which introduced Branded Merchandise as preview-only and Custom Box Sleeves as launching-tomorrow). That file is the gold-standard tone, structure, and level of detail.

## Key Principles
- **GCS-only lens** — if a sales rep wouldn't care, leave it out
- **ELI5 the things that matter** — don't assume they know what a "manifest sync" or "config" is
- **Frame new things as new** — never imply something existed before when it didn't
- **Surface launch timing** — preview vs. launching-tomorrow vs. launched-today changes everything
- **Action-oriented** — tell them what to *do* with the info (mock up a proposal, try the flow, etc.)
- **Always ask first** about launch context before writing — don't infer from changelogs alone
