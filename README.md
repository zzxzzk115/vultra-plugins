# Vultra Plugins

Central catalog for Vultra plugins.

The root `plugins.json` file is the machine-readable plugin list. Editors and command-line tooling
fetch it directly, enumerate `plugins[]`, then install a selected entry (and version) from its
`source` descriptor.

Default catalog URL:

```text
https://raw.githubusercontent.com/zzxzzk115/vultra-plugins/main/plugins.json
```

## Catalog schema (version 2)

Each plugin entry describes the plugin once and lists every published version:

```jsonc
{
  "schemaVersion": 2,
  "kind": "vultra.pluginCatalog",
  "plugins": [
    {
      "id": "com.example.plugin",        // unique plugin id (matches the manifest id)
      "name": "Example",
      "author": "...",
      "description": "...",
      "repository": "https://github.com/...",
      "readme": "https://github.com/.../README.md",
      "capabilities": ["native", "lua"],
      "platforms": ["windows"],          // empty/absent = all platforms

      // Latest release, kept for schema-v1 readers:
      "version": "0.2.0",
      "source": { "type": "git", "url": "https://github.com/....git", "ref": "v0.2.0" },

      // All published versions, newest first. Editors use this for
      // version selection, updates, and rollbacks:
      "versions": [
        {
          "version": "0.2.0",
          "notes": "What changed in this release.",   // optional
          "source": { "type": "git", "url": "https://github.com/....git", "ref": "v0.2.0" }
        },
        {
          "version": "0.1.0",
          "source": { "type": "git", "url": "https://github.com/....git", "ref": "v0.1.0" }
        }
      ]
    }
  ]
}
```

Conventions:

- `versions` is ordered newest-first; `versions[0]` is the latest release and must match the
  top-level `version`/`source` pair.
- Each `source.ref` is an immutable git tag (`vX.Y.Z`) in the plugin repository.
- Publishing a new plugin version = push the tag in the plugin repository, then prepend a
  `versions[]` entry here and update the top-level `version`/`source`.
