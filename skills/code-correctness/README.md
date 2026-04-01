# code-correctness

clean code is not about making code look expensive. it is about making code easier to trust.

`code-correctness` is now organized as a modular skill tree.

- the top-level `SKILL.md` is the orchestrator
- the theme folders contain lesson modules
- each lesson module is grounded in one reel from the `s4.codes` series
- the old monolithic skill is preserved as a backup reference in `references/monolithic-backup.md`

## architecture

the skill tree is organized by theme:

- `meaningful-names/`
- `functions/`
- `comments/`
- `formatting/`
- `objects-and-data/`

the active entrypoint is `SKILL.md`. it tells the model to read the full lesson tree, then explain reviews and refactors using the intention of the most relevant lessons.

## what it helps with

- confusing names
- long or mixed-responsibility functions
- hidden side effects
- stale, noisy, or misleading comments
- file structure and readability issues
- weak abstraction boundaries
- code that is technically fine but annoying to read or risky to change

## what it does differently

- it grounds advice in the actual lesson curriculum instead of generic cleanup language
- it treats the lessons as a complementary system, not isolated tips
- it keeps restraint as a valid outcome
- it respects framework idioms and hot paths
- it keeps the old monolithic version as a fallback instead of throwing it away

## attribution

the modular lesson tree is based on the public `s4.codes` clean-code reel series.

- source account: `s4.codes`
- this skill uses the saved reel descriptions and transcriptions as the lesson source material
- the lesson tree should keep being updated as more videos from the series come out

thanks to `s4.codes` for publishing the series and making the principles concrete enough to turn into a usable skill system.

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
- clean up these comments
- this file is messy
- should this be a class
- refactor without changing behavior
- make this easier to maintain

## files

- `SKILL.md` - main orchestrator skill
- `meaningful-names/*/SKILL.md` - lesson modules for naming
- `functions/*/SKILL.md` - lesson modules for function design
- `comments/*/SKILL.md` - lesson modules for comments
- `formatting/*/SKILL.md` - lesson modules for file and layout structure
- `objects-and-data/*/SKILL.md` - lesson modules for abstraction and design choices
- `references/monolithic-backup.md` - previous all-in-one version kept as backup
- `README.md` - overview and install notes

## open source

yes, this is open source.

- license: `GPL-3.0`
- repo: `https://github.com/koikbr/code-correctness-skill`

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- email: oi@koik.com.br

## author

koi
