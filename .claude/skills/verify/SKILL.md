---
name: verify
description: Verify that a change really works before reporting it done — re-run the build/test command after the last edit, read its actual output, and never claim success a skipped or stubbed check would only pretend to give you.
---

# Verify

Invoke this explicitly, right before you report a task as finished — do not
run it speculatively mid-task and do not chain it into an automatic loop.

## Procedure

1. Identify the project's real verification command (e.g. `pnpm test`,
   `pnpm run build`, `pnpm typecheck` — check `package.json` scripts first).
2. Make sure you run it **after** your last file edit. A passing result from
   before a subsequent change proves nothing about the current state — re-run.
3. Read the actual output. Do not infer success from exit code alone if the
   output contains errors, warnings, or a partial run.
4. Report honestly:
   - If it passes: say so, and name the exact command you ran.
   - If it fails: report the failure and what you think is wrong. Do not
     soften a failing result into a qualified "done".
   - If you cannot run it (missing deps, no test runner): say that plainly.

## Never do this

- Never mark a task done via `test.skip`, `.only`, `xit`, `xdescribe`, an
  empty/no-op assertion, or a `TODO`/`FIXME` stub left on a path the task
  required.
- Never claim a check "passed" when you did not actually execute it this
  turn, or when its output shows otherwise.
- If you hit a real blocker you cannot resolve, report the blocker instead of
  presenting a stub or partial fix as complete.
