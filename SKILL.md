---
name: to-mvp
description: >-
  Turn an existing codebase into a personalized, self-paced learning course that levels a
  frontend/creative developer up to full-stack by rebuilding the project's core features as an
  MVP. Inspects the repo, produces a high-level architecture/stack overview, then generates an
  ordered, junior-friendly course (easy to advanced) anchored to the project's real code, taught
  in the Gordon Zhu "Watch and Code" style (don't-trust-verify, read code before writing it,
  fundamentals before frameworks, deliberate practice, mastery checkpoints), and packages it as a
  downloadable zip of course directories. Use this whenever the user wants to "learn from this
  project", "build a learning course / curriculum / syllabus from my codebase", "level up to
  full-stack", "make an MVP course", mentions to-mvp, or asks to be taught their own repo as if
  they were a junior developer — even if they don't say "course" explicitly.
---

# to-mvp

Convert an existing project into a structured, build-along course that takes a strong frontend /
creative developer to confident full-stack by reconstructing the project's essential features as
a minimum viable product (MVP). The course is generated **from the learner's actual codebase**, so
every lesson points at real code rather than a generic tutorial, and it is delivered as a zip of
numbered course directories.

The teaching philosophy is non-negotiable and shapes every file you produce: read
`references/gordon-zhu-method.md` before generating anything. The short version is **"Don't trust,
verify."** Reading and understanding code comes before writing it; fundamentals come before
frameworks; practice is active, not passive; and the learner does not advance until a mastery
checkpoint is cleared.

## What the learner wants (the goals this skill must satisfy)

Hold all of these simultaneously — the course fails if any are dropped:

1. **Calibrate to the real learner, not a blank-slate beginner.** The default learner is an
   accomplished frontend / creative developer who is a *junior at the back-end and systems layer*.
   Scaffold every backend concept from the ground up as you would for a junior, but do **not**
   spend modules re-teaching HTML/CSS/component basics they already demonstrate in the repo.
   Front-load the gaps; compress or skip the strengths. State this calibration explicitly in the
   syllabus so the user can correct it.
2. **Understand the existing project first.** Produce a genuine high-level overview: what the
   project is and does, the stack, the architecture, the data model, and how a single request
   travels end to end. Orientation precedes instruction.
3. **Teach the full-stack fundamentals that an MVP actually requires** — data persistence, an API
   layer, validation, auth/authz, server vs client boundaries, config/secrets, testing, and
   deployment — mapped onto *this* project's stack. See `references/fullstack-mvp-map.md`.
4. **Sequence small topics from easy to advanced**, each one anchored to specific files in the
   repo, each building on the last with no hidden prerequisites.
5. **Culminate in a buildable MVP capstone** that exercises the core full-stack features the
   learner just studied — the proof they can ship the whole stack, not just the UI.
6. **Deliver it as a course on disk**: a directory tree (one folder per module) packaged as a zip,
   so it can be worked through offline and at the learner's own pace.

## Workflow

Work through the phases in order. Phase 3 is a **gate**: do not generate the full course until the
syllabus is confirmed.

### Phase 0 — Load the method
Read `references/gordon-zhu-method.md` and `references/course-structure.md` now. They define the
pedagogy and the exact on-disk layout/templates every generated file must follow. Read
`references/fullstack-mvp-map.md` when you reach Phase 2.

### Phase 1 — Inspect and map the project
Locate the codebase (uploaded repo, working directory, or a path the user gives). If no project is
available, ask for one — this skill is worthless without a real codebase to teach from.

Read enough to map it honestly, not to skim:
- **What it is**: the product/purpose in plain language.
- **Stack**: languages, framework(s), database, key libraries, hosting/build tooling. Read
  `package.json` / lockfiles / config rather than guessing versions.
- **Architecture**: top-level structure, the layers (UI, API/server, data), and the boundaries
  between them.
- **Data model**: schema, models, migrations, or the equivalent.
- **The request lifecycle**: trace one real user action from the UI through the server to the
  database and back. This trace becomes the spine of the orientation module.

Capture this as the orientation material — you'll write it into `00-orientation/`.

### Phase 2 — Calibrate the learner and derive the MVP feature set
Confirm the calibration from goal 1 (frontend-strong, backend-junior unless the user says
otherwise). Then read `references/fullstack-mvp-map.md` and intersect the canonical full-stack
fundamentals with what *this* project's stack uses. The output is two lists:
- **Already-covered** (frontend/UI strengths to compress or skip).
- **Gap topics** (the modules to actually teach), each tied to where it lives in the repo.

Define the **MVP capstone**: the smallest version of this project that still demonstrates the full
stack end to end (persist data, expose it via an API, validate and protect it, render it, deploy
it). Strip the project down to its load-bearing features — not a clone, an MVP.

### Phase 3 — Design the syllabus (PLAN FIRST — confirmation gate)
Before writing any course files, produce a syllabus for the user to review and present it in chat:
- the project overview (1 short paragraph),
- the calibration (what's skipped vs taught, and why),
- the ordered module list (title + one line each, easy to advanced), with the real repo files each
  module will anchor to,
- the MVP capstone definition and which modules feed it.

Ask the user to confirm or adjust (sequence, depth, calibration, the MVP scope). Do not proceed
until they respond. This planning-first gate prevents generating a large course in the wrong
direction.

### Phase 4 — Generate the course
Following `references/course-structure.md` exactly, create the directory tree under
`/home/claude/<project-name>-course/`. One numbered folder per module, starting with
`00-orientation/`. Every module folder contains, at minimum: a `README.md` lesson, a
`code-reading.md` (pointers into the real repo with verification questions), `exercises.md` (active
build/break/fix/refactor/test tasks), and `checkpoint.md` (the mastery gate). Keep solutions in a
separate `solutions/` subfolder so the learner can't peek by accident. Write a top-level
`README.md` (how to use the course, the method, prereqs, setup, the full syllabus) and a
`GLOSSARY.md`.

Apply the Gordon Zhu method to every file: lead with reading real code, attach a verification step
to every claim, prefer one deep exercise over five shallow ones, and never let a module end without
a checkpoint.

### Phase 5 — Add the MVP capstone
The final numbered folder is the capstone. Include the MVP spec, a milestone breakdown that
sequences the build, and an acceptance checklist tied to the full-stack fundamentals from Phase 2.
The capstone is where the separate modules converge into one shippable thing.

### Phase 6 — Package and present
Zip the course directory and present it:

```bash
cd /home/claude && zip -r "<project-name>-course.zip" "<project-name>-course" -x "*.DS_Store"
cp "<project-name>-course.zip" /mnt/user-data/outputs/
```

Then use the `present_files` tool on the zip. In your closing message, give a brief tour of the
module sequence and tell the learner where to start (the orientation folder) — keep it short.

## Notes
- This skill produces a **learning course**, not a refactor of the user's project. Never modify the
  source repo; only read from it.
- If the project is small or single-layer, scale the module count down — depth over padding. A
  focused 5-module course beats a bloated 15-module one.
- If the user names a different learning style or says they are a true beginner, honor it, but keep
  the read-code-first, verify-everything backbone unless they explicitly opt out.
