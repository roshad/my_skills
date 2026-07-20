# Tauri GitHub Release Checks

Read this reference when `tauri.conf.json`, `tauri.conf.*`, `src-tauri`, `tauri-action`, or Tauri updater configuration is present.

## Version Sources

Common authoritative sources include:

- frontend `package.json` and lockfile;
- `src-tauri/Cargo.toml` and `Cargo.lock` when it records the application package version;
- `src-tauri/tauri.conf.json` or equivalent configuration;
- workspace manifests and platform-specific bundle metadata.

Use the repository's synchronization command when safe. Verify every source after it runs.

## Publication Path

Inspect `.github/workflows` before publishing. A common pattern is:

```text
push vX.Y.Z tag
  -> GitHub Actions builds platform bundles
  -> tauri-action creates or updates the GitHub Release
  -> updater manifest and signatures are uploaded
```

When this pattern exists, let the workflow own Release creation. After it completes, edit the existing Release body with curated notes instead of creating another Release.

## Secrets and Signing

Verify only secret presence/status through safe workflow evidence. Never print signing keys, passwords, tokens, or decoded values.

Common requirements include updater private key material, key password, and `GITHUB_TOKEN` content permission.

## Expected Artifacts

Derive exact expectations from bundle targets and workflow configuration. Common assets include:

- platform installer or application bundle;
- updater archive/package;
- `latest.json` or platform updater manifest;
- one or more `.sig` signature files.

Verify with GitHub Release metadata after the workflow completes. For updater-enabled releases, confirm that the configured updater endpoint can resolve the manifest and that manifest asset URLs correspond to the same release version.

Accept only complete SemVer release tags even when the workflow trigger uses a broad pattern such as `v*`. For manually dispatched workflows, verify that the workflow checks out or otherwise resolves the requested tag instead of building an unrelated branch revision.

## Common Failure Modes

- Release exists with placeholder text: edit only the title/body.
- Installer exists but `latest.json` is missing: inspect updater artifact and signing configuration.
- Manifest exists but signatures are missing: inspect signing steps and secrets.
- Tag version differs from configuration: stop; do not publish inconsistent updater metadata.
- Workflow was manually dispatched with a tag that does not exist: verify the workflow contract before continuing.
- Local publishing command and tag-triggered workflow both create Releases: choose one owner and avoid duplicates.
