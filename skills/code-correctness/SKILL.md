---
name: code-correctness
description: Apply clean-code and correctness-oriented refactors to existing code. Use this skill whenever the user wants a code review or refactor focused on readability, maintainability, naming, comments, function design, duplication, hidden side effects, flag arguments, command-query separation, abstraction level, file organization, or clean-code smells. Trigger even when the user does not say "clean code" and instead says things like "this function is too long," "these names are confusing," "this code is hard to follow," "please clean this up," "make this easier to maintain," or "refactor without changing behavior."
license: GPL-3.0
metadata:
  authors: "koi"
  version: "0.1.3"
  website: "https://koik.com.br"
  github: "https://github.com/koikbr/code-correctness-skill"
  support: "mailto:oi@koik.com.br"
---

# Code Correctness

Code gets written once and read over and over. When a name makes the reader stop, when a function mixes decisions with mechanics, when a comment tells the old story while the code tells a new one, or when a harmless-looking call quietly mutates state, the code stops being trustworthy.

Use this skill to make the code easier to trust, easier to follow, and safer to change.

## Working stance

- Start by identifying the code's real job before proposing edits.
- Prefer the smallest refactor that makes intent obvious.
- Preserve behavior unless the user asked for behavioral change or the current behavior is clearly misleading or broken.
- Preserve external contracts unless it is clearly safe to change them. Be especially careful with exported names, public method names, return shapes, and persisted field names.
- Do not confuse interface stability with local naming conservatism. Parameter names, local variables, and private helpers should usually be improved aggressively when that does not change the observable contract.
- When a public API is awkward but must remain stable, introduce clearer internal entry points and keep the old API as a thin compatibility wrapper when that materially improves the design.
- Do not refactor just because a refactor is possible. If the code is already clear, compact, and honest, prefer restraint.
- Remember that readability is not decoration. It is how the next developer avoids making the wrong change.
- Favor code that explains itself over code that requires explanatory comments.
- Preserve existing project conventions when they are coherent; standardize only where inconsistency is actively hurting readability.
- Do not over-refactor. Every extracted helper should earn its place by making the calling code easier to understand.
- Respect context-specific tradeoffs such as framework idioms, performance-sensitive paths, public API obligations, and the different readability goals of test code.

## Operating flow

Follow this order when applying the skill:

1. Read the surrounding code to understand the intent, inputs, outputs, and side effects.
2. Decide whether the code actually needs refactoring, or whether restraint is the higher-quality move.
3. Compare names, comments, structure, and signatures against the actual behavior.
4. Identify the highest-cost readability or correctness problems first.
5. Refactor toward smaller units, clearer names, cleaner boundaries, and more local state.
6. Re-check that the edited code still tells the same story as the behavior.

If you can describe the code in plain language after the refactor, but had to decode it before, you are probably moving in the right direction.

During refactoring, prefer improvements that keep the surface area stable while making the internals clearer.

## What to optimize for

Optimize for code that:

- tells the truth about what it does,
- does not force readers to decode names,
- keeps one level of abstraction per function,
- makes side effects visible,
- minimizes duplication,
- uses comments only for information code cannot carry,
- and stays easy to search, test, discuss, and change.

## Priority order

When several improvements are possible, prefer this order:

1. Fix misleading names.
2. Split mixed responsibilities.
3. Remove hidden side effects.
4. Reduce duplication.
5. Replace comment-dependent clarity with code-dependent clarity.
6. Improve file and formatting structure.

## Judgment rules

### Prefer restraint when the code is already good


- A needless refactor creates churn without adding understanding.
- Over-editing is a correctness risk because every change has blast radius.

- Leave already-good code mostly alone.
- Fix only the genuinely weak point if one exists.
- A zero-diff result is valid when the code is already clear, idiomatic, and honest.
- Do not make polish-only changes such as ceremonial type aliases, annotation churn, or renames that add no real clarity.
- Do not invent helpers, wrappers, or abstractions just to look active.
- The goal is not to look active. The goal is to reduce reader effort.
- Do not rename semantically precise identifiers into more dramatic but less accurate names.

### Respect domain comments that carry real knowledge


- Some knowledge cannot be recovered from the code alone.
- Deleting those comments can erase the reason the code is safe.

- Keep comments that explain external constraints or surprising historical facts that still matter.
- Rewrite those comments only to make them clearer or more precise.
- Remove only the comments that restate mechanics or add noise.
- A good comment protects future maintainers from a mistake the code alone would not prevent.

### Respect performance-sensitive paths


- Some readability patterns add allocations, callbacks, indirection, or repeated work that are harmless elsewhere but inappropriate here.
- A pretty refactor can still be a bad engineering decision.

