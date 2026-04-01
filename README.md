# code-correctness-skill

clean code is not decoration. it is how code tells the truth.

this repository publishes the modular `code-correctness` skill for the open agent skills ecosystem.

## what changed

the skill is now organized as a modular lesson tree:

- the top-level `skills/code-correctness/SKILL.md` is the orchestrator
- the theme folders contain lesson modules
- each lesson module is grounded in one reel from the saved `s4.codes` series
- the previous monolithic skill is preserved as `references/monolithic-backup.md`

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

## attribution

the modular lesson tree is based on the public `s4.codes` clean-code reel series.

- the saved reel descriptions and transcriptions are used as lesson source material
- the tree should keep being updated as more videos in the series are released

thanks to `s4.codes` for publishing the series and making the principles concrete enough to turn into a usable skill system.

## install

install directly from GitHub:

```bash
npx skills add koikbr/code-correctness-skill
```

list the skill before installing:

```bash
npx skills add koikbr/code-correctness-skill --list
```

install only this skill:

```bash
npx skills add koikbr/code-correctness-skill --skill code-correctness
```

## open source

yes. this repository is open source under `GPL-3.0`.

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- support: oi@koik.com.br
