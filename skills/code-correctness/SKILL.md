---
name: code-correctness
description: Apply clean-code and correctness-oriented refactors to existing code. Use this skill whenever the user wants a code review or refactor focused on readability, maintainability, naming, comments, function design, duplication, hidden side effects, flag arguments, command-query separation, abstraction level, file organization, or clean-code smells. Trigger even when the user does not say "clean code" and instead says things like "this function is too long," "these names are confusing," "this code is hard to follow," "please clean this up," "make this easier to maintain," or "refactor without changing behavior."
license: GPL-3.0
metadata:
  authors: "koi"
  version: "0.1.0"
  website: "https://koik.com.br"
  github: "https://github.com/koikbr/code-correctness-skill"
  support: "mailto:oi@koik.com.br"
---

# Code Correctness

This skill is for changing or reviewing code using correctness principles drawn from clean-code practice.

Correctness is not only "the program runs." Code is also incorrect when it misleads the next reader, hides behavior behind vague names, mixes responsibilities, spreads duplication, or invites unsafe edits.

Your job when using this skill is to make the code more truthful, more local, more searchable, and easier to change without breaking intent.

## Working stance

- Start by identifying the code's real job before proposing edits.
- Prefer the smallest refactor that makes intent obvious.
- Preserve behavior unless the user asked for behavioral change or the current behavior is clearly misleading or broken.
- Preserve external contracts unless it is clearly safe to change them. Be especially careful with exported names, public method names, return shapes, and persisted field names.
- Do not confuse interface stability with local naming conservatism. Parameter names, local variables, and private helpers should usually be improved aggressively when that does not change the observable contract.
- When a public API is awkward but must remain stable, introduce clearer internal entry points and keep the old API as a thin compatibility wrapper when that materially improves the design.
- Do not refactor just because a refactor is possible. If the code is already clear, compact, and honest, prefer restraint.
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

These rules exist to keep the skill from becoming mechanically "clean" instead of genuinely useful.

### Prefer restraint when the code is already good

Apply this when the code is already clear, compact, and locally honest.

Why this matters:

- A needless refactor creates churn without adding understanding.
- Over-editing is a correctness risk because every change has blast radius.

What to do:

- Leave already-good code mostly alone.
- Fix only the genuinely weak point if one exists.
- A zero-diff result is valid when the code is already clear, idiomatic, and honest.
- Do not make polish-only changes such as ceremonial type aliases, annotation churn, or renames that add no real clarity.
- Do not invent helpers, wrappers, or abstractions just to look active.
- Do not rename semantically precise identifiers into more dramatic but less accurate names.

### Respect domain comments that carry real knowledge

Apply this when comments encode protocol quirks, regulatory rules, production incidents, performance constraints, or other non-obvious why.

Why this matters:

- Some knowledge cannot be recovered from the code alone.
- Deleting those comments can erase the reason the code is safe.

What to do:

- Keep comments that explain external constraints or surprising historical facts that still matter.
- Rewrite those comments only to make them clearer or more precise.
- Remove only the comments that restate mechanics or add noise.

### Respect performance-sensitive paths

Apply this when the code runs in a hot path, tight loop, parser, serializer, ranking pass, rendering-critical path, or similar performance-sensitive context.

Why this matters:

- Some readability patterns add allocations, callbacks, indirection, or repeated work that are harmless elsewhere but inappropriate here.
- A pretty refactor can still be a bad engineering decision.

What to do:

- Improve naming and local clarity first.
- Keep direct loops and compact logic when they are the right tradeoff.
- Do not replace simple hot-path code with abstraction-heavy style unless the gain clearly outweighs the cost.

### Respect framework idioms and semantic patterns

Apply this when code looks unusual because the framework, runtime, or library expects that structure.

Why this matters:

- Some code is non-obvious for semantic reasons, not because it is sloppy.
- Flattening those patterns into "cleaner" code can silently break behavior.

What to do:

- Understand why the pattern exists before changing it.
- Preserve stale-closure guards, lifecycle ordering, hook semantics, transactional boundaries, or other framework-specific constraints when they are intentional.
- If the idiom is already clear, prefer no change over cosmetic cleanup.
- Improve naming and surrounding structure without erasing the semantic reason for the pattern.
- In framework state/ref code, keep names aligned with the actual time or ownership semantics. Prefer `latest`, `current`, or `previous` when those are the literal meanings.

### Treat tests as readability-first specifications

Apply this when working on unit tests, integration tests, fixtures, or assertions.

Why this matters:

