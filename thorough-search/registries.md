# Registry and GitHub commands

Run these in the Shell tool. Prefer official search UIs via WebSearch / WebFetch when the CLI is noisy.

## npm

```bash
npm search "wasm pdf" --searchlimit 20
```

Also fetch `https://www.npmjs.com/search?q=<query>` and sort or scan by **Last publish**.

Read a candidate: `https://www.npmjs.com/package/<name>` and its `repository` / `homepage`.

## PyPI

Fetch `https://pypi.org/search/?q=<query>`. Open the project page for created / last release dates.

## crates.io

Fetch `https://crates.io/search?q=<query>`. Open `https://crates.io/crates/<name>` for last update and docs.rs.

## Go

Fetch `https://pkg.go.dev/search?q=<query>`.

## GitHub

WebSearch or fetch:

```
https://github.com/search?q=<query>&type=repositories&s=updated&o=desc
```

Also try `s=stars`. Recently updated catches new work that stars miss.

## Web

At least two WebSearch calls with different phrasings. If results are only famous names, add the current year and a constraint the famous tools do not use (e.g. `jsx`, `tailwind`, `no chromium`).
