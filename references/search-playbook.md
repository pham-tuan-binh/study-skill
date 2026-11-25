# Search playbook

Run these as parallel subagents. Each one returns a compact ranked candidate list (no tables needed) with these fields: name, org, v1 date, link, one line, headline result with protocol, relevance (high/medium/low), and an "unverified" flag where it applies.

## Agent A: citation graph

- Read the seed paper's related work and baselines.
- Pull forward citations of the seed and the 2 or 3 most central papers from Semantic Scholar: `https://api.semanticscholar.org/graph/v1/paper/arXiv:<id>/citations?fields=title,year,externalIds,abstract&limit=100&offset=0`. Page through the results and back off on HTTP 429.
- Batch-fetch abstracts for the shortlist (`/paper/batch`).

## Agent B: keyword sweeps

- Search arXiv listings and the web for 10 to 15 phrasings of the area's vocabulary, covering both the field's own new terms and older terms.
- Restrict to roughly the last 18 months, and add the older foundational works that the newer ones build on.

## Agent C: leaderboards, lists, industry

- **Public leaderboards:** for every entry that is not a plain baseline, find its paper, repo or adapter. When the page won't render, benchmark adapter repos often list entries.
- **Curated lists:** go through the awesome-lists entry by entry, and use existing surveys to cross-check coverage.
- **Company and lab blogs:** include a post only if it describes a concrete system.

## Merge

- Deduplicate across agents, watching for the same group publishing under different names.
- Rank the candidates, then split them into strong and weak using the triage rules in `SKILL.md`.
- Show the user the ranked shortlist, with a line per item, before writing pages. They decide which ones get full pages.

## Verification

- Open the full text before trusting abstract numbers.
- Record each number's protocol: official eval, subset, self-run, privileged state, demos, budget.
- Mark affiliations guessed from author names as unverified.
