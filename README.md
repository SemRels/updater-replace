# updater-replace

[![Latest Release](https://img.shields.io/github/v/release/SemRels/updater-replace?label=version&color=blue)](https://github.com/SemRels/updater-replace/releases/latest)
[![CI](https://github.com/SemRels/updater-replace/actions/workflows/ci.yml/badge.svg)](https://github.com/SemRels/updater-replace/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/SemRels/updater-replace)](LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/SemRels/updater-replace/badge)](https://scorecard.dev/viewer/?uri=github.com/SemRels/updater-replace)
[![REUSE status](https://api.reuse.software/badge/github.com/SemRels/updater-replace)](https://api.reuse.software/info/github.com/SemRels/updater-replace)

Generic pre-tag updater for semrel. It replaces regular expressions or literal
text in arbitrary project files, validates expected match counts before writing,
and preserves file modes and existing CRLF line endings.

## Configuration

Use the plugin during `pre-tag` so the changed files are available to the
release commit:

```yaml
plugins:
  - uses: updater-replace
    phase: pre-tag
    args:
      files: README.md,docs/**/*.md
      ignore: docs/archive/**
      from: 'version-\d+\.\d+\.\d+'
      to: 'version-${nextRelease.version}'
      countMatches: "true"
```

The flat form configures one rule. `files` and `ignore` accept comma,
semicolon, or newline-separated patterns.

| Argument | Environment variable | Description |
|---|---|---|
| `files` | `SEMREL_PLUGIN_FILES` | Required file patterns, including recursive `**` globs |
| `ignore` | `SEMREL_PLUGIN_IGNORE` | Exclusion patterns |
| `from` | `SEMREL_PLUGIN_FROM` | Required Go regular expression |
| `to` | `SEMREL_PLUGIN_TO` | Replacement string and template |
| `literal` | `SEMREL_PLUGIN_LITERAL` | Treat `from` as literal text |
| `flags` | `SEMREL_PLUGIN_FLAGS` | Go regexp flags: `i`, `m`, `s`, and `U` |
| `countMatches` | `SEMREL_PLUGIN_COUNT_MATCHES` | Include match counts in the per-file summary |
| `replacements` | `SEMREL_PLUGIN_REPLACEMENTS` | YAML or JSON array for multiple rules |

Because semrel passes each `args` value through one environment variable, use a
YAML block scalar for structured rules:

```yaml
plugins:
  - uses: updater-replace
    phase: pre-tag
    args:
      replacements: |
        - files: ["README.md", "docs/**/*.md"]
          ignore: ["docs/archive/**"]
          from: 'version-\d+\.\d+\.\d+'
          to: 'version-${nextRelease.version}'
          countMatches: true
          results:
            - file: README.md
              hasChanged: true
              numMatches: 1
              numReplacements: 1
        - files: Dockerfile
          from: '^ARG VERSION=.*$'
          flags: m
          to: 'ARG VERSION=${nextRelease.version}'
```

`files`, `ignore`, and `from` accept either a string or a list. Multiple `from`
patterns run in order. Replacement strings use Go regexp expansion, so `$1` and
`${name}` capture references are supported.

## Templates

The `to` value supports:

| Template | Source |
|---|---|
| `${nextRelease.version}` | `SEMREL_VERSION` / `SEMREL_NEXT_VERSION` |
| `${lastRelease.version}` | `SEMREL_CURRENT_VERSION` |
| `${gitTag}` / `${tag}` | `SEMREL_TAG_NAME` |
| `${channel}` / `${branch}` | `SEMREL_BRANCH` |
| `${date}` | Current UTC date (`YYYY-MM-DD`) |
| `${SEMREL_VERSION}` and other listed `SEMREL_*` names | Corresponding release environment variable |

## Examples

README badge:

```yaml
from: 'version-\d+\.\d+\.\d+'
to: 'version-${nextRelease.version}'
```

Dockerfile:

```yaml
from: '^ARG VERSION=.*$'
flags: m
to: 'ARG VERSION=${nextRelease.version}'
```

Source constant:

```yaml
from: 'const Version = "[^"]+"'
to: 'const Version = "${nextRelease.version}"'
```

## Assertions and atomic writes

Every rule is evaluated in memory first. All `results` assertions must pass
before any file is written, preventing partial updates when a regular expression
drifts. Writes use a temporary file in the target directory followed by an
atomic rename and retain the original permission bits.

In a semrel dry run, the plugin performs matching, templating, and assertion
validation, reports files it would update, and writes nothing.

The plugin prints modified paths to stdout, one per line, and diagnostics to
stderr. A missing file match, invalid pattern, failed assertion, or write error
exits non-zero.

## Local development

```bash
go test ./...
go build ./cmd/plugin
```

## License

Apache-2.0
