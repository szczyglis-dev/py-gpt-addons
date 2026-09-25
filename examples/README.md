# External extensions examples

This directory mirrors the runtime `%workdir%/extensions` layout. Each child directory is a standalone extension with its own `manifest.json`. Import **one extension directory at a time** through **Config -> Install extension...**, or copy the type directory into the active profile's `extensions` directory and restart PyGPT.

Included type folders: `plugins`, `llms`, `vector_stores`, `loaders`, `audio_input`, `audio_output`, `web`, `tools`, `agents`, `themes`, and `locale`.

The Python examples intentionally implement only the minimum needed to prove startup discovery and normal runtime registration; they are not production providers. `manifest.example.json` shows the full manifest metadata/dependency syntax, and `extensions.registry.example.json` shows both an official-repository relative `path` entry and an external `github_url` + `github_path` entry.
