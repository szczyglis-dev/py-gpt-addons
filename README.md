# Official PyGPT Add-ons repository

**Last updated:** 2026-10-01

This is the official add-ons repository and public catalog for [PyGPT](https://github.com/szczyglis-dev/py-gpt). It contains the registries used by PyGPT to discover **official** and **third-party/community** add-ons, together with additional installable resources such as Agent Skills and MCP Connectors.

The repository is intended to be a central place for discovering, reviewing, and publishing reusable PyGPT add-ons. Add-ons can live directly in this repository or in their own GitHub repositories and be referenced from the appropriate catalog. 🚀

## Catalogs

The repository maintains the following public registries:

- [`addons.json`](./addons.json) - PyGPT add-ons such as plugins, providers, tools, themes, locale packs, vector stores, data loaders, and other supported add-on types.
- [`mcp.json`](./mcp.json) - MCP Connector definitions that can be browsed and imported from PyGPT.
- [`skills.json`](./skills.json) - Agent Skills available through the PyGPT Skills catalog.

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

External add-ons are application-wide and are installed under the application base workdir's `addons` directory. They are shared by all profiles using that PyGPT installation and are designed to work with both source installations and compiled PyGPT builds.

> [!WARNING]
> Add-ons execute with the same permissions as PyGPT. `trusted` and `official` are not a security sandbox or a guarantee that code is harmless. Public registry entries are content-pinned with SHA-256, and trusted entries must pass registry/manifest/content verification, which protects against silent upstream replacement after review. Always review third-party source code, dependencies, repository ownership, and requested capabilities before installing.

## Contributing

Community Add-on source code should stay in the author's own GitHub repository. Submit only the catalog link/metadata to this repository; do not vendor the third-party Add-on code here. Every public `addons.json` entry must include a deterministic `sha256` content pin, and the same digest must be committed in the Add-on's upstream `manifest.json`. Any later Add-on update requires a new digest and a new registry PR.

See **[CONTRIBUTING.md](./CONTRIBUTING.md)** for the complete PR workflow, SHA-256 generation commands, registry examples, update rules, and security-review checklist.

Agent Skills and MCP Connectors use their respective registry formats; the Add-on SHA-256 rules described above apply to `addons.json`.

## Examples

The [`examples`](./examples) directory contains example add-on packages and reference implementations. Start there when creating a new add-on or when you want to verify the expected repository layout and manifest structure.

## Documentation

Complete documentation for creating, packaging, installing, and publishing PyGPT add-ons is available here:

**[PyGPT documentation - Extending PyGPT](https://pygpt.readthedocs.io/en/latest/extending.html)**

**[PyGPT documentation - Add-ons API](https://pygpt.readthedocs.io/en/latest/addons_api.html)**

It covers the add-on manifest, all supported add-on types, Python entry points, plugins and providers, themes, locale packs, GitHub/monorepo layouts, registry entries, profile portability, and custom launcher registration.

Additional PyGPT links:

- **PyGPT:** https://github.com/szczyglis-dev/py-gpt
- **Website:** https://pygpt.net
- **Documentation:** https://pygpt.readthedocs.io
- **Discord:** https://pygpt.net/discord

## License and third-party content

Each contributed third-party add-on, Skill, or MCP Connector may have its own license and terms. Check the linked project before installing or redistributing it. Contributions to this catalog should only reference content you are permitted to publish and distribute.