- Improve naming and local clarity first.
- Keep direct loops and compact logic when they are the right tradeoff.
- Do not replace simple hot-path code with abstraction-heavy style unless the gain clearly outweighs the cost.

### Respect framework idioms and semantic patterns


- Some code is non-obvious for semantic reasons, not because it is sloppy.
- Flattening those patterns into "cleaner" code can silently break behavior.

- Understand why the pattern exists before changing it.
- Preserve stale-closure guards, lifecycle ordering, hook semantics, transactional boundaries, or other framework-specific constraints when they are intentional.
- If the idiom is already clear, prefer no change over cosmetic cleanup.
- Improve naming and surrounding structure without erasing the semantic reason for the pattern.
- In framework state/ref code, keep names aligned with the actual time or ownership semantics. Prefer `latest`, `current`, or `previous` when those are the literal meanings.

### Treat tests as readability-first specifications


- Tests are read to understand behavior, not just to remove duplication.
- Repetition in tests is often acceptable when it keeps each case obvious.

- Prefer explicit test intent over clever abstraction.
- Deduplicate only when duplication is genuinely hiding the scenario.
- Do not turn simple tests into loops, helpers, or meta-structures that obscure what each case proves.

### Be bolder when redesign is explicitly allowed


- Over-learning "preserve the interface" can turn into timidity.
- Sometimes the cleanest and most honest fix is to redesign the awkward surface itself.

- If the prompt explicitly allows interface change, evaluate the public design itself instead of assuming it is fixed.
- Remove flag-driven or mixed-responsibility APIs when redesign is clearly the better outcome.
- Do not keep a bad surface forever just because earlier evals trained caution.

## Naming rules

### Use intention-revealing names


- Code is read far more often than it is written.
- A weak name makes every future read slower.
- A name that hides intent forces the reader to reconstruct meaning from context.

- Replace vague names such as `data`, `value`, `item`, `d`, `res`, or `tmp` with names that reveal business purpose.
- Prefer names that explain what the value means in this code, not what type it happens to be.
- Prefer the most semantically exact name, not the most elaborate-sounding one.
- If a comment exists only to explain a name, improve the name first.
- A good name should tell the reader why the thing exists, what role it plays, and how to use it without sending them hunting through the file.

### Make meaningful distinctions


- Different names imply different roles.
- Generic suffixes create fake distinctions and increase hesitation.

- Avoid filler names like `Manager`, `Handler`, `Processor`, `Info`, or numbered variants unless the distinction is real and obvious.
- Rename items according to their specific responsibility.
- Make behavioral differences visible in the names themselves.
- If two things have different names, a reader should feel the difference before opening the implementation.

### Use pronounceable names


- Code is discussed aloud in reviews and pairing.
- Unpronounceable names are harder to remember and harder to coordinate around.

- Prefer real words over compressed abbreviations.
- Rename identifiers that cannot be said naturally in conversation.

### Use searchable names


- Single letters and raw numbers are difficult to search meaningfully.
- Searchability is part of maintainability.

- Reserve single letters for tiny, obvious local scopes.
- Give constants, fields, helpers, and reused values names that can be found quickly across the codebase.
- Replace magic numbers with named constants when the value carries domain meaning.

### Avoid encodings


- Type encodings spend name-space on information the IDE already knows.
- That pushes out the more important information: purpose.

- Remove Hungarian notation or similar prefixes when they only restate type.
- Keep the name focused on intent and role.

### Avoid mental mapping


- Every translation consumes attention.
- The reader should spend effort on behavior, not decoding vocabulary.

- Replace cryptic abbreviations with domain words.
- Make formulas, predicates, and loops readable without a translation table.

## Function rules

### Keep functions small


- Long functions hide intent under detail.
- The reader has to separate story from implementation by hand.

- Extract dense blocks, nested loops, and low-level mechanics into helpers with names that expose intent.
- Let the top-level function read as a short sequence of meaningful steps.
- The top-level function should read like the headline. The lower-level helpers can carry the details.

### Make each function do one thing


- Multiple responsibilities create multiple reasons to change.
- Mixed tasks make testing and modification riskier.

- If you can name internal sections differently, split them.
- If an extracted helper would get a meaningful name, that responsibility likely deserves its own function.
- Keep the top-level function centered on one idea.
- If you can divide a function into named sections, that is usually a sign that the function is carrying more than one idea.

### Keep one level of abstraction per function


- Mixed abstraction obscures flow.
- The reader gets dragged between policy and syntax.

