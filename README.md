# Fluidpill plugins

The catalog the Fluidpill manager's **Store** reads. It is one file,
[`index.json`](index.json), listing plugins. Each plugin is a zip published
somewhere else, usually as a GitHub release asset, with its SHA-256.

Fluidpill reads this file from:

```
https://raw.githubusercontent.com/unikqqe/fluidpill-plugins/main/index.json
```

## What an entry looks like

```json
{
  "id": "claude",
  "name": "Claude",
  "version": "2.0.0",
  "author": "Fluidpill",
  "description": "What Claude Code is doing in each session: the task, the current tool, and when it needs you or is done.",
  "accent": "#d97757",
  "icon": "icons/claude.png",
  "uses": ["setup", "alerts", "sound", "other_apps"],
  "download": "https://github.com/unikqqe/dyniland/releases/download/plugin-claude-v2.0.0/claude-2.0.0.zip",
  "sha256": "…64 hex digits…",
  "publisher_key": "p256:…",
  "min_fluidpill": "0.1.0"
}
```

| Key | Required | Meaning |
|---|---|---|
| `id` | yes | The plugin id from its `plugin.json`. The zip must hold this plugin. |
| `name` | yes | Shown in the Store. |
| `version` | yes | Semver. The zip's `plugin.json` must say the same version. |
| `author`, `description` | no | Shown in the Store and on the install sheet. |
| `accent` | no | `#rrggbb`. Colours the plugin's tile. |
| `icon` | no | A PNG of at most 64 KiB: an https link, or a path relative to this file (`icons/x.png`). |
| `uses` | no | What the plugin uses, shown on the install sheet. Copy it from the entry `pack` writes. |
| `download` | yes | Where the zip is (https). |
| `sha256` | yes | The zip's SHA-256. Fluidpill refuses a download that does not match, before it extracts anything. |
| `publisher_key` | no | The key the package is signed with. When set, Fluidpill checks `plugin.sig` with this key, and the plugin's `plugin.json` must declare the same key. |
| `min_fluidpill` | no | The oldest Fluidpill it runs on (semver). Older ones show it but do not offer it. |

A bad entry is left out of the Store, not the whole catalog. Keys Fluidpill doesn't know yet are ignored.

## Releasing a new version of a plugin

1. Build the package with `fluidpill-plugin pack <folder> --key <key file> --download <url of the zip>`. It writes `<id>-<version>.zip` and `<id>-<version>.catalog.json`.
2. Publish the zip at that URL (a GitHub release asset, for example).
3. Replace the plugin's entry in `index.json` with the contents of `<id>-<version>.catalog.json`.

The Claude plugin is built by the `Claude plugin` workflow in the Fluidpill repository. Pushing a `plugin-claude-v<version>` tag attaches both files to the release.

Installed plugins update from the Store's **Update** button. An update runs the plugin's setup step again, so the Claude hooks always point at the new files.

## Checks

[`.github/workflows/check.yml`](.github/workflows/check.yml) runs on every pull request. For each entry it checks the required keys, then downloads the zip and compares its SHA-256.

See [CONTRIBUTING.md](CONTRIBUTING.md) to add a plugin.
