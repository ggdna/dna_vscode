# dna_vscode

The DNA layer for the editor: shared VS Code settings and recommended
extensions.

## Guides

- `dna/doc/guides/vscode-guide.md` — what is configured, how to add a
  setting or extension, and how a consumer overrides one

## Skills

- `/vscode` — compares installed extensions against the recommendations
  and checks that `settings.json` parses

## Configuration

- `dna/dot-vscode/settings.json` — formatting, rulers, coverage gutters,
  the license header template
- `dna/dot-vscode/extensions.json` — the recommended extensions

## Layers

Orthogonal: this layer carries only its own topic and is combined with
other layers by the consuming repo.

## Variables

- `dnaCopyrightHolder` — the name in the license header of every file
- `dnaCompany` — the author name the license header template inserts

## Usage

Declare it as a dev-dependency and initialize once:

```bash
pnpm add -D @ggdna/dna-vscode   # TypeScript projects
dart pub add dev:dna_vscode     # Dart projects
helix init
```

The placed test instantiates and verifies the DNA on every test run.

## Development

The `dna/` folder is hand-authored source and is never generated. The repo
instantiates its own DNA — run `dart test` after changes; commit first, a
file the DNA would overwrite must not carry uncommitted work.
