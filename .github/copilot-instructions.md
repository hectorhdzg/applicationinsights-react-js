# Repository instructions

- Dependency installation must work with both the public npm registry and Microsoft's `1ds-sdk-js` Azure Artifacts feed.
- Do not assume that the newest version allowed by a dependency range is available from the Azure Artifacts feed. Preserve known-compatible lockfile versions when a transitive range resolves to a version missing from the feed.
- Pin feed-constrained transitive packages by declaring them as exact root `devDependencies` (in addition to `overrides`). An `overrides` entry alone is not recorded in `package-lock.json` and is silently ignored by the npm 10.x client used in Microsoft builds.
- Pin the whole dependency chain, not just the failing package. A pin cannot apply if a parent's range excludes it (for example, `browserslist` requiring `electron-to-chromium ^1.5.420`).
- Keep public npm registry URLs in `package-lock.json` so external contributors can install dependencies without Microsoft credentials. If a lockfile is regenerated through an Azure Artifacts registry, rewrite `resolved` URLs back to `https://registry.npmjs.org/` and keep `sha512` integrity values, since the feed returns `sha1` hashes.
- Before accepting a dependency update, verify that the selected package versions and tarballs are available from the `1ds-sdk-js` feed used by Microsoft builds. Metadata alone is not sufficient: a version can be listed while its tarball 404s. Verify with a clean npm cache (`npm pack <pkg>@<version> --cache <empty-dir>`), because a warm local cache hides these failures.