- Keep orchestration code high-level.
- Push implementation details into subordinate helpers.
- Aim for top-down reading where each function leads naturally to the next depth level.
- Good code should read top-down, one layer at a time, like a small narrative that keeps descending into detail only when needed.

### Bury repeated switches


- Repeated switches spread change risk.
- Adding one new case means editing many sites.

- Consolidate creation or dispatch in one place.
- Prefer polymorphism or behavior objects when repeated branching is part of the design.

### Keep argument count low


- Extra parameters create ordering confusion and increase misuse risk.
- Long parameter lists often indicate a missing abstraction.

- Prefer zero, one, or two arguments when practical.
- When arguments pile up, look for a missing object or concept.
- Group related data under a meaningful name.

### Avoid flag arguments


- Flag arguments often mean one function is doing two jobs.
- Call sites become less readable because `true` or `false` rarely explains itself.

- Split flag-driven branches into separate functions when they represent different behaviors.
- Keep boolean data only when it is actual domain data, not a mode selector.
- Avoid replacing one unclear flag with a combinatorial explosion of wrapper functions. Prefer a cleaner core function plus explicit orchestration where needed.

### Avoid output arguments


- Output arguments hide data flow.
- They violate the common expectation that data comes in through arguments and leaves through return values.

- Return values instead of mutating caller-owned output containers.
- Make data movement visible at the call site.

### Preserve interface stability while refactoring internals


- Many readability fixes are internal and do not require breaking external callers.
- Renaming a public function or changing returned field names can create accidental behavior changes even when the internals improve.

- First improve internals without changing the public contract.
- Only rename exported or externally consumed interfaces when it is clearly safe or explicitly requested.
- Prefer local variable renames, parameter renames, helper extraction, and internal restructuring before API churn.
- If the prompt explicitly allows redesign, reconsider whether preserving the old interface is still the right goal.
- Arguments carry mental weight. If the call site makes the reader stop to decode order or meaning, the design still needs work.

### Be careful with pairs and triads


- Some pairs are natural, but unrelated pairs and most triads create confusion.
- Readers must remember both meaning and ordering.

- Keep only natural pairs as peers.
- Turn unrelated arguments into an object or move one concept onto the owning object.
- Use names that make order obvious when order matters.

### Eliminate hidden side effects


- Hidden side effects create surprise bugs.
- Callers trust names; misleading names create unsafe assumptions.

- Rename the function if mutation is its true job.
- Prefer splitting the check from the mutation.
- Make side effects explicit in structure and naming.
- A function that sounds harmless but changes the world is the kind of lie that turns into a bug later.

### Separate commands from queries


- A question should be safe to ask.
- Mixing commands and queries makes calls harder to reason about and reuse.

- Use one function to return information and another to perform the mutation.
- Keep query-like names read-only.
- Keep command-like names obviously mutating.
- If an existing public function mixes both and cannot be safely removed, keep it as a compatibility wrapper over clearer internal command/query operations.

### Prefer exceptions over error codes when appropriate


- Error codes spread branching across every call site.
- Shared status enums couple many files together.

- Let the success path read as straight-line logic.
- Handle exceptional flow in dedicated error handling paths.
- Keep happy-path code and error-handling code clearly separated.

### Remove duplication


- Duplication multiplies maintenance work.
- One missed update creates inconsistent behavior.

- Extract shared request logic, calculations, policy rules, and validation.
- Keep one authoritative representation of each rule or idea.
- One truth in one place is the goal, but not at the cost of turning readable code into a maze.

### Rewrite toward clarity

Apply this after code is working but still messy.

- First drafts are usually organized around getting the code to run.
- Clean structure often appears only after refactoring.

- Use tests as a safety net.
- Improve names, split functions, remove duplication, and simplify the flow.
- Treat refactoring as part of finishing the work.

## Comment and documentation rules

### Start from skepticism

Default assumption: most comments should not exist.

- Code evolves faster than comments.
- A stale comment is worse than no comment because it misleads confidently.

- First try to remove the need for the comment by renaming, extracting, or restructuring.
- Keep comments when the code cannot carry the important information alone, especially for external constraints, surprising tradeoffs, or protocol history that still matters.
- Comments are expensive because they create a second thing that can drift out of sync.

### Use comments for why, not what


- Code usually shows how.
- Comments are most valuable when they explain business rules, tradeoffs, or external constraints.

- Comment business reasons, legal constraints, performance tradeoffs, and surprising decisions.
- Do not restate code that already reads clearly.
- If a comment only says what the code already says, it is noise. If it protects a business rule or a non-obvious constraint, it earns its place.

### Use warnings, amplification, and TODOs deliberately


