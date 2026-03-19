---
name: code-correctness
description: Apply clean-code and correctness-oriented refactors to existing code. Use this skill for review or refactor work centered on readability, maintainability, naming, comments, function design, duplication, hidden side effects, flag arguments, command-query separation, abstraction level, file organization, or code that feels hard to follow, noisy, or harder to change than it should be.
license: GPL-3.0
metadata:
  authors: "koi"
  version: "0.1.2"
  website: "https://koik.com.br"
  github: "https://github.com/koikbr/code-correctness-skill"
  support: "mailto:oi@koik.com.br"
---

# Code Correctness

Use this skill when the problem is not just whether code runs, but whether the code tells the truth.

Code gets written once and read over and over. When a name makes the reader stop, when a function mixes decisions with mechanics, when a comment tells the old story while the code tells a new one, or when a harmless-looking call quietly mutates state, the code stops being trustworthy. It may still work, but it starts lying.

Your job is to make the code easier to trust, easier to follow, and safer to change.

## Default posture

- Read the code until you understand what it is trying to do.
- Keep behavior stable unless the user asked for a behavior change or the current behavior is plainly wrong.
- Keep public contracts stable unless it is clearly safe or explicitly useful to change them.
- Be much less conservative with locals, parameters, private helpers, and internal structure.
- Prefer the smallest change that makes the code read better.
- If the code is already clear and honest, do not touch it just to leave fingerprints.
- Remember that readability is not decoration. It is how the next developer avoids making the wrong change.

## Working order

1. Figure out the real job of the code.
2. Decide whether the right move is refactoring or restraint.
3. Look for the highest-cost lie: bad names, mixed responsibilities, hidden mutation, stale comments, awkward control flow, or duplication.
4. Fix the core problem first.
5. Re-read the result top to bottom and check that the code now says what it actually does.

If you can describe the code in plain language after the refactor, but had to decode it before, you are probably moving in the right direction.

## What good output looks like

Aim for code that:

- reads without translation,
- keeps one level of abstraction at a time,
- makes side effects visible,
- does not depend on comments for basic clarity,
- stays easy to search and discuss,
- and does not add ceremony for its own sake.

## Judgment calls

These matter as much as the usual clean-code rules.

### Prefer restraint over churn

- A zero-diff result is valid.
- Do not rename things just to make them sound fancier.
- Do not introduce helpers, wrappers, value objects, or type aliases unless they make the code easier to follow.
- Do not "improve" code that is already compact, idiomatic, and truthful.
- The goal is not to look active. The goal is to reduce reader effort.

### Preserve real context

- Keep comments that carry domain knowledge, protocol history, production constraints, legal requirements, or hard-won operational lessons.
- Remove comments that only narrate the mechanics.
- If a comment still matters, keep it or rewrite it more clearly. Do not delete it just because comments are usually bad.
- A good comment protects future maintainers from a mistake the code alone would not prevent.

### Respect the environment the code lives in

- Hot paths need judgment. Do not trade direct code for pretty but slower abstractions.
- Framework code may look odd for a reason. Understand the idiom before changing it.
- Tests are specs. A little repetition is fine when it keeps each case obvious.
- If the prompt explicitly allows redesign, do not hide behind compatibility when the interface itself is the problem.
- The best refactor depends on context. The same change can be clean in one file and wrong in another.

## Naming

### Use names that carry meaning

- Replace vague names like `data`, `value`, `item`, `tmp`, `res`, or single letters when the scope is larger than a tiny loop.
- Name things for what they mean in the domain, not for their type.
- Pick the most accurate name, not the most dramatic one.
- If a comment is working hard to explain a name, the name is probably the real problem.
- A good name should tell the reader why the thing exists, what role it plays, and how to use it without sending them hunting through the file.

### Make distinctions real

- Different names should mark real differences in responsibility or behavior.
- Avoid filler suffixes like `Manager`, `Handler`, `Processor`, or numbered variants unless they actually help a reader choose correctly.
- Do not make readers keep a private abbreviation dictionary in their head.
- If two things have different names, a reader should feel the difference before opening the implementation.

### Keep names speakable and searchable

