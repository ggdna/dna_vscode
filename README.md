<!--
@license
Copyright (c) ggsuite

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# dna_vscode

English | [Deutsch](README.de.md)

Gives a repo the shared VS Code settings, the Prettier configuration and the
recommended extensions.

## Motivation

Every repo of the family should open with the same editor setup and be
formatted the same way: 80 character rulers, format on save, coverage in the
gutter and the same license header in every new file. Without a shared layer
each repo keeps its own copy of these settings, and the copies drift.

Two details are easy to get wrong by hand, so the layer settles them once.
Prettier in VS Code is pinned to the repo's configuration; otherwise it falls
back to its bundled defaults and writes double quotes into YAML although
`.prettierrc` asks for single ones. And Prettier leaves the files the DNA
generates alone; a formatter that rewrote them would make every test run back
them up and overwrite them again.

## Usage

Declare it as a dev-dependency and initialize once:

```bash
pnpm add -D @ggdna/dna-vscode   # TypeScript projects
dart pub add dev:dna_vscode     # Dart projects
helix init
```

`helix init` places a test that instantiates and verifies the DNA on every
test run. It copies the settings and the extension list to `.vscode/`, the
Prettier files to `.prettierrc` and `.prettierignore` in the repo root, the
guide to `doc/guides/` and the skill to `.claude/skills/vscode/`.

Open the repo in VS Code and install the recommended extensions it offers.
Type `/vscode` in Claude Code to have the installed extensions compared with
the recommendations and `settings.json` checked; the skill lists its findings
first and installs or fixes only what you confirm.

## Guides

- `dna/doc/guides/vscode-guide.md` — what is configured, how to add a setting
  or a recommended extension, and how a consuming repo overrides the settings

## Skills

- `/vscode` — reports every recommended extension that is not installed and
  checks that `.vscode/settings.json` parses as JSONC and that each
  `editor.defaultFormatter` points at an installed extension; follows the
  VS Code guide

## Configuration

- `dna/dot-vscode/settings.json` — format on save through Prettier, rulers at
  80 characters, trailing whitespace and final newline, the testing panel and
  coverage gutters, the license header template of the psi-header extension,
  and `dna/_dna.json` opened as JSONC
- `dna/dot-vscode/extensions.json` — the recommended extensions, among them
  Prettier, markdownlint, GitLens, Coverage Gutters and psi-header
- `dna/dot-prettierrc` — the Prettier options: single quotes, semicolons,
  trailing commas, a print width of 80
- `dna/dot-prettierignore` — what Prettier skips: build output, lockfiles,
  `CHANGELOG.md`, Dart, JavaScript and TypeScript files, and every file the DNA
  generates (`dna/`, `doc/guides/`, `doc/templates/`, `.vscode/`, `.claude/`,
  `scripts/`)

## Scope

The layer only configures the editor and Prettier. It does not install VS Code,
the extensions or Prettier itself, and it leaves the Dart and TypeScript
settings, the launch configurations and the tasks to the language layers such
as dna_dart and dna-ts and to the repo. A consuming repo adapts the settings
with a `settings.overrides.json` in its own `dna/dot-vscode/`, as the guide
describes.

## Layers

The layer is orthogonal: `dna/_dna.json` lists no parent layers. It carries
only its own topic and is combined with other layers by the consuming repo.

## Variables

- `dnaCopyrightHolder` — the holder named in the copyright header of every
  placed file and in the license header template
- `dnaCompany` — the author name the psi-header extension inserts

Both name the organization. `mustOverride` in `dna/_dna.json` forces every repo
or layer that lists this layer to set them in its own `dna/_vars.json`;
`helix init` and `helix add` ask for the missing ones.

## Development

The `dna/` folder is hand-authored source and is never generated. The repo
instantiates its own DNA — run `dart test` after changes; commit first, a file
the DNA would overwrite must not carry uncommitted work.

Edit the files below `dna/`, never their copies in `.vscode/`, `.prettierrc`,
`.prettierignore`, `doc/` and `.claude/`: the test overwrites them.

## License

MIT — see [LICENSE](LICENSE).
