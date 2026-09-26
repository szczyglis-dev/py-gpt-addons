# Official PyGPT Add-ons repository

**Last updated:** 2026-09-25

This is the official add-ons repository and public catalog for [PyGPT](https://github.com/szczyglis-dev/py-gpt). It contains the registries used by PyGPT to discover **official** and **third-party/community** add-ons, together with additional installable resources such as Agent Skills and MCP Connectors.

The repository is intended to be a central place for discovering, reviewing, and publishing reusable PyGPT add-ons. Add-ons can live directly in this repository or in their own GitHub repositories and be referenced from the appropriate catalog. 🚀

## Catalogs

The repository maintains the following public registries:

- [`addons.json`](./addons.json) — PyGPT add-ons such as plugins, providers, tools, themes, locale packs, vector stores, data loaders, and other supported add-on types.
- [`mcp.json`](./mcp.json) — MCP Connector definitions that can be browsed and imported from PyGPT.
- [`skills.json`](./skills.json) — Agent Skills available through the PyGPT Skills catalog.

PyGPT can browse these catalogs from its built-in add-on, connector, and skill managers.

## Supported add-on types

The external add-on system currently supports these package types:

| Type | Purpose |
| --- | --- |
| `plugin` | Adds model-callable commands/tools, integrations, event hooks, or other plugin functionality. |
| `llm` | Adds an LLM/provider wrapper compatible with PyGPT's LLM provider interface. |
| `vector_store` | Adds a custom vector-store backend for indexing and RAG. |
| `loader` | Adds a custom data loader for files, web content, or other data sources. |
| `audio_input` | Adds a speech/audio input provider. |
| `audio_output` | Adds a speech/audio output (TTS) provider. |
| `web` | Adds a web/search provider. |
| `tool` | Adds a GUI/application tool to PyGPT. |
| `agent` | Adds an agent/provider implementation. |
| `theme` | Adds a PyGPT theme using CSS/QSS theme assets (`app.css`, `app.xml`, `chat.css`). |
| `locale` | Adds one or more PyGPT translation/locale packs. |

The same repository also publishes **Agent Skills** through `skills.json` and **MCP Connectors** through `mcp.json`. These are separate catalogs from `addons.json`, but are distributed from the same official repository.

## Add-on packages

Installable external add-ons use a `manifest.json` file that identifies the add-on and its type. Python-based add-ons declare an entry point and are loaded into the same runtime registries as built-in PyGPT components. Theme and locale packages use their normal PyGPT asset formats.

External add-ons are profile-scoped and are installed under `%workdir%/addons`. They are designed to work with both source installations and compiled PyGPT builds.

> [!WARNING]
> Add-ons execute with the same permissions as PyGPT. Registry fields such as `trusted` and `official` are metadata, not a security sandbox. Always review third-party source code, dependencies, repository ownership, and requested capabilities before installing.

## Contributing

You can publish your own add-on, Agent Skill, or MCP Connector from your GitHub repository by opening a pull request against the `master` branch of this repository.

Add an entry with a link to your GitHub project to the appropriate registry:

- **PyGPT add-on:** `addons.json`
- **MCP Connector:** `mcp.json`
- **Agent Skill:** `skills.json`

For a PyGPT add-on, make sure the linked add-on root contains a valid `manifest.json`, uses a unique ID, and declares a supported add-on type. A repository containing multiple add-ons may reference a specific subdirectory.

To avoid collisions with built-in add-ons and add-ons from other authors, published add-on IDs should follow a GitHub-derived naming convention: `<github_user>_<repo_name>_<addon_name>`. Normalize characters to the manifest-safe form (lowercase letters, digits, `.`, `_`, and `-`). For example: `szczyglis_dev_py_gpt_example_plugin`. Keep the same ID stable across future releases of the add-on.

Please include enough information for review, especially:

- add-on/resource name and short description;
- type;
- author/maintainer;
- GitHub repository URL and optional subdirectory/path;
- version or Git ref when applicable;
- dependency requirements;
- security-sensitive capabilities such as filesystem, network, shell/command execution, desktop control, or credential access.

After review and acceptance, the entry will become part of the official catalog and will be visible to PyGPT users in the corresponding **Explore/Browse** view. ✅

### Registry ownership and metadata

Being listed in the official repository does not automatically mean a third-party project is maintained by the PyGPT project. The catalog distinguishes official and third-party entries with metadata. Authors remain responsible for their repositories, releases, dependencies, licenses, and maintenance unless explicitly stated otherwise.

## Examples

The [`examples`](./examples) directory contains example add-on packages and reference implementations. Start there when creating a new add-on or when you want to verify the expected repository layout and manifest structure.

## Documentation

Complete documentation for creating, packaging, installing, and publishing PyGPT add-ons is available here:

**[PyGPT documentation — Extending PyGPT](https://pygpt.readthedocs.io/en/latest/extending.html)**

It covers the add-on manifest, all supported add-on types, Python entry points, plugins and providers, themes, locale packs, GitHub/monorepo layouts, registry entries, profile portability, and custom launcher registration.

Additional PyGPT links:

- **PyGPT:** https://github.com/szczyglis-dev/py-gpt
- **Website:** https://pygpt.net
- **Documentation:** https://pygpt.readthedocs.io
- **Discord:** https://pygpt.net/discord

## License and third-party content

Each contributed third-party add-on, Skill, or MCP Connector may have its own license and terms. Check the linked project before installing or redistributing it. Contributions to this catalog should only reference content you are permitted to publish and distribute.

## Changelog

- **2026-09-25** - initial launch.
