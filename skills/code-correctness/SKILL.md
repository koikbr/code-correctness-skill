---
name: code-correctness
description: Guide and orchestrator for the modular code-correctness lesson tree extracted from the s4.codes reel series. Use this whenever the user wants a code review or refactor focused on readability, naming, functions, comments, formatting, file organization, duplication, side effects, or object-versus-data design. This skill should read the full lesson tree, use the most relevant lessons as the explanation backbone, and explain its reasoning in terms of the relevant lesson intention rather than generic clean-code slogans.
license: GPL-3.0
metadata:
  authors: "koi"
  version: "0.2.0"
  website: "https://koik.com.br"
  github: "https://github.com/koikbr/code-correctness-skill"
  support: "mailto:oi@koik.com.br"
---

# Code Correctness

This is now the entrypoint for the modular `code-correctness` lesson tree.

Do not treat this file as the full source of truth for every principle. Treat it as the routing layer that tells you to load the full lesson tree and then lean on the most relevant lessons when explaining the work.

This lesson tree is grounded in the public `s4.codes` reel series. Keep that attribution intact when describing where the lesson principles came from, and expect the tree to grow as more videos in the series are released.

## What this skill does

- Identify the real readability or correctness problem in the user's code.
- Map that problem to the most relevant lesson themes.
- Read the full lesson tree before answering so the guidance stays grounded in the complete curriculum.
- Explain proposed changes in terms of the lesson intention behind them.
- Keep the refactor restrained and behavior-preserving unless the user asks for change.

## How to use the modular lessons

Follow this sequence:

1. Understand the code and the user's actual problem.
2. Read the full lesson tree across all five themes.
3. Identify which lessons are most relevant to the current problem.
4. Give guidance or make edits using those lesson principles as the reason behind the change.
5. Use the backup monolithic reference only if you need the older all-in-one wording for comparison.

The lessons are meant to complement each other. Even when one lesson is the clear anchor, keep the rest of the curriculum in mind so your guidance stays coherent with the full system.

## Load all lesson modules

Before answering a code-correctness task, read all lesson modules in:

- `meaningful-names/`
- `functions/`
- `comments/`
- `formatting/`
- `objects-and-data/`

Do not treat the lesson tree as a menu where you only skim one file and ignore the rest. The lessons were designed as a sequence, and they reinforce each other. After reading the full tree, let the most relevant lessons carry the explanation for the current task.

## Theme map

Each theme has an intention. When you reference a lesson, explain the intention in plain language: what the code is getting wrong, what the lesson is trying to protect, and why that matters for this edit.

### `meaningful-names/`
Use when the problem is about names that are vague, misleading, hard to search, hard to pronounce, over-encoded, or mentally expensive to decode.

Intention:
- Make names reveal purpose without forcing readers to decode them.
- Prevent names from lying about behavior, type, or role.

Typical lesson files:
- `meaningful-names/01-introduction/SKILL.md`
- `meaningful-names/02-avoid-disinformation/SKILL.md`
- `meaningful-names/03-meaningful-distinctions/SKILL.md`
- `meaningful-names/07-avoid-mental-mapping/SKILL.md`

Typical triggers:
- confusing variable names
- misleading names
- meaningless distinctions
- bad naming conventions

### `functions/`
Use when the problem is about long functions, mixed responsibilities, argument design, switches, side effects, command-query separation, duplication, or refactoring toward smaller units.

Intention:
- Make functions read like a short honest story.
- Keep responsibilities focused and side effects visible.
- Stop readers from jumping between high-level decisions and low-level mechanics.

Typical lesson files:
- `functions/01-short/SKILL.md`
- `functions/02-do-one-thing/SKILL.md`
- `functions/03-level-of-abstraction/SKILL.md`
- `functions/08-side-effects/SKILL.md`
- `functions/09-command-query-separation/SKILL.md`

Typical triggers:
- this function is too long
- this function does too much
- too many parameters
- hidden mutation
- duplicated logic

