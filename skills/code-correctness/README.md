# code-correctness

clean code is not about making code look expensive. it is about making code easier to trust.

`code-correctness` is a skill for reviewing and refactoring existing code without falling into the usual ai habits: overexplaining, over-abstracting, renaming things into nonsense, or touching code that was fine to begin with.

it pushes toward better names, cleaner function boundaries, visible side effects, good restraint, and code that reads like it means what it says.

## what it helps with

- confusing names
- long or mixed-responsibility functions
- hidden side effects
- stale or noisy comments
- flag-heavy apis
- code that is technically fine but annoying to read or risky to change

## what it does differently

- it treats restraint as a valid outcome
- it keeps comments that still carry real context
- it respects framework idioms and hot paths
- it does not force DRY where repetition is clearer
- it is tuned for code intent, not cleanup theater

## install

### direct from github

```bash
npx skills add koikbr/code-correctness-skill
```

list what the repo exposes:

```bash
npx skills add koikbr/code-correctness-skill --list
```

install only this skill explicitly:

```bash
npx skills add koikbr/code-correctness-skill --skill code-correctness
```

### manual

copy or symlink the folder into your local skills directory:

```bash
mkdir -p ~/.agents/skills
cp -r code-correctness ~/.agents/skills/
```

for Claude Code, a common location is:

```bash
mkdir -p ~/.claude/skills
cp -r code-correctness ~/.claude/skills/
```

## when to use it

use it when the task is about improving existing code, especially if the user says things like:

- this function is too long
- these names are confusing
- this code is hard to follow
- please clean this up
- refactor without changing behavior
- make this easier to maintain

## why it exists

a lot of ai refactors are almost right. that is the problem.

they make code look cleaner while making it less honest. they add helpers nobody needed, flatten framework patterns that were there for a reason, delete comments that still matter, or keep changing good code because silence feels wrong.

this skill was built to push in the other direction.

## files

- `SKILL.md` - the skill instructions
- `README.md` - overview and install notes

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- email: oi@koik.com.br

## license

GPL-3.0

## author

koi
