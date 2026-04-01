# code-correctness

clean code is not decoration. it is how code tells the truth.

`code-correctness` is organized as a modular skill tree.

## what it is

this skill contains:

- the top-level `SKILL.md` orchestrator
- a full lesson tree organized by theme
- one lesson module per reel from the saved `s4.codes` series
- a monolithic backup reference for broad tasks

the active entrypoint is `SKILL.md`. it tells the model to read the full lesson tree, then explain reviews and refactors using the intention of the most relevant lessons.

## lesson themes

- `meaningful-names/`
- `functions/`
- `comments/`
- `formatting/`
- `objects-and-data/`

## what it helps with

- confusing or misleading names
- long or mixed-responsibility functions
- hidden side effects
- stale, noisy, or misleading comments
- file structure and readability issues
- weak abstraction boundaries
- behavior-preserving refactors grounded in explicit lessons

## what it does differently

- it grounds advice in the actual lesson curriculum instead of generic cleanup language
- it treats the lessons as a complementary system, not isolated tips
- it keeps restraint as a valid outcome
- it respects framework idioms and hot paths
- it keeps the old monolithic version as a fallback instead of throwing it away

## acknowledgements

the modular lesson tree is based on the public `s4.codes` clean-code reel series.

- instagram: `https://instagram.com/s4.codes`
- the saved reel descriptions and transcriptions are used as lesson source material
- the tree is intended to keep growing as more videos in the series come out

thanks to `s4.codes` for publishing the series and making the principles concrete enough to turn into a reusable skill system.

## install

```bash
npx skills add koikbr/code-correctness-skill
```

list the available skill before installing:

```bash
npx skills add koikbr/code-correctness-skill --list
```

install only `code-correctness`:

```bash
npx skills add koikbr/code-correctness-skill --skill code-correctness
```

## files

- `SKILL.md` - main orchestrator skill
- `meaningful-names/*/SKILL.md` - lesson modules for naming
- `functions/*/SKILL.md` - lesson modules for function design
- `comments/*/SKILL.md` - lesson modules for comments
- `formatting/*/SKILL.md` - lesson modules for file and layout structure
- `objects-and-data/*/SKILL.md` - lesson modules for abstraction and design choices
- `references/monolithic-backup.md` - previous all-in-one version kept as backup

## open source

yes, this skill is open source.

- license: `GPL-3.0`
- repository: `https://github.com/koikbr/code-correctness-skill`

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- email: oi@koik.com.br