- Some lines look unnecessary until context is known.
- Without a warning, later cleanup may silently break behavior.

- Add warnings where an obvious-looking simplification would be dangerous.
- Add amplification where a small line has disproportionate impact.
- Keep TODOs only when there is a real blocker, dependency, or deferred external fix.

### Document public APIs for external readers


- Internal code and public APIs have different audiences.
- External users need usage guidance, not implementation archaeology.

- Document what it does, inputs, outputs, failure modes, and a small example.
- Keep legal headers standard and minimal when required.

### Remove noisy comments


- Noise comments double scan time without adding meaning.
- Too much noise trains readers to ignore all comments.

- Delete comments that merely restate the code.
- Delete mandatory-but-empty doc comments.
- Interrupt the reader only when there is information the code cannot deliver.

### Remove misleading comments


- A misleading comment creates false confidence.
- Readers stop checking the code because they think the comment already explained it.

- Keep comments specific, local, and easy to verify against nearby code.
- Delete or rewrite comments that require guessing.
- Do not document behavior that is actually controlled somewhere else unless the dependency is explicit and stable.

### Delete dead history from source files


- Version control already keeps history better.
- Inline history clutters present-day reasoning.

- Remove commented-out code.
- Remove attribution headers and manual change logs from source files.
- Trust version control for recovery and history.

### Do not use comments as structural crutches


- Section markers and closing-brace comments usually signal structure problems, not documentation needs.

- Shorten the function.
- Split the file by responsibility.
- Use comments as the last fix, not the first one.

### Keep comments plain and readable


- Comments should be readable in raw source form.
- If the comment itself is hard to read, it has failed.

- Use plain text.
- Remove vague notes and irrelevant history.
- Rewrite or delete comments that are hard to parse quickly.

## File and formatting rules

### Organize files like a newspaper


- The reader should meet the highest-level idea first.
- Predictable ordering lowers navigation cost.

- Put top-level orchestration near the top of the file.
- Define helpers below the code that calls them when the language and project style allow it.
- Group related helpers together by purpose.
- A file should read like a newspaper: the headline first, supporting detail below, related material grouped together.

### Use vertical spacing to show concepts


- Readers scan structure before reading details.
- Spacing should reveal conceptual grouping.

- Use blank lines to separate distinct ideas.
- Keep tightly related lines visually dense.
- Avoid both wall-of-text formatting and arbitrary spacing.

### Keep declarations near use


- Early declarations add cognitive baggage.
- The reader carries names in memory before they become useful.

- Move local variables into the narrowest reasonable scope.
- Introduce a name when the reader is ready to use it.
- Every variable introduced too early becomes mental baggage until the reader finally discovers why it exists.

### Keep shared class properties in one predictable place


- Class properties are shared resources, not local details.
- Scattering them makes class-wide dependencies harder to see.

- Group class properties where the team expects to find them.
- Optimize for class-level discoverability, not local convenience.

### Let the team standardize the rest

Apply this to braces, quotes, indentation, and similar style conventions.

- Many style decisions are team conventions rather than universal truths.
- Consistency removes distraction.

- Preserve coherent local conventions.
- Standardize only when inconsistency harms readability.

## Decision guide: refactor before commenting

When code is hard to understand, try these in order:

1. Rename the thing.
2. Extract the thing.
3. Introduce a named constant or helper.
4. Reorganize the function or file.
5. Add a comment only if the essential information still cannot be expressed clearly in code.

## Final check

Before finishing, verify that:

- names match behavior,
- the code was changed only where the change earned its keep,
- functions have focused responsibilities,
- abstraction levels are not mixed carelessly,
- side effects are visible,
- comments are accurate and necessary,
- performance-sensitive or framework-specific constraints were respected,
- duplication has been reduced where practical,
- and formatting supports reading instead of distracting from it.

## Release notes

### 0.1.3

- kept the full coverage, but stripped out rubric-like scaffolding that did not sound like the source material
- kept the didactic force from the transcripts without flattening the skill into a short checklist

### 0.1.2

- brought back more of the explanatory voice from the source transcripts
- kept the text direct, but restored the didactic parts that help an agent judge code instead of just applying rules

### 0.1.1

- tightened the wording to be more direct and less generic
- kept the same guidance, but stripped out fluff and repeated prompt-like phrasing

### 0.1.0

- Initial public release.
- Built from a transcript-derived clean-code guidance set focused on code intent, naming honesty, explicit side effects, top-down readability, and refactoring restraint.
- Hardened through iterative evaluation, including transcript-aware grading and adversarial checks for over-refactoring, framework idioms, performance-sensitive code, public API design, and test readability.
