# chain-command-blocker (source)

Go source for the **chain-command-blocker** Claude Code plugin.

The plugin itself — plugin manifest, hook launcher, and prebuilt
binaries — lives in
[gurisugi/chain-command-blocker](https://github.com/gurisugi/chain-command-blocker).
That repo is what users install. This repo only produces the binaries
and publishes them to GitHub Releases via goreleaser.

## Layout

```
cmd/chain-command-blocker/  CLI entrypoint (the binary shipped by the plugin)
internal/                   config / permissions / settings / shell parsers
tools/go.mod                go tool pin for golangci-lint and pinact
Makefile                    lint / pinact / test / build targets
.goreleaser.yml             release archive config
.tagpr                      release PR / tag automation
```

## Development

```bash
make test            # go test ./...
make lint            # golangci-lint via go tool
make pinact-verify   # verify pinned GitHub Actions
make build           # cross-compile bin/ for darwin/linux × amd64/arm64
```

## Release flow

1. Merge changes into `main`
2. tagpr opens a "Release for vX.Y.Z" PR; merging it pushes the tag
3. The `release` workflow runs goreleaser, producing
   `chain-command-blocker_{os}_{arch}.tar.gz` + `checksums.txt`
4. Over in the plugin repo, the maintainer runs
   `./scripts/sync-from-src.sh vX.Y.Z` to pull the binaries into
   `bin/` and commit them at the plugin repo's own cadence

## License

MIT — see [LICENSE](LICENSE).
