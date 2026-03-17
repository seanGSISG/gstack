# Next Steps — Testing & Validation

Things to verify now that the Windows fork is functional. None are blocking — the core stack works.

## 1. Test `/qa` against a real app

`/browse` works, so `/qa` should too. Start a local dev server and run:

```
/qa http://localhost:3000
```

This tests the full QA pipeline: diff analysis, page navigation, screenshot capture, issue detection, fix loop.

## 2. Test `/review` and `/ship` with CodeRabbit

Needs a GitHub repo with [CodeRabbit](https://coderabbit.ai) installed. Create a PR, let CodeRabbit post review comments, then run `/review` or `/ship`. Verify:

- Comments are fetched via `coderabbitai[bot]` username
- Classification works (VALID, FALSE POSITIVE, ALREADY FIXED)
- Reply templates post correctly to the PR

## 3. Run LLM evals (no API key needed)

```bash
cd ~/.claude/skills/gstack  # or your gstack project dir
EVALS=1 bun test test/skill-llm-eval.test.ts
```

Verifies the `claude -p` judge replacement works end-to-end. Uses your Claude Code Max subscription, not API credits.

## 4. Test `/setup-browser-cookies`

Cookie decryption from browser DBs (DPAPI on Windows) is not yet supported. But JSON file imports and unencrypted cookies should work. Try:

```
/setup-browser-cookies
```

Verify it detects installed browsers and the picker UI opens.

## 5. Fresh install test

In a new project, verify the install flow works:

```
cd <new-project>
cp -Rf ~/.claude/skills/gstack .claude/skills/gstack
cd .claude/skills/gstack && ./setup
```

Then try `/browse https://example.com` to confirm skill symlinks and binary are working.

## 6. Test `stop` command reliability

The `browse stop` command timed out during testing. Worth investigating if the server shutdown is clean on Windows — may need a `taskkill` fallback instead of relying on the HTTP stop endpoint.
