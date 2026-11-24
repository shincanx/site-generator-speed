# site-generator-speed

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/shincanx/site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/shincanx/site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, written for the make.fun bounty
[Build a static site faster, 22 November 2026](https://make.fun/competitions/site-generator-speed-01-run-20261122).

`solve.js` exports `run(input)`, which returns exactly the answer makefun-site
1.0.0's `build` returns for the same input: the output tree of the site as a
JSON object of every path it writes and that file's text, byte for byte.
`main.ts` is the program the harness runs: it reads every case from
`/input/cases.json`, answers each one alone with `run`, and writes
`/output/answers.json`.

The implementation follows makefun-site's README and the behaviour of its
reference code — its front matter, template dialect, Markdown subset, slug
rule and report are its own — with the same pipeline, order and report lines,
only faster:

- Markdown inline rendering scans plain runs in bulk instead of one regex
  replace per character, and the block lexer dispatches on the line's first
  character.
- `slugify` memoises per build and skips NFKD for pure-ASCII text (NFKD is
  the identity on ASCII).
- Front matter is scanned with `indexOf` instead of splitting the body.
- Templates are compiled once per build into plain JS functions
  (null-prototype scope chains keep the dialect's lookup rules exact),
  with a small tree-walking interpreter as fallback.
- Word counting, line trimming and HTML escaping use manual scans instead
  of per-match regex callbacks.

Local grade on the kit's practice set (3 cases): `passed 1`, 3/3 cases,
reproduced byte for byte, about 2.9× the house baseline's CPU time.

## License

MIT. See [LICENSE](LICENSE).
