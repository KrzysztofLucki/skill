---
name: idea_vault_manager
description: Manage a file-based Idea Vault (cookbook, stories, Arduino, etc.) with a consistent structure, dashboard, and append-first progress tracking.
---

# Idea Vault Manager (v1)
You manage a user's long-lived ideas/projects on disk (cookbook, stories, Arduino projects, etc.) by maintaining structure, status, and incremental progress.

Skills are instruction manuals. Follow this document as the authoritative specification for structure, rules, and workflows. Each meaningful action should translate into small, auditable file updates.

## When to Use This Skill
Use this skill whenever the user asks to:
- create a new idea/project
- continue an existing idea/project
- add content (recipes, chapters, notes, experiment logs)
- summarize where they left off
- organize, index, dashboard, or update progress across ideas

## Core Principle
File-based memory is the source of truth. You "remember" by reading the vault files, not by guessing.

---

## Safety & Editing Rules (Non‑negotiable)
1. Never delete user files. Archive instead.
2. Prefer APPEND over rewrite.
3. Never rewrite or “retcon” CANON content automatically.
4. If a change is risky/large/ambiguous, write a PROPOSAL instead of editing.
5. Keep ideas separated: operate in one idea folder at a time unless explicitly asked.
6. Never write secrets/credentials into the vault (do not copy tokens, keys, .env contents, etc.).

---

## Vault Root Location
The vault lives inside the workspace at:

idea-vault/

If it does not exist, you may create it using the bootstrap playbook below.

---

## Canonical Vault Folder Layout (Authoritative Spec)

idea-vault/
  DASHBOARD.md
  INDEX.md
  RULES.md
  ideas/
    <idea_slug>/
      STATUS.md
      README.md
      canon/
      drafts/
      artifacts/
      CHANGELOG.md
  archive/
  inbox/

### Meaning of Folders
- ideas/<idea_slug>/canon/     : protected truth (final chapters, finalized recipes, canon rules)
- ideas/<idea_slug>/drafts/    : drafts and experiments (default target for new creative writing)
- ideas/<idea_slug>/artifacts/ : practical outputs (logs, lists, build notes, summaries)
- inbox/                       : quick captures awaiting triage
- archive/                     : retired ideas (never deleted)

---

## Required Vault Files and Their Roles

### DASHBOARD.md (Operational Control Panel)
- Single-glance operational view:
  - Active Focus (top priority)
  - In Progress
  - Parked (do not push)
  - Recently Touched
  - System Notes
- Protocol:
  1) Read DASHBOARD.md before choosing next work (unless user specifies a target).
  2) Update DASHBOARD.md after meaningful work (new files, status changes, milestones).
  3) Never promote Parked → Active without user intent.

### INDEX.md (Catalog / Registry)
- List all ideas with:
  - state (active/paused/archived)
  - 1-line summary
  - last updated
  - folder link (slug)

### RULES.md (Local Rules)
- Plain-language safety rules. Treated as additional constraints.

---

## Per‑Idea Required Files

### STATUS.md (Single Source of Truth)
Must include:
- State: idea | drafting | active | paused | archived
- Created and Last updated
- Current focus (1–3 bullets)
- Next actions (1–5 bullets)
- Constraints / Do-not-change list
- Key files (links)

### README.md (Human Intent)
Short description of what it is and what “good” looks like.

### CHANGELOG.md (Append-only History)
Append entries for meaningful work:
- date
- what changed
- files created/updated
- next step (optional)

---

## Naming Conventions
- idea_slug: lowercase snake_case (e.g., cookbook, story_solaris, arduino_irrigation)
- story chapters: drafts/chNN_<title>.md (default) → canon/ only when explicitly final
- recipes: drafts/<name>.md (draft) or canon/<name>.md (final)
- logs/experiments: artifacts/YYYY-MM-DD_<topic>.md

---

# Playbooks

## Playbook 0 — Bootstrap the Vault (first run)
If idea-vault/ does not exist:
1. Create:
   - idea-vault/
   - idea-vault/ideas/
   - idea-vault/inbox/
   - idea-vault/archive/
2. Create initial files:
   - DASHBOARD.md
   - INDEX.md
   - RULES.md
3. Populate them with the starter content (see “Starter Content” section).

## Playbook A — Create a New Idea
Trigger: user says “create a new idea/project called X”
1. Ensure vault exists (Playbook 0 if needed).
2. Compute idea_slug from the name (snake_case).
3. Create: idea-vault/ideas/<idea_slug>/{canon,drafts,artifacts}/
4. Copy templates from this skill’s templates/idea/ into the new idea folder:
   - STATUS.md, README.md, CHANGELOG.md
5. Fill placeholders minimally:
   - name, dates, 1-paragraph description, initial state
6. Update:
   - INDEX.md: add entry
   - DASHBOARD.md: add to Active Focus or Parked depending on user intent

Ask at most ONE clarifying question only if needed:
- “Should this be active now, or parked?”

## Playbook B — Add Content to an Idea
Trigger: “add a recipe / write a chapter / log an experiment”
1. Read DASHBOARD.md + INDEX.md to locate the target idea (unless user explicitly names it).
2. Read idea STATUS.md to learn constraints + current focus.
3. Write content:
   - default to drafts/ for creative text
   - artifacts/ for logs/lists/notes
   - canon/ only if user explicitly requests “final”
4. Update STATUS.md:
   - Last updated
   - progress bullets
   - next actions (if changed)
5. Append CHANGELOG.md with what you added and file paths.
6. Update DASHBOARD.md:
   - Recently Touched
   - next “Active Focus” bullet if relevant

## Playbook C — Continue Work / “Where did I leave off?”
1. Read STATUS.md and the last 1–3 entries from CHANGELOG.md.
2. Provide a short summary:
   - current state
   - last action
   - next actions (from STATUS.md)
3. If user chooses an action, execute Playbook B and update files.

## Playbook D — Triage Inbox
Trigger: user dumps raw notes without a clear destination
1. Save to: idea-vault/inbox/YYYY-MM-DD_<short-title>.md
2. Optionally propose where it belongs (without moving it automatically).
3. Only file/move it into an idea folder if the user requests it.

---

## Starter Content (use for Bootstrap)
### idea-vault/RULES.md (starter)
- Never delete files; archive instead.
- Append-first editing; avoid rewrites.
- canon/ is protected; drafts/ are flexible.
- Keep one idea per folder; never mix content.
- No secrets/credentials inside the vault.

### idea-vault/INDEX.md (starter)
# Idea Vault Index

## Active

## Parked

## Archived

## Conventions
- Each idea lives in idea-vault/ideas/<idea_slug>/
- Each idea MUST have STATUS.md, README.md, CHANGELOG.md
- Write drafts to drafts/ by default; canon/ only when explicitly final

### idea-vault/DASHBOARD.md (starter)
# Idea Vault Dashboard

Last updated: YYYY-MM-DD

## 🔥 Active Focus (top priority)

## 🟡 In Progress

## 💤 Parked (do not push unless asked)

## 🧠 Recently Touched

## 🛠 System Notes (agent-facing)
- Prefer small incremental updates.
- If uncertain, write a proposal instead of editing canon.
``
