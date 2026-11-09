# hermes-lang-tr

**Turkish (Türkçe) language pack for Hermes — core, TUI and Desktop.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Hermes bundles Turkish for the CLI and messaging gateway (`locales/tr.yaml`) and
for the TUI (`locales/tr.tui.yaml`), but the Desktop app has no bundled Turkish
catalog. This pack ships all three surfaces: it fills the Desktop gap and
overrides the bundled Turkish wording where the community translation differs.

It is a text-only language pack: three YAML files, a `plugin.yaml`, and an
`__init__.py` whose `register()` does nothing (see below). The Hermes backend
serves the strings to Desktop over `i18n.catalog`, where they merge over the
bundled English catalog. Any string the pack does not cover stays in English.

## Requirements

- Hermes Agent 0.21.6 or later, for both the backend and the Desktop app.
  Language packs (`provides_locales`) arrived in hermes-agent #126296; older
  versions skip the pack.

## Install

### Option A — Hermes Desktop

Capabilities → Plugins → **Install from Git**, enter this repository URL, then
**Review repository** and **Install**. Then set the language (Option B's last
line, or the Desktop language switcher) and restart Desktop.

### Option B — `hermes plugins install`

```bash
hermes plugins install hermes-lang-tr          # from the plugin catalog
hermes plugins install <owner>/hermes-lang-tr  # directly from this repository
hermes config set display.language tr
```

Restart Desktop (and any running gateway or TUI) so it reloads the language
list. Turkish then appears as **Türkçe** in the Desktop language switcher.

Plugins are per profile. Install the pack in the profile your Desktop backend
runs, or pass `-p <profile>`.

### Option C — personal overlay, no plugin

Copy the files to `<HERMES_HOME>/locales/`: `tr.yaml`, `tr.tui.yaml` and
`tr.desktop.yaml`. A plugin pack takes precedence over the overlay when both
exist.

### Verify

```bash
hermes plugins list                  # hermes-lang-tr, enabled
hermes config get display.language   # tr
```

## Coverage

| Surface | File | Keys |
| --- | --- | --- |
| CLI, gateway replies, approval prompts | `locales/tr.yaml` | 3,535 |
| TUI | `locales/tr.tui.yaml` | 1,440 |
| Desktop | `locales/tr.desktop.yaml` | 5,569 |

Checked against the English catalogs of hermes-agent (`locales/en.yaml`,
`ui-tui/src/i18n/en.ts`, `apps/desktop/src/i18n/en.ts` and the Desktop key
export `locales/_keys.desktop.json`):

- 100% key coverage on all three surfaces — no missing keys, no keys that do
  not exist in English;
- placeholders preserved: named placeholders (`{name}`, `{count}`) on core
  strings, positional placeholders (`{0}`, `{1}`) on Desktop/TUI entries whose
  English value is a function;
- values are text only, as the pack format requires.

Coverage counts the main catalogs, not every visible string. Agent responses,
tool output, logs and the *names* of slash commands are not translated by any
language pack; tell the agent to answer in Turkish if you want that.

## What this plugin does and does not do

- No executable code: `__init__.py` holds only a `register()` that returns
  immediately. It exists because Desktop's *Install from Git* check requires it
  beside `plugin.yaml`; the backend registers `locales/` from the manifest
  before `register()` runs. No JavaScript, no network access, no shell
  commands, no background processes, no stored credentials.
- Registering the pack never changes `display.language`. You opt in with
  `hermes config set display.language tr`.
- The catalog contains static UI strings only — agent responses, tool output
  and logs are not affected, as with any Hermes language pack.

## Updating to a new Hermes version

The three YAML files are the source of truth and are edited directly. When the
English key set changes, align the pack with the target version:

```bash
hermes plugins validate .     # keys that do not exist in English are listed
```

Also compare English values with the previous audited revision: a key can stay
the same while its meaning changes.

## Translation and attribution

The Turkish strings are an independent community translation, prepared with AI
assistance from the English catalogs of NousResearch/hermes-agent (MIT
License). Corrections from Turkish users are welcome as issues or pull
requests. See [NOTICE](./NOTICE).

## License

[MIT](./LICENSE)
