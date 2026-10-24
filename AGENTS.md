# Repository instructions

> Human guide: [README.md](README.md) · [Documentation index](docs/README.md)

## Commit messages

Follow the [Conventional Commits 1.0.0 specification](https://www.conventionalcommits.org/en/v1.0.0/) for all commit messages. Use the format `<type>[optional scope]: <description>`, with an optional body and footer separated by blank lines. Use `feat` for new features and `fix` for bug fixes. Mark breaking changes with `!` before the colon or a `BREAKING CHANGE: <description>` footer. Other types, such as `docs`, `chore`, `ci`, and `refactor`, are allowed.

## Releases

`.github/workflows/release.yml` runs [release-please](https://github.com/googleapis/release-please) on every push to `main`. From the Conventional Commits merged since the last tag, it keeps one release pull request open, titled `chore: release X.Y.Z`:

- `fix`, `perf` and `revert` make a patch release, `feat` a minor release, and a breaking change (`!`) a major release. The other types release nothing on their own and stay out of the changelog.
- It writes the version to `version.txt` and adds the release to `CHANGELOG.md`.
- Merging it tags `vX.Y.Z` and publishes the GitHub release.
- GitHub runs no workflow on that pull request, because the workflow's own token opened it. The other workflows run on `main` once it merges.

Never change the version or `CHANGELOG.md` by hand. `.release-please-manifest.json` holds the last released version, and `release-please-config.json` the settings. To force a version, add a `Release-As: X.Y.Z` footer to a commit.