- Tests are read to understand behavior, not just to remove duplication.
- Repetition in tests is often acceptable when it keeps each case obvious.

What to do:

- Prefer explicit test intent over clever abstraction.
- Deduplicate only when duplication is genuinely hiding the scenario.
- Do not turn simple tests into loops, helpers, or meta-structures that obscure what each case proves.

### Be bolder when redesign is explicitly allowed

Apply this when the user invites redesign, API cleanup, or stronger behavioral restructuring rather than mere compatibility-preserving cleanup.

Why this matters:

- Over-learning "preserve the interface" can turn into timidity.
- Sometimes the cleanest and most honest fix is to redesign the awkward surface itself.

What to do:

- If the prompt explicitly allows interface change, evaluate the public design itself instead of assuming it is fixed.
- Remove flag-driven or mixed-responsibility APIs when redesign is clearly the better outcome.
- Do not keep a bad surface forever just because earlier evals trained caution.

## Naming rules

### Use intention-revealing names

Apply this when a reader has to inspect surrounding logic to understand a name.

Why this matters:

- Code is read far more often than it is written.
- A weak name makes every future read slower.
- A name that hides intent forces the reader to reconstruct meaning from context.

What to do:

- Replace vague names such as `data`, `value`, `item`, `d`, `res`, or `tmp` with names that reveal business purpose.
- Prefer names that explain what the value means in this code, not what type it happens to be.
- Prefer the most semantically exact name, not the most elaborate-sounding one.
- If a comment exists only to explain a name, improve the name first.

### Make meaningful distinctions

Apply this when names sound different but do not help the reader distinguish responsibilities.

Why this matters:

- Different names imply different roles.
- Generic suffixes create fake distinctions and increase hesitation.

What to do:

- Avoid filler names like `Manager`, `Handler`, `Processor`, `Info`, or numbered variants unless the distinction is real and obvious.
- Rename items according to their specific responsibility.
- Make behavioral differences visible in the names themselves.

### Use pronounceable names

Apply this when names are compact but awkward to say.

Why this matters:

- Code is discussed aloud in reviews and pairing.
- Unpronounceable names are harder to remember and harder to coordinate around.

What to do:

- Prefer real words over compressed abbreviations.
- Rename identifiers that cannot be said naturally in conversation.

### Use searchable names

Apply this when an identifier has broad scope or long-term importance.

Why this matters:

- Single letters and raw numbers are difficult to search meaningfully.
- Searchability is part of maintainability.

What to do:

- Reserve single letters for tiny, obvious local scopes.
- Give constants, fields, helpers, and reused values names that can be found quickly across the codebase.
- Replace magic numbers with named constants when the value carries domain meaning.

### Avoid encodings

Apply this when names include type or storage hints that tooling already provides.

Why this matters:

- Type encodings spend name-space on information the IDE already knows.
- That pushes out the more important information: purpose.

What to do:

- Remove Hungarian notation or similar prefixes when they only restate type.
- Keep the name focused on intent and role.

### Avoid mental mapping

Apply this when the code makes the reader translate abbreviations repeatedly.

Why this matters:

- Every translation consumes attention.
- The reader should spend effort on behavior, not decoding vocabulary.

What to do:

- Replace cryptic abbreviations with domain words.
- Make formulas, predicates, and loops readable without a translation table.

## Function rules

### Keep functions small

Apply this when the main idea is buried inside mechanics.

Why this matters:

- Long functions hide intent under detail.
- The reader has to separate story from implementation by hand.

What to do:

- Extract dense blocks, nested loops, and low-level mechanics into helpers with names that expose intent.
- Let the top-level function read as a short sequence of meaningful steps.

### Make each function do one thing

Apply this when a function can be split into clearly different sections.

Why this matters:

- Multiple responsibilities create multiple reasons to change.
- Mixed tasks make testing and modification riskier.

What to do:

- If you can name internal sections differently, split them.
- If an extracted helper would get a meaningful name, that responsibility likely deserves its own function.
- Keep the top-level function centered on one idea.

### Keep one level of abstraction per function

Apply this when business intent and low-level implementation are mixed together.

Why this matters:

- Mixed abstraction obscures flow.
- The reader gets dragged between policy and syntax.

What to do:

- Keep orchestration code high-level.
- Push implementation details into subordinate helpers.
- Aim for top-down reading where each function leads naturally to the next depth level.

### Bury repeated switches

Apply this when type-based branching repeats across several places.

Why this matters:

- Repeated switches spread change risk.
- Adding one new case means editing many sites.

What to do:

