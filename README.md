# code-correctness-skill

clean code is not decoration. it is how code tells the truth.

this repository publishes the modular `code-correctness` skill for the open agent skills ecosystem.

## what it is

this project contains:

- the top-level `code-correctness` orchestrator skill
- a full lesson tree organized by theme
- one lesson module per reel from the saved `s4.codes` series
- a monolithic backup reference for broad tasks

the skill is designed to review and refactor code by grounding the reasoning in explicit lesson intentions instead of generic clean-code slogans.

## lesson themes

- `meaningful-names/`
- `functions/`
- `comments/`
- `formatting/`
- `objects-and-data/`

## repository layout

```text
skills/
  code-correctness/
    SKILL.md
    README.md
    meaningful-names/
    functions/
    comments/
    formatting/
    objects-and-data/
    references/
```

## what it helps with

- confusing or misleading names
- long or mixed-responsibility functions
- hidden side effects
- stale, noisy, or misleading comments
- file structure and readability issues
- weak abstraction boundaries
- behavior-preserving refactors grounded in explicit lessons

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

## open source

yes, this project is open source.

- license: `GPL-3.0`
- repository: `https://github.com/koikbr/code-correctness-skill`

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- email: oi@koik.com.br
