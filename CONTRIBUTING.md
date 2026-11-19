# Adding a plugin

Anyone can propose a plugin with a pull request that adds one entry to [`index.json`](index.json).

## Before you open the pull request

1. **Your plugin works from a zip.** `fluidpill-plugin validate <folder>` passes. Installing the zip with `fluidpill-plugin install <id>-<version>.zip` gives you a working plugin.
2. **It is versioned.** `version` in `plugin.json` is semver and goes up with every release.
3. **It is signed (recommended).**
   1. Run `fluidpill-plugin keygen <key file>` once and keep the key file private.
   2. Put the `"publisher_key"` line it prints into your `plugin.json`.
   3. Pack with `--key`. Only you can then publish updates that Fluidpill accepts.
4. **Pack it:**

   ```
   fluidpill-plugin pack <folder> --key <key file> --download https://…/<id>-<version>.zip
   ```

   Then publish the zip at exactly that address.
5. **Use the entry `pack` wrote** (`<id>-<version>.catalog.json`) as your entry in `index.json`. You may add `icon` (a PNG of at most 64 KiB, in `icons/` here or at an https address) and `min_fluidpill`.

## What reviewers look at

- **The entry.** The required keys are there, the `id` is not taken, and the checks pass.
- **What the plugin uses.** The `uses` list must match its manifest. A plugin that runs its own program, adds setup steps or changes other apps' settings gets a closer look at its source.
- **Where the source is.** Link it in the pull request. Plugins whose source cannot be read are not accepted.
- **That it does what its description says**, and nothing that would surprise someone who installed it from that description.

Installing a plugin trusts it: there is no further approval in Fluidpill. Please keep that trust.

## Updating your plugin

Open a pull request that replaces your entry with the new `<id>-<version>.catalog.json`. Keep the same `publisher_key`. A signed update with a different key is refused by Fluidpill.

## Removing a plugin

Open a pull request that removes the entry, or ask in an issue. Plugins already installed stay installed. They just stop being offered.