- Consolidate creation or dispatch in one place.
- Prefer polymorphism or behavior objects when repeated branching is part of the design.

### Keep argument count low

Apply this when a function call carries too much mental weight.

Why this matters:

- Extra parameters create ordering confusion and increase misuse risk.
- Long parameter lists often indicate a missing abstraction.

What to do:

- Prefer zero, one, or two arguments when practical.
- When arguments pile up, look for a missing object or concept.
- Group related data under a meaningful name.

### Avoid flag arguments

Apply this when a boolean argument changes which job the function performs.

Why this matters:

- Flag arguments often mean one function is doing two jobs.
- Call sites become less readable because `true` or `false` rarely explains itself.

What to do:

- Split flag-driven branches into separate functions when they represent different behaviors.
- Keep boolean data only when it is actual domain data, not a mode selector.
- Avoid replacing one unclear flag with a combinatorial explosion of wrapper functions. Prefer a cleaner core function plus explicit orchestration where needed.

### Avoid output arguments

Apply this when a function mutates an input parameter to communicate a result.

Why this matters:

- Output arguments hide data flow.
- They violate the common expectation that data comes in through arguments and leaves through return values.

What to do:

- Return values instead of mutating caller-owned output containers.
- Make data movement visible at the call site.

### Preserve interface stability while refactoring internals

Apply this when a cleaner internal design would require changing public names or call shapes.

Why this matters:

- Many readability fixes are internal and do not require breaking external callers.
- Renaming a public function or changing returned field names can create accidental behavior changes even when the internals improve.

What to do:

- First improve internals without changing the public contract.
- Only rename exported or externally consumed interfaces when it is clearly safe or explicitly requested.
- Prefer local variable renames, parameter renames, helper extraction, and internal restructuring before API churn.
- If the prompt explicitly allows redesign, reconsider whether preserving the old interface is still the right goal.

### Be careful with pairs and triads

Apply this when multiple arguments are difficult to distinguish or order correctly.

Why this matters:

- Some pairs are natural, but unrelated pairs and most triads create confusion.
- Readers must remember both meaning and ordering.

What to do:

- Keep only natural pairs as peers.
- Turn unrelated arguments into an object or move one concept onto the owning object.
- Use names that make order obvious when order matters.

### Eliminate hidden side effects

Apply this when a function name sounds observational but the function mutates state.

Why this matters:

- Hidden side effects create surprise bugs.
- Callers trust names; misleading names create unsafe assumptions.

What to do:

- Rename the function if mutation is its true job.
- Prefer splitting the check from the mutation.
- Make side effects explicit in structure and naming.

### Separate commands from queries

Apply this when one function both answers a question and changes state.

Why this matters:

- A question should be safe to ask.
- Mixing commands and queries makes calls harder to reason about and reuse.

What to do:

- Use one function to return information and another to perform the mutation.
- Keep query-like names read-only.
- Keep command-like names obviously mutating.
- If an existing public function mixes both and cannot be safely removed, keep it as a compatibility wrapper over clearer internal command/query operations.

### Prefer exceptions over error codes when appropriate

Apply this when normal flow is overwhelmed by repeated status checking.

Why this matters:

- Error codes spread branching across every call site.
- Shared status enums couple many files together.

What to do:

- Let the success path read as straight-line logic.
- Handle exceptional flow in dedicated error handling paths.
- Keep happy-path code and error-handling code clearly separated.

### Remove duplication

Apply this when the same knowledge appears in more than one place.

Why this matters:

- Duplication multiplies maintenance work.
- One missed update creates inconsistent behavior.

What to do:

- Extract shared request logic, calculations, policy rules, and validation.
- Keep one authoritative representation of each rule or idea.

### Rewrite toward clarity

Apply this after code is working but still messy.

Why this matters:

- First drafts are usually organized around getting the code to run.
- Clean structure often appears only after refactoring.

What to do:

- Use tests as a safety net.
- Improve names, split functions, remove duplication, and simplify the flow.
- Treat refactoring as part of finishing the work.

## Comment and documentation rules

### Start from skepticism

Default assumption: most comments should not exist.

Why this matters:

- Code evolves faster than comments.
- A stale comment is worse than no comment because it misleads confidently.

What to do:

- First try to remove the need for the comment by renaming, extracting, or restructuring.
- Keep comments when the code cannot carry the important information alone, especially for external constraints, surprising tradeoffs, or protocol history that still matters.

### Use comments for why, not what

Apply this when the important information is a constraint or rationale rather than a mechanic.

Why this matters:

- Code usually shows how.
- Comments are most valuable when they explain business rules, tradeoffs, or external constraints.

