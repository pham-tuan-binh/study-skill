# Search playbook

Run these as parallel subagents. Each one returns a compact ranked candidate list (no tables needed) with these fields: name, org, v1 date, link, one line, headline result with protocol, relevance (high/medium/low), and an "unverified" flag where it applies.

## Agent A: citation graph

- Read the seed paper's related work and baselines.
- Pull forward citations of the seed and every central hub of the area (main benchmarks, most-cited datasets and systems), ranked by citation count. OpenAlex: `https://api.openalex.org/works?filter=cites:<openalex id>,from_publication_date:<YYYY-MM-DD>&sort=cited_by_count:desc&per-page=200&cursor=*`, then pass each response's `meta.next_cursor` as `cursor` until it is null (find the OpenAlex id with `https://api.openalex.org/works?search=<title>`). Semantic Scholar as a fallback: `https://api.semanticscholar.org/graph/v1/paper/arXiv:<id>/citations?fields=title,year,externalIds,abstract&limit=100&offset=0`; page through all results and back off on HTTP 429. Never stop at the first page.
- Batch-fetch abstracts for the shortlist (Semantic Scholar `/paper/batch` is a POST with body `{"ids": [...]}`, not a GET with `?ids=`).

## Agent B: keyword sweeps

- Search arXiv listings and the web for 10 to 15 phrasings of the area's vocabulary, covering both the field's own new terms and older terms.
- Restrict to roughly the last 18 months, and add the older foundational works that the newer ones build on.

## Agent C: leaderboards, lists, industry

- **Public leaderboards:** for every entry that is not a plain baseline, find its paper, repo or adapter. When the page won't render, benchmark adapter repos often list entries.
- **Curated lists:** go through the awesome-lists entry by entry, and use existing surveys to cross-check coverage.
- **Company and lab blogs:** include a post only if it describes a concrete system.

## Agent D: lab, org and hub sweep

- List the 20-30 labs and companies active in the area. For each, check the blog, GitHub org, Hugging Face org and recent model launch pages (launch pages now carry robot and agent benchmark numbers).
- Sweep Hugging Face datasets and models for the area's tags, sorted by downloads and by likes, several pages deep; sweep GitHub and PyPI for frameworks. Record download and star counts as adoption evidence.
- Use press coverage to recover X announcements, since X itself usually can't be fetched.

## Agent E: hidden releases and parents

- Go through the area's main model and method papers and record any dataset, benchmark, harness or data engine they release, even when the title doesn't name it.
- For every candidate that belongs to a larger project, look up the parent project and its sibling releases.
- List adjacent categories the brief doesn't name and recommend in or out of scope for each.

## Merge

- Deduplicate across agents, watching for the same group publishing under different names.
- Rank the candidates, then split them into strong and weak using the triage rules in `SKILL.md`.
- Show the user the ranked shortlist, with a line per item, before writing pages. They decide which ones get full pages.

## Verification

- Open the full text before trusting abstract numbers.
- Record each number's protocol: official eval, subset, self-run, privileged state, demos, budget.
- Mark affiliations guessed from author names as unverified.