- Prefer names people can say out loud in review.
- Reserve short throwaway names for truly tiny local scopes.
- Turn magic values into named constants when the value has domain meaning.

## Functions

### Keep the top level readable

- A function should read like a short story, not a pile of mechanics.
- Push detail down when it helps the main path read clearly.
- Do not extract helpers mechanically. A helper should earn its place by making the caller simpler.
- The top-level function should read like the headline. The lower-level helpers can carry the details.

### One job per function

- If a function validates, transforms, saves, logs, and notifies, it is doing too much.
- Split work along real responsibility lines.
- A function should have one reason to change.
- If you can divide a function into named sections, that is usually a sign that the function is carrying more than one idea.

### Keep one level of abstraction at a time

- Do not mix business policy with low-level detail in the same breath.
- Let higher-level functions orchestrate.
- Let helpers handle the lower-level steps.
- Good code should read top-down, one layer at a time, like a small narrative that keeps descending into detail only when needed.

### Be honest about mutation

- If a function mutates state, the name and structure should make that obvious.
- Separate pure checks from commands when you can.
- Query-shaped names should not quietly write to sessions, caches, files, or shared fields.
- A function that sounds harmless but changes the world is the kind of lie that turns into a bug later.

### Avoid awkward call surfaces

- Too many arguments usually means the code is missing a concept.
- Flag arguments often mean one function is doing multiple jobs.
- Output arguments hide data flow.
- When redesign is allowed, fix the awkward surface itself. When redesign is not allowed, clean up the internals and keep the public shape as a thin compatibility layer if needed.
- Arguments carry mental weight. If the call site makes the reader stop to decode order or meaning, the design still needs work.

### Remove duplication carefully

- Duplication is bad when the same rule has to be updated in multiple places.
- Duplication is fine when removing it would hide the main story or make tests harder to read.
- Do not chase DRY so hard that the code becomes indirect.
- One truth in one place is the goal, but not at the cost of turning readable code into a maze.

## Comments and docs

### Treat comments with suspicion, not contempt

- Most comments should not need to exist.
- Some comments are the only place where the important why lives.
- First try to make the code clearer.
- If the why still cannot live in code, keep the comment.
- Comments are expensive because they create a second thing that can drift out of sync.

### Good comments usually explain why

- Keep comments about constraints, tradeoffs, weird external requirements, or surprising decisions.
- Delete comments that restate obvious code.
- Delete dead history, commented-out code, and personal diary notes from source files.
- Do not use comments as lane markers for oversized functions. Fix the structure instead.
- If a comment only says what the code already says, it is noise. If it protects a business rule or a non-obvious constraint, it earns its place.

### Public APIs need real documentation

- If other people consume the code without reading the implementation, document inputs, outputs, failure modes, and the basic usage shape.
- Keep that documentation plain and specific.

## Files and layout

### Make files easy to scan

- Put the main flow near the top when the language and local style allow it.
- Keep related helpers together.
- Use whitespace to separate ideas, not to decorate the file.
- A file should read like a newspaper: the headline first, supporting detail below, related material grouped together.

### Keep scope local

- Declare locals near where they are used.
- Keep shared class state in a predictable place.
- Do not make readers carry unused names for half a function.
- Every variable introduced too early becomes mental baggage until the reader finally discovers why it exists.

### Follow the house style

- Braces, quotes, tabs, spaces, and similar style details are team conventions.
- Preserve coherent local style unless inconsistency is making the file harder to read.

## Fast checklist

Before finishing, check:

- are the names honest?
- does each function have a clear job?
- are side effects easy to spot?
- did you preserve the right comments and delete the useless ones?
- did you avoid abstraction that only adds ceremony?
- did you respect hot paths, framework idioms, and test readability?
- did you leave good code alone where that was the best choice?

## Release notes

### 0.1.1

- tightened the wording to be more direct and less generic
- kept the same guidance, but stripped out fluff and repeated prompt-like phrasing

### 0.1.2

- brought back more of the explanatory voice from the source transcripts
- kept the text direct, but restored the didactic parts that help an agent judge code instead of just applying rules

### 0.1.0

- first public release
- transcript-derived guidance for naming, functions, comments, side effects, restraint, and code intent
- hardened through iterative and adversarial evaluation