What to do:

- Comment business reasons, legal constraints, performance tradeoffs, and surprising decisions.
- Do not restate code that already reads clearly.

### Use warnings, amplification, and TODOs deliberately

Apply this when future maintainers are likely to make a well-intentioned but wrong change.

Why this matters:

- Some lines look unnecessary until context is known.
- Without a warning, later cleanup may silently break behavior.

What to do:

- Add warnings where an obvious-looking simplification would be dangerous.
- Add amplification where a small line has disproportionate impact.
- Keep TODOs only when there is a real blocker, dependency, or deferred external fix.

### Document public APIs for external readers

Apply this when the code is a library or public interface used by people who will not read the implementation.

Why this matters:

- Internal code and public APIs have different audiences.
- External users need usage guidance, not implementation archaeology.

What to do:

- Document what it does, inputs, outputs, failure modes, and a small example.
- Keep legal headers standard and minimal when required.

### Remove noisy comments

Apply this when a comment only labels or repeats obvious code.

Why this matters:

- Noise comments double scan time without adding meaning.
- Too much noise trains readers to ignore all comments.

What to do:

- Delete comments that merely restate the code.
- Delete mandatory-but-empty doc comments.
- Interrupt the reader only when there is information the code cannot deliver.

### Remove misleading comments

Apply this when a comment is vague, incomplete, or refers to logic too loosely.

Why this matters:

- A misleading comment creates false confidence.
- Readers stop checking the code because they think the comment already explained it.

What to do:

- Keep comments specific, local, and easy to verify against nearby code.
- Delete or rewrite comments that require guessing.
- Do not document behavior that is actually controlled somewhere else unless the dependency is explicit and stable.

### Delete dead history from source files

Apply this when source files contain commented-out code, author tags, or inline changelogs.

Why this matters:

- Version control already keeps history better.
- Inline history clutters present-day reasoning.

What to do:

- Remove commented-out code.
- Remove attribution headers and manual change logs from source files.
- Trust version control for recovery and history.

### Do not use comments as structural crutches

Apply this when comments exist mainly to navigate oversized functions or files.

Why this matters:

- Section markers and closing-brace comments usually signal structure problems, not documentation needs.

What to do:

- Shorten the function.
- Split the file by responsibility.
- Use comments as the last fix, not the first one.

### Keep comments plain and readable

Apply this when comments are filled with personal shorthand, irrelevant stories, or formatting noise.

Why this matters:

- Comments should be readable in raw source form.
- If the comment itself is hard to read, it has failed.

What to do:

- Use plain text.
- Remove vague notes and irrelevant history.
- Rewrite or delete comments that are hard to parse quickly.

## File and formatting rules

### Organize files like a newspaper

Apply this when file order makes readers jump around to follow the main flow.

Why this matters:

- The reader should meet the highest-level idea first.
- Predictable ordering lowers navigation cost.

What to do:

- Put top-level orchestration near the top of the file.
- Define helpers below the code that calls them when the language and project style allow it.
- Group related helpers together by purpose.

### Use vertical spacing to show concepts

Apply this when code is visually flat or randomly spaced.

Why this matters:

- Readers scan structure before reading details.
- Spacing should reveal conceptual grouping.

What to do:

- Use blank lines to separate distinct ideas.
- Keep tightly related lines visually dense.
- Avoid both wall-of-text formatting and arbitrary spacing.

### Keep declarations near use

Apply this when local variables are declared long before they matter.

Why this matters:

- Early declarations add cognitive baggage.
- The reader carries names in memory before they become useful.

What to do:

- Move local variables into the narrowest reasonable scope.
- Introduce a name when the reader is ready to use it.

### Keep shared class properties in one predictable place

Apply this when shared fields are scattered near individual methods.

Why this matters:

- Class properties are shared resources, not local details.
- Scattering them makes class-wide dependencies harder to see.

What to do:

- Group class properties where the team expects to find them.
- Optimize for class-level discoverability, not local convenience.

### Let the team standardize the rest

Apply this to braces, quotes, indentation, and similar style conventions.

Why this matters:

- Many style decisions are team conventions rather than universal truths.
- Consistency removes distraction.

What to do:

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

### 0.1.0

- Initial public release.
- Built from a transcript-derived clean-code guidance set focused on code intent, naming honesty, explicit side effects, top-down readability, and refactoring restraint.
- Hardened through iterative evaluation, including transcript-aware grading and adversarial checks for over-refactoring, framework idioms, performance-sensitive code, public API design, and test readability.
