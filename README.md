<!--
@license
Copyright (c) ggsuite

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# dna_vscode

The DNA layer for the editor: shared VS Code settings and recommended
extensions.

## Guides

- `dna/doc/en/guides/vscode-guide.md` — what is configured, how to add a
  setting or extension, and how a consumer overrides one

Every file of `dna/doc/en/` also ships in German and Farsi, with the
same name:

- `dna/doc/de/guides/`
- `dna/doc/fa/guides/`

## Skills

- `/vscode` — compares installed extensions against the recommendations
  and checks that `settings.json` parses

## Configuration

- `dna/dot-vscode/settings.json` — formatting, rulers, coverage gutters,
  the license header template
- `dna/dot-vscode/extensions.json` — the recommended extensions

## Publishing

`dna/dot-pubignore` keeps `/doc/*/guides/` out of the package `dart pub publish`
uploads. helix joins it with the ignore files of the other layers into the
project's `.pubignore`.

## Layers

Orthogonal: this layer carries only its own topic and is combined with
other layers by the consuming repo.

## Variables

- `dnaCopyrightHolder` — the name in the license header of every file
- `dnaCompany` — the author name the license header template inserts

They name the organization, so they are never inherited: every layer or
project listing this one sets them in its own `dna/_vars.json`.
`helix init` and `helix add` ask for the missing ones.

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
