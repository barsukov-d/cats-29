---
name: ast-grep
description: Structural code search and replace using ast-grep (`sg`) — find call sites, rename symbols, or match code shapes by AST pattern instead of text/regex. Use explicitly when a task needs a precise structural match across files.
---

# ast-grep (`sg`)

Invoke `sg` explicitly, only when a task genuinely benefits from structural
(AST-based) matching over plain text search — do not run it speculatively and
do not chain it into an autonomous multi-step search loop.

`$$$` inside a pattern matches zero or more arguments/statements; a bare
`$NAME` (all caps) matches exactly one node and can be reused to constrain a
match (e.g. the same identifier on both sides of a call).

## Finding call sites

```
sg --pattern 'oldFunctionName($$$ARGS)' --lang ts
```

Finds every call to `oldFunctionName` regardless of argument count/shape,
across the whole workspace (run from `/workspace`).

## Scoping to a directory

```
sg --pattern 'console.log($$$ARGS)' --lang ts src/
```

## Structural rename (search + rewrite)

```
sg --pattern 'oldName($$$ARGS)' --rewrite 'newName($$$ARGS)' --lang ts -i
```

`-i` (interactive) shows each match with a diff before applying it — prefer
this over a blind rewrite. Omit `-i` only when you have already confirmed the
matches with a plain search and are confident about every hit.

## Other languages

Pass `--lang` for the language you're targeting (`ts`, `tsx`, `js`, `py`,
`go`, `rust`, …) — ast-grep parses per-language grammars, so an incorrect
`--lang` will simply find nothing.

## When to prefer `sg` over grep/sed

- Renaming a function/variable across many call sites (grep/sed risk matching
  inside strings/comments or missing reformatted call sites).
- Finding all usages of a specific call shape regardless of whitespace or
  argument formatting.

## When NOT to use it

- Plain string/log message searches — grep is simpler and sufficient.
- One-off single-file edits where reading the file directly is faster.
