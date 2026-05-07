# Trex CLI

`trex` is a Supabase-CLI-compatible binary for managing Trex deployments. It
implements the same command surface as the upstream Supabase CLI — `trex login`,
`trex link`, `trex functions deploy`, `trex secrets set`, `trex gen types`,
`trex config push` — and points at the Supabase-compatible management API
exposed by Trex.

This package is a fork of [`supabase/cli`](https://github.com/supabase/cli). The
source is otherwise upstream-compatible; only the published artifact name (`trex`)
and a few user-visible strings differ.

## Install

### npm

```bash
npm install -g trex
```

The postinstall script downloads the platform-specific binary from the GitHub
release matching the package version.

### Homebrew

A Trex tap is not yet published. Install via npm or build from source.

### Build from source

```bash
git clone https://github.com/p-hoffmann/cli
cd cli
go build -o trex
./trex --version
```

## Use

Point the CLI at your Trex server (defaults to `http://localhost:8001`):

```bash
trex login --use-api http://localhost:8001
trex link --project-ref trexsqldefaultlocall --use-api http://localhost:8001

trex functions new hello-world
trex functions deploy hello-world

trex secrets set MY_API_KEY=…
trex gen types typescript --schema public,trex
```

See the [CLI documentation](https://github.com/p-hoffmann/trexsql/tree/main/plugins/docs/docs/cli.md)
for the full command list and compatibility notes.

## Development

This is a Go project. Run the test suite with:

```bash
go test ./...
```

The CI configuration (`.github/workflows/ci.yml`) runs lint, unit tests, and
end-to-end tests against a local stack. Releases are produced by
`.github/workflows/release-beta.yml` via [GoReleaser](https://goreleaser.com/),
which builds binaries for darwin/linux/windows on amd64/arm64 and publishes
deb / rpm / apk / archlinux packages plus an npm wrapper.

## Compatibility

The CLI's Go module path remains `github.com/supabase/cli` (the upstream fork
path) so that import paths and ldflag injections work without rewriting every
file. The published binary, npm package, and release archives are named `trex`.

The on-disk config directory (`supabase/config.toml`) and the `SUPABASE_*`
environment variables are left as-is for compatibility with the upstream
Supabase CLI workflow — projects can switch between `supabase` and `trex`
without re-running `init`.

## License

MIT, inherited from upstream Supabase CLI.
