# code-correctness-skill

clean code is not decoration. it is how code tells the truth.

this repository publishes the `code-correctness` skill for the open agent skills ecosystem.

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

## repository layout

```text
skills/
  code-correctness/
    SKILL.md
    README.md
```

the `skills/` layout is used so tools like `npx skills` and skills.sh can discover the skill more consistently.

## current skill

- `code-correctness` - judgment-heavy code review and refactoring guidance focused on truthful naming, explicit side effects, clean function boundaries, restraint, and real engineering tradeoffs

## support

- website: https://koik.com.br
- github: https://github.com/koikbr/code-correctness-skill
- support: oi@koik.com.br

## license

GNU GPL v3.0. see `LICENSE` and `skills/code-correctness/SKILL.md`.
