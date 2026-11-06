# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal collection of custom [Fabric](https://github.com/danielmiessler/fabric) patterns. It holds Markdown prompts only, so there is no build, lint, or test tooling. Each top-level directory is one pattern, and its `system.md` is the prompt Fabric sends to the model.

## Trying a pattern

Fabric reads patterns from `~/.config/fabric/patterns/`, not from this repo. A pattern has to be symlinked or copied there before it can run, and a copied pattern won't pick up later edits until it is copied again.

```bash
ln -s "$PWD/note_youtube" ~/.config/fabric/patterns/note_youtube   # install (stays in sync)
fabric -y "https://youtube.com/watch?v=..." --pattern note_youtube  # run on a YouTube transcript
```

The only way to check a prompt change is to run the pattern and read its output. This calls the configured model.

## Pattern conventions

- **Directory name:** use lowercase with underscores. The name is also the Fabric pattern name, and the README's `.bashrc` snippet turns it into a shell function, so it must be a valid function name. That snippet writes titled output to the Obsidian vault as `DD-MM-YYYY-title.md`.
- **`system.md` sections, in order:** `# IDENTITY and PURPOSE`, `# STEPS`, `# OUTPUT STRUCTURE`, `# OUTPUT INSTRUCTIONS`, separated by `---` and ending with a line telling the model to follow all the instructions. `note_youtube` also has a leading `# CONFIGURATION` block.
- **`user.md`:** optional short human-readable description, useful when sharing a pattern or contributing it upstream.
- **New patterns:** add a row to the Patterns table in `README.md`.

## Git workflow

Work directly on `main`: when asked to commit or push, commit and push there, with no feature branch or pull request.

## `note_youtube` details

- `OUTPUT_LANGUAGE` at the top of `system.md` sets the note language and is currently `Finnish`. It translates the section headers too, but quotes stay in their original language.
- Output must be Markdown only, with no commentary around it, because it is saved directly as an Obsidian note. The YAML frontmatter must always be present.
