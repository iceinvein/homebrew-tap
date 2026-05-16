# iceinvein/homebrew-tap

Homebrew tap for macOS Apple-Silicon tools maintained by [`iceinvein`](https://github.com/iceinvein).

## Install

```bash
brew tap iceinvein/tap
brew install code-intelligence-mcp
brew services start code-intelligence-mcp
```

## Formulae

### [`code-intelligence-mcp`](Formula/code-intelligence-mcp.rb)

Local code intelligence MCP server: semantic search, graph navigation, on-device LLM descriptions, embedded dashboard. Apple Silicon (arm64) only. macOS 13+ required for the modern `launchctl bootstrap` API.

After install:

- Daemon: `http://127.0.0.1:17800/mcp`
- Dashboard: `http://127.0.0.1:17802/`
- Logs: `brew services log code-intelligence-mcp`

Bind a workspace by configuring your MCP client URL as
`http://127.0.0.1:17800/mcp?repo=/abs/path/to/your/repo`, or run
`code-intelligence-mcp-server migrate` to rewrite an existing v3
`~/.claude.json` entry.

Upstream repository: [`iceinvein/code_intelligence_mcp_server`](https://github.com/iceinvein/code_intelligence_mcp_server).

## Releasing

The canonical formula source lives in the upstream repo at
`dist/homebrew/code-intelligence-mcp.rb`. This tap mirrors it. The
release flow is:

1. `scripts/release.sh X.Y.Z` in the upstream repo bumps the formula
   version and resets the sha256 placeholder.
2. Push the tag; the `Release` GitHub Actions workflow builds the
   binary tarball, uploads it to the GitHub Release, and prints the
   tarball's sha256 in the workflow summary.
3. Sed the printed sha256 into the upstream formula, commit, push.
4. Copy the updated formula into this repo:

   ```bash
   cp ../code_intelligence_mcp_server/dist/homebrew/code-intelligence-mcp.rb \
       Formula/code-intelligence-mcp.rb
   git commit -am "code-intelligence-mcp X.Y.Z"
   git push
   ```

5. Smoke test in a fresh shell:

   ```bash
   brew untap iceinvein/tap 2>/dev/null
   brew tap iceinvein/tap
   brew install --build-from-source code-intelligence-mcp    # validates the formula
   ```

## License

The formulae themselves are MIT. Each tool's license is in its own
repository.
