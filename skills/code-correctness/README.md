# code-correctness

clean code is not decoration. it is how code tells the truth.

`code-correctness` is an agent skill for refactoring and reviewing existing code with more judgment than ceremony. it pushes an agent toward clearer naming, smaller and more honest functions, explicit side effects, better top-down flow, and stronger restraint when the best move is to leave good code alone.

this skill was built from a transcript-derived clean-code guidance set, then tightened through iterative evals and adversarial checks focused on the thing most ai code still lacks: engineering judgment.

## what it does

- improves code readability without changing behavior unless change is clearly needed
- fixes misleading names, mixed responsibilities, hidden side effects, and weak structure
- preserves important domain comments when they carry real context
- respects framework idioms, hot paths, public contracts, and test readability
- avoids mechanical cleanup, wrapper explosion, and polish-only churn
- knows when a zero-diff result is the right result

## why this exists

most ai refactors can make code look cleaner while making it less truthful.

they over-abstract, flatten framework patterns, delete comments that still matter, or rename things into something louder but less accurate.

this skill exists to push the agent toward code intent first:

- the right name, not the fanciest one
- the right helper, not the maximum number of helpers
- the right amount of change, not change for its own sake

## install

### manual

copy or symlink this folder into your skills directory:

```bash
mkdir -p ~/.agents/skills
cp -r code-correctness ~/.agents/skills/
```

for Claude Code, the common location is:

```bash
mkdir -p ~/.claude/skills
cp -r code-correctness ~/.claude/skills/
```

### packaged `.skill`

a packaged release artifact is also available as `code-correctness.skill`.

## when it should trigger

use this skill when the task is about improving existing code quality, especially when the user says things like:

- “this function is too long”
- “these names are confusing”
- “this code is hard to follow”
- “please clean this up”
- “refactor without changing behavior”
- “make this easier to maintain”

## what makes it different

this skill is intentionally judgment-heavy.

it was tightened not only against normal refactor fixtures, but also against adversarial cases where many models get worse when trying to be helpful:

- already-good code where restraint should win
- comments that carry protocol history or operational context
- performance-sensitive loops
- framework-specific idioms that look odd for good reasons
- test files where clarity matters more than clever deduplication
- api redesign cases where boldness is allowed and timidity is the real bug

## evaluation approach

the skill was iterated with:

- transcript-aware grading based on the actual clean-code principles behind the source material
- with-skill vs baseline comparisons across multiple iterations
- adversarial evals designed to catch over-refactoring and weak engineering judgment

the goal was not to reward syntactic cleanliness alone, but to reward code that becomes more truthful, more readable, and safer to change.

## files

- `SKILL.md` — the skill itself
- `README.md` — public-facing overview and installation notes

## support

- website: https://koik.com.br
- github: https://github.com/koikbr
- support: oi@koik.com.br

## license

GNU GPL v3.0 — see `SKILL.md` frontmatter for the declared license metadata.

## author

koi
