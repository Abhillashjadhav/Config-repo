---
name: prd-first
description: Forces a written PRD before any code generation. Use this skill the moment the user says "build", "create app", "make me a", "vibe code", "let's build", "I want an app that", "spin up a", "prototype a", "ship a", or any phrasing that signals they want code generated from a high-level idea. Also use when the user references an existing project but the conversation has no PRD context (no /prds/ file referenced, no clear success metric). The skill blocks code generation until a 5-field PRD exists as a markdown file in the repo. Do NOT use when the user is asking factual questions, debugging existing code, editing a specific file they've named, or working in a repo where a PRD for this feature already exists at /prds/. Skip when user explicitly says "skip the PRD" or "just code it" — but flag the risk once before proceeding.
---

# PRD-First Discipline

The user (Abhillash) vibe-codes 10-15 apps and loses track of what each app is actually doing because Claude generates code from high-level intent without a written contract. This skill forces a 10-minute thinking pass before any code generation. The PRD is the contract; the code must satisfy it; future-Abhillash can read the file three months from now and remember why.

## The hard rule

**No PRD, no code.** When the trigger fires, refuse to generate code or vibe-code prompts until a PRD exists as a markdown file in the repo at `/prds/YYYY-MM-DD-<slug>.md`. This is non-negotiable except when the user explicitly overrides with "skip the PRD" — in which case flag the risk once, then proceed.

## The 5-question protocol

Ask **one question at a time.** Wait for the answer. Do not batch. This matches the user's learning style (theory → quiz → build, one question at a time).

Each question has a quality bar. If the answer is vague, ask one follow-up. Then move on — don't gold-plate.

**Question 1 — Problem:**
> What hurts today, and for whom?

Quality bar: a specific pain, not a feature wish. "I want a dashboard" is a feature; "I'm losing 30 minutes a day reconciling Stripe payouts against orders" is a problem. If they answer with a feature, ask: "What's the underlying pain that makes you want that?"

**Question 2 — User:**
> Who specifically uses this, and what's their current alternative?

Quality bar: a real person (you, your team, a known segment) and a named alternative ("I do it in a spreadsheet now", "we use Notion but it doesn't sync"). If "everyone" — push back: "Pick the one user whose problem we're solving first."

**Question 3 — Success:**
> One metric, one number, one timeframe — when do we know this worked?

Quality bar: measurable. "Faster reconciliation" fails. "Reconcile a day's payouts in under 3 minutes by end of week 2" passes. If they can't name a number, accept a binary: "Does the thing I described in Q1 still happen on Friday? Yes/No."

**Question 4 — Scope cuts:**
> What are we explicitly NOT building in v1?

Quality bar: at least 3 things named. This is the most important question. If they say "nothing, build it all", push back hard: "Name three things you're cutting. Vibe-coded apps die from scope creep, not from missing features."

**Question 5 — Non-goals:**
> What would make us call this a failure even if it ships?

Quality bar: at least one named failure mode. Examples: "if it costs more than $5/month to run", "if I have to maintain it manually every week", "if non-technical users can't open it without help". This catches the class of bugs where you build the right thing correctly but it's still useless.

## What to do with the answers

After all 5 questions answered, write the PRD to `/prds/YYYY-MM-DD-<slug>.md` using this exact template:

```markdown
