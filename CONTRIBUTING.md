# Contributing to the PyGPT Add-ons registry

This repository is a public **catalog**, not a mirror of third-party Add-on source code. Community Add-ons should remain in the author's own GitHub repository. Pull requests to this repository should add or update the corresponding entry in `addons.json` and should not copy the Add-on's Python/assets into this repository.

The only source code kept here is project-maintained material such as the examples already present under `examples/`.

## Before opening a PR

Your Add-on repository must:

- be publicly accessible on GitHub;
- contain a valid `manifest.json` at the Add-on root;
- use a stable, globally unique manifest `id`;
- use one of the Add-on types supported by PyGPT;
- declare the exact release `version` and `min_app_version`;
- document external dependencies and security-sensitive behavior;
- contain a `sha256` field generated from the exact Add-on tree being submitted;
- publish that exact tree under an immutable Git release ref, preferably a version tag such as `v1.0.0` (an exact commit SHA is also acceptable). Do not submit `main`, `master`, or another moving branch as the public registry `ref`.

A monorepo is supported. In that case, point `github_path` to the directory containing the Add-on's `manifest.json`.

## Immutable release refs

Every public Add-on version must remain downloadable exactly as it was reviewed. Before opening a registry PR:

1. finish the release content and version in `manifest.json`;
2. generate/write the final `sha256`;
3. commit the exact release tree in your own repository;
4. create and push an immutable version tag for that commit, for example `v1.0.0`;
5. use that tag as the registry `ref`.

Do **not** use `main`, `master`, or another moving branch as the `ref` of a public registry entry. A branch can change while the registry still contains the SHA-256 of the previously reviewed version. In that situation PyGPT correctly rejects the changed tree, but users can no longer install the currently approved release until the next registry PR is accepted. A stable release tag prevents that availability gap.

Treat every published tag as immutable: never force-move, overwrite or delete a tag referenced by an accepted registry entry. Development can continue normally on your default branch immediately after tagging. For the next Add-on release, create a new tag (for example `v1.1.0`), calculate the new SHA-256 and open a new registry PR.

An exact full commit SHA may be used instead of a version tag when needed, but version tags are preferred because they are easier to understand and correspond naturally to the Add-on `version`.

For a package in a repository subdirectory, the equivalent GitHub browser/tree URL is, for example:

```text
https://github.com/user/repo/tree/v1.0.0/plugin
```

In `addons.json`, prefer the structured fields instead of embedding the tag into `github_url`:

```json
{
  "github_url": "https://github.com/user/repo",
  "github_path": "plugin",
  "ref": "v1.0.0"
}
```

## Content SHA-256 pinning

Every Add-on published in the public `addons.json` registry must be pinned with a deterministic PyGPT Add-on SHA-256. The same digest must appear in two places:

1. the Add-on's own `manifest.json` as `sha256`;
2. the registry entry in `addons.json` as `sha256`.

For an entry marked `trusted: true`, PyGPT requires the manifest digest and registry digest and refuses installation if either is missing, malformed, different from the other, or different from the downloaded Add-on contents.

This is content pinning, not a cryptographic author signature. It prevents a repository owner from changing the code behind an already-reviewed registry entry without a corresponding registry update being reviewed and merged.

### Generate the digest

Use the helper shipped in the main PyGPT repository. It implements the same algorithm as the installer.

Linux/macOS:

```sh
./bin/addon-sha256.sh /path/to/your/addon --write
```

Windows:

```bat
bin\addon-sha256.bat C:\path\to\your\addon --write
```

`--write` stores the calculated value in `manifest.json` and prints it. Running the command again without `--write` verifies what the digest should be:

```sh
./bin/addon-sha256.sh /path/to/your/addon
```

Example manifest fragment:

```json
{
  "manifest_version": 1,
  "id": "githubuser_project_my_plugin",
  "version": "1.2.0",
  "type": "plugin",
  "sha256": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"
}
```

Copy exactly the same value into the registry entry:

```json
{
  "id": "githubuser_project_my_plugin",
  "version": "1.2.0",
  "type": "plugin",
  "github_url": "https://github.com/githubuser/project",
  "github_path": "pygpt/my_plugin",
  "ref": "v1.2.0",
  "sha256": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "trusted": false,
  "official": false
}
```

The SHA-256 covers the complete Add-on tree recursively. Relative paths are normalized to `/` and sorted deterministically. File contents are hashed byte-for-byte. `manifest.json` is included too, but it is serialized canonically with only its top-level `sha256` field removed before hashing; this avoids a circular self-hash while still protecting the manifest's ID, version, entry point, dependency declarations and all other fields. `.git` metadata, directory timestamps, permissions and empty directories are not part of the digest. Symlinks are rejected.

Because file bytes are part of the digest, generate the value from the exact content you intend to publish. Avoid local line-ending transformations that differ from the content committed to GitHub.

## Adding a new Add-on

Open a PR that changes `addons.json` only (plus registry/documentation corrections when explicitly needed). Do **not** vendor/copy your Add-on source into this repository.

A typical community entry contains:

```json
{
  "id": "githubuser_project_my_plugin",
  "name": "My Plugin",
  "description": "Short description.",
  "author": "Author name",
  "version": "1.0.0",
  "type": "plugin",
  "github_url": "https://github.com/githubuser/project",
  "github_path": "pygpt/my_plugin",
  "ref": "v1.0.0",
  "sha256": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "trusted": false,
  "official": false
}
```

In the PR description, include a concise security/review note covering any filesystem access, network access, shell/command execution, desktop control, credential/API-key use, native extensions, and external Python dependencies.

`trusted` and `official` are review/ownership metadata. Do not assume that listing in this repository automatically makes a community Add-on trusted or official.

## Updating an existing Add-on

**Every upstream Add-on update requires a new PR to this registry.** This is intentional.

When any file inside the published Add-on tree changes:

1. update the Add-on version;
2. regenerate `sha256` and commit the new `manifest.json` to the Add-on's own repository;
3. create and push a **new immutable version tag** for that exact commit (for example `v1.1.0`);
4. open a new PR here updating at least the registry `version`, `sha256`, and `ref`;
5. wait for the registry PR to be reviewed and merged.

Until the new registry PR is accepted, the existing registry entry continues to point to the previous immutable tag, so users can still install the currently approved version with its matching SHA-256.

Do not reuse an old SHA-256 for changed content, do not reuse/move an existing release tag for new content, and do not delete release tags referenced by accepted registry entries.

## PR checklist

- [ ] The Add-on code lives in the author's own GitHub repository; this PR does not vendor it here.
- [ ] `manifest.json` is valid and its `id`, `type`, `version` and source path match the registry entry.
- [ ] `manifest.json` contains the generated `sha256`.
- [ ] `addons.json` contains the same `sha256`.
- [ ] The hash was generated from the exact committed Add-on contents being submitted.
- [ ] `ref` points to an immutable version tag (preferred) or exact commit SHA, not `main`/`master`/another moving branch.
- [ ] The referenced release tag has been pushed and will not be moved or deleted.
- [ ] Dependencies and security-sensitive capabilities are documented.
- [ ] This is a new PR for this exact Add-on revision/update.
