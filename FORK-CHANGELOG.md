# Fork Changelog

Changes in this Windows-compatible fork ([seanGSISG/gstack](https://github.com/seanGSISG/gstack)) from upstream ([garrytan/gstack](https://github.com/garrytan/gstack)).

---

## 2026-03-16

### fix: Playwright hanging on Windows by running server under Node.js

Bun's Playwright integration hangs on `chromium.launch()` on Windows. The browse server now uses Node.js built-in APIs (`http.createServer`, `fs.*`, `child_process.*`) instead of Bun-specific APIs (`Bun.serve`, `Bun.write`, `Bun.file`, `Bun.spawnSync`, `Bun.spawn`).

On Windows, `browse.exe` spawns `node server.js` instead of `bun run server.ts`. The `server.js` bundle is compiled via `bun build --target node` during the build step. On macOS/Linux, the original `bun run` path is preserved.

**Files changed:** `browse/src/server.ts`, `browse/src/cli.ts`, `browse/src/config.ts`, `browse/src/write-commands.ts`, `browse/src/cookie-import-browser.ts`, `package.json`

### feat: Replace Anthropic SDK with `claude -p` for evals

Evals now use `claude -p` subprocess instead of `@anthropic-ai/sdk`, so they work with Claude Code Max subscription — no `ANTHROPIC_API_KEY` needed. The SDK dependency has been removed entirely.

**Files changed:** `test/helpers/llm-judge.ts`, `test/skill-llm-eval.test.ts`, `test/skill-e2e.test.ts`, `package.json`, `.env.example`, `CLAUDE.md`, `CONTRIBUTING.md`

### feat: Replace Greptile integration with CodeRabbit

Swapped all Greptile references to CodeRabbit across skills, triage logic, history files, tests, and documentation. The GitHub bot filter now targets `coderabbitai[bot]` instead of `greptile-apps[bot]`.

**Files changed:** `review/coderabbit-triage.md` (renamed from `greptile-triage.md`), `review/SKILL.md`, `ship/SKILL.md`, `retro/SKILL.md`, all `.tmpl` files, `README.md`, `CHANGELOG.md`, `CONTRIBUTING.md`, `TODOS.md`, test files

### feat: Windows compatibility (initial fork)

Full Windows support for gstack. Changes from upstream:

- **Temp paths:** All hardcoded `/tmp` references replaced with `os.tmpdir()`
- **Path separators:** Safe directory validation uses `path.sep` instead of hardcoded `/`
- **Process management:** Uses `taskkill`/`tasklist` on Windows instead of POSIX signals
- **Browser cookie paths:** Resolves to `%LOCALAPPDATA%` on Windows
- **URL opener:** Uses `cmd /c start` on Windows, `xdg-open` on Linux, `open` on macOS
- **SKILL.md preamble:** Falls back to `$USERPROFILE` for session directory, detects `.exe` binaries
- **Shell scripts:** Work in Git Bash (bundled with Claude Code on Windows)

**Known limitation:** Cookie decryption from browser imports (`/setup-browser-cookies`) is not yet supported on Windows (DPAPI differs from macOS Keychain).