### `comments/`
Use when the problem is about comments that are stale, noisy, misleading, apologetic, redundant, visually cluttered, or standing in for missing refactors.

Intention:
- Keep comments for information the code cannot carry by itself.
- Remove comments that drift, distract, or compensate for weak structure.

Typical lesson files:
- `comments/01-introducing-comments/SKILL.md`
- `comments/02-explaining-context-and-intent/SKILL.md`
- `comments/05-redundance-and-noise/SKILL.md`
- `comments/06-misleading-and-confusing/SKILL.md`
- `comments/10-the-refactoring-principle/SKILL.md`

Typical triggers:
- should this comment exist
- clean up comments
- this code needs too many comments
- comments are outdated

### `formatting/`
Use when the problem is about file reading flow, visual grouping, declaration placement, or structural readability within a file.

Intention:
- Make the file read top-down with visual grouping that supports understanding.
- Use structure and placement so readers see the story before the details.

Typical lesson files:
- `formatting/01-code-reading-flow/SKILL.md`
- `formatting/02-visual-code-layout/SKILL.md`
- `formatting/03-placement-and-rules/SKILL.md`

Typical triggers:
- hard to scan
- file feels messy
- code organization is confusing
- function order is awkward

### `objects-and-data/`
Use when the problem is about abstraction boundaries, exposing behavior versus raw data, or deciding between object-oriented and procedural designs.

Intention:
- Choose the representation that matches the likely direction of change.
- Avoid object-heavy design when data plus procedures is the clearer fit, and avoid raw data exposure when behavior should own the rules.

Typical lesson files:
- `objects-and-data/01-data-abstraction/SKILL.md`
- `objects-and-data/02-data-object-anti-symmetry/SKILL.md`

Typical triggers:
- should this be a class
- too much raw data exposure
- object versus function design
- wrong abstraction boundary

## Using the full curriculum

The lessons are meant to complement each other.

- Start with the lesson that best explains the main problem.
- Bring in supporting lessons when they sharpen the reasoning.
- Keep the explanation centered on the most relevant few lessons, even though the whole tree was loaded.
- Let the full curriculum shape your judgment, but do not dump every lesson into the response.

Common combinations:

- `meaningful-names/02-avoid-disinformation` + `functions/02-do-one-thing`
  When misleading names are a symptom of mixed responsibilities.

- `functions/03-level-of-abstraction` + `formatting/01-code-reading-flow`
  When the code is hard to follow because orchestration and details are mixed.

- `comments/06-misleading-and-confusing` + `meaningful-names/01-introduction`
  When comments exist only because the names and structure are weak.

- `objects-and-data/02-data-object-anti-symmetry` + `functions/04-switch-statements`
  When branching design and type design are tangled together.

## How to explain changes

When recommending or making a change:

- explain what the code is getting wrong,
- explain which lesson file or lesson principle applies,
- explain the intention behind that lesson,
- explain why that intention matters here,
- and then show the smallest useful fix.

The lesson reference should sound like a reason, not like a citation ritual.

Good pattern:

- `This name forces mental decoding, so I am applying Meaningful Names / Avoid Mental Mapping. The intention is to make the code understandable without a translation step.`

Good example:

- `Rename this because the current name is disinformation: it says list, but the value is a map. This matches Meaningful Names / Avoid Disinformation.`

Bad example:

- `This is more clean-code.`

Do not mention lesson names mechanically. Mention them when they help anchor the reasoning in a real principle.

## Restraint rules

- Do not load many lesson modules just because they exist.
- Do not use lesson references as permission to over-refactor.
- If the code is already clear and honest, say so.
- If two lessons point in different directions, explain the tradeoff instead of pretending there is one obvious answer.

## Backup reference

If you need the previous all-in-one version of this skill, read:

- `references/monolithic-backup.md`

That file is now a fallback reference, not the primary operating model.
