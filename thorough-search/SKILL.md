---
name: thorough-search
description: >-
  Surveys libraries, tools, and "what exists" questions by searching language
  registries (npm, PyPI, crates.io, crates, Maven, Go) and GitHub as well as
  the web, using multiple query variants and a recent-package pass. Use when
  recommending libraries, answering "is there a library for X", comparing
  frameworks, researching frontend or backend tools, or doing discovery search
  where well-known names can crowd out newer options.
---

# Thorough search

Do not answer a discovery question from memory or one web search. Well-known names crowd out newer packages.

## When this applies

- "Is there a library / crate / package for X?"
- Recommend, compare, or survey tools
- "What's the current way to do X?"
- The user expected a specific name you missed

Skip for a named API lookup ("how does `pdf-lib` embed fonts?") — fetch that project's docs instead.

## Hard rules

1. Search the **language registry** and **GitHub**, not only the web.
2. Run **at least three query variants** in parallel. Do not seed them with names you already know.
3. Include a **recent pass**: current year (and prior year) plus "new", "released", or registry sort-by-date.
4. If the user named a candidate, look it up first — then still survey unless they asked only about that one.
5. Docs MCPs (Context7, etc.) are for a name you already have. They are not discovery.
6. After shortlisting, **open the homepage / README / npm page** for each serious candidate. Do not recommend from a search snippet alone.

## Query set

Build queries from **capability + constraints**, not from famous libraries.

Example: "frontend PDF in the browser with WASM"

| Variant | Example |
|---|---|
| Capability + runtime | `wasm pdf browser javascript` |
| Capability + input shape | `jsx html css pdf webassembly` |
| Registry keywords | npm: `wasm pdf`, `html-to-pdf` |
| Recent | `wasm pdf jsx 2026`, npm sort by last publish |
| Adjacent ecosystem | `satori pdf`, `og image pdf rust wasm` |

If the first page is only names you already knew, the query failed. Change terms (input format, runtime, language of the engine) and search again.

## Where to search

Pick the registry for the user's stack. Run registry + GitHub + web together.

| Stack | Registry |
|---|---|
| JS / TS | `npm search <q> --searchlimit 20` and [npmjs.com/search](https://www.npmjs.com/search) |
| Python | `pip index versions` is not discovery — use [pypi.org/search](https://pypi.org/search) |
| Rust | [crates.io/search](https://crates.io/search) |
| Go | `pkg.go.dev/search?q=` |
| Other | language package index + GitHub topic search |

GitHub: search repos by keyword, sort by **Recently updated** as well as stars.

See [registries.md](registries.md) for commands.

## Rank results

Prefer evidence over fame:

- Last publish / last commit (stale vs active)
- Created date (a 2-week-old package can still be the right answer)
- Runtime fit (browser, WASM, edge, Node)
- Docs that match the asked API (JSX, HTML, CLI, …)
- License and install size when relevant

Stars and "everyone uses X" are not a veto on a newer fit.

## Answer shape

1. **Best fit** for the stated constraints, with a one-line why.
2. **2–4 alternatives**, including at least one newer or less famous option — or say the recent pass found nothing credible.
3. For each: what it is, engine (JS / WASM / native), and a docs or repo link.
4. If you likely missed something, say what you searched and what you did not.

## Failure you are preventing

Asking "WASM PDF in the browser" and answering only `pdf-lib`, Typst, and `wasm-pdf` — while `takumi-pdf` (WASM, JSX/HTML, browser) existed and was not in the query set.
