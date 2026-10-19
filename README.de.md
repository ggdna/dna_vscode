<!--
@license
Copyright (c) ggsuite

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# dna_vscode

[English](README.md) | Deutsch

Gibt einem Repo die gemeinsamen VS-Code-Einstellungen, die
Prettier-Konfiguration und die empfohlenen Erweiterungen.

## Motivation

Jedes Repo der Familie soll mit derselben Editor-Einrichtung geöffnet und
gleich formatiert werden: Lineal bei 80 Zeichen, Formatieren beim Speichern,
Coverage in der Randleiste und derselbe Lizenzkopf in jeder neuen Datei. Ohne
gemeinsame Schicht führt jedes Repo seine eigene Kopie dieser Einstellungen,
und die Kopien laufen auseinander.

Zwei Details geraten von Hand leicht falsch, deshalb legt die Schicht sie
einmal fest. Prettier in VS Code ist an die Konfiguration des Repos gebunden;
sonst fällt es auf seine mitgelieferten Standardwerte zurück und schreibt
doppelte Anführungszeichen in YAML, obwohl `.prettierrc` einfache verlangt. Und
Prettier lässt die Dateien in Ruhe, die die DNA erzeugt; ein Formatierer, der
sie umschriebe, ließe jeden Testlauf sie sichern und erneut überschreiben.

## Verwendung

Als Dev-Dependency eintragen und einmal initialisieren:

```bash
pnpm add -D @ggdna/dna-vscode   # TypeScript projects
dart pub add dev:dna_vscode     # Dart projects
helix init
```

`helix init` legt einen Test ab, der die DNA bei jedem Testlauf instanziiert
und prüft. Er kopiert die Einstellungen und die Erweiterungsliste nach
`.vscode/`, die Prettier-Dateien nach `.prettierrc` und `.prettierignore` im
Wurzelverzeichnis des Repos, den Guide nach `doc/guides/` und den Skill nach
`.claude/skills/vscode/`.

Öffne das Repo in VS Code und installiere die empfohlenen Erweiterungen, die es
anbietet. Tippe `/vscode` in Claude Code, um die installierten Erweiterungen mit
den Empfehlungen abgleichen und `settings.json` prüfen zu lassen; der Skill
listet zuerst seine Befunde und installiert oder korrigiert nur, was du
bestätigst.

## Guides

- `dna/doc/guides/vscode-guide.md` — was eingestellt ist, wie man eine
  Einstellung oder eine empfohlene Erweiterung ergänzt und wie ein nutzendes
  Repo die Einstellungen überschreibt

## Skills

- `/vscode` — meldet jede empfohlene Erweiterung, die nicht installiert ist,
  und prüft, dass `.vscode/settings.json` als JSONC lesbar ist und jeder
  `editor.defaultFormatter` auf eine installierte Erweiterung zeigt; folgt dem
  VS-Code-Guide

## Konfiguration

- `dna/dot-vscode/settings.json` — Formatieren beim Speichern über Prettier,
  Lineale bei 80 Zeichen, Leerzeichen am Zeilenende und abschließender
  Zeilenumbruch, das Test-Panel und die Coverage-Randleiste, die
  Lizenzkopf-Vorlage der Erweiterung psi-header, und `dna/_dna.json` wird als
  JSONC geöffnet
- `dna/dot-vscode/extensions.json` — die empfohlenen Erweiterungen, darunter
  Prettier, markdownlint, GitLens, Coverage Gutters und psi-header
- `dna/dot-prettierrc` — die Prettier-Optionen: einfache Anführungszeichen,
  Semikolons, nachgestellte Kommas, eine Zeilenbreite von 80
- `dna/dot-prettierignore` — was Prettier auslässt: Build-Ausgaben, Lockfiles,
  `CHANGELOG.md`, Dart-, JavaScript- und TypeScript-Dateien und jede Datei, die
  die DNA erzeugt (`dna/`, `doc/guides/`, `doc/templates/`, `.vscode/`,
  `.claude/`, `scripts/`)

## Umfang

Die Schicht richtet nur den Editor und Prettier ein. Sie installiert weder
VS Code noch die Erweiterungen oder Prettier selbst und überlässt die Dart- und
TypeScript-Einstellungen, die Launch-Konfigurationen und die Tasks den
Sprach-Schichten wie dna_dart und dna-ts und dem Repo. Ein nutzendes Repo passt
die Einstellungen mit einer `settings.overrides.json` im eigenen
`dna/dot-vscode/` an, wie der Guide es beschreibt.

## Schichten

Die Schicht ist orthogonal: `dna/_dna.json` nennt keine Eltern-Schichten. Sie
trägt nur ihr eigenes Thema und wird vom nutzenden Repo mit anderen Schichten
kombiniert.

## Variablen

- `dnaCopyrightHolder` — der Rechteinhaber im Copyright-Kopf jeder abgelegten
  Datei und in der Lizenzkopf-Vorlage
- `dnaCompany` — der Autorname, den die Erweiterung psi-header einsetzt

Beide nennen die Organisation. `mustOverride` in `dna/_dna.json` zwingt jedes
Repo und jede Schicht, die diese Schicht einbindet, sie in der eigenen
`dna/_vars.json` zu setzen; `helix init` und `helix add` fragen nach den
fehlenden.

## Entwicklung

Der Ordner `dna/` ist handgeschriebene Quelle und wird nie generiert. Das Repo
instanziiert seine eigene DNA — nach Änderungen `dart test` laufen lassen;
vorher committen, denn eine Datei, die die DNA überschreiben würde, darf keine
uncommittete Arbeit enthalten.

Bearbeite die Dateien unter `dna/`, nie ihre Kopien in `.vscode/`,
`.prettierrc`, `.prettierignore`, `doc/` und `.claude/`: Der Test überschreibt
sie.

## Lizenz

MIT — siehe [LICENSE](LICENSE).
