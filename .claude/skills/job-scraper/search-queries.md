# Search Queries for Job Scraper — Vietnam

<!-- SETUP: Customize role/skill placeholders and optional Vietnamese city filters; preserve the Vietnam market and source list. -->

## Installed portal CLIs (primary for `/scrape`)

This workspace searches **Vietnam only**. Discover installed skills, then run only the intersection of enabled skills and the sources below. Currently the only allowed installed CLI is `linkedin-search`, always with `--location "Vietnam"` (or an explicitly requested Vietnamese city followed by `, Vietnam`). Never use a bare `Remote` location. Keep the query's country filter even for remote roles.

The Danish portals (`jobindex-search`, `jobbank-search`, `jobdanmark-search`, `jobnet-search`) and `freehire-search` are outside this workspace's source list, regardless of their `enabled` flag. Do not search or health-probe them, or use them as a WebSearch fallback. New portal skills are not automatically included; the user must request adding the source to this list.

The Vietnamese boards below use the existing **WebSearch fallback** because they have no installed CLI. No new scraper or `/add-portal` step is required. Search their public job pages and retrieve the actual posting before presenting it. If access is blocked, follow `09-web-research.md` or ask for the posting text; do not invent results from snippets.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

| Source | Public entry point | Search method |
|--------|--------------------|---------------|
| TopCV | https://www.topcv.vn/ | WebSearch: `site:topcv.vn` |
| VietnamWorks | https://www.vietnamworks.com/ | WebSearch: `site:vietnamworks.com` |
| CareerViet | https://careerviet.vn/tim-viec-lam.html | WebSearch: `site:careerviet.vn` |
| Việc Làm 24h | https://vieclam24h.vn/ | WebSearch: `site:vieclam24h.vn` |
| ITviec (technical roles) | https://itviec.com/it-jobs | WebSearch: `site:itviec.com` |
| LinkedIn jobs in Vietnam | https://www.linkedin.com/jobs/search/?location=Vietnam | CLI with `--location "Vietnam"`; WebSearch fallback: `site:linkedin.com/jobs/view` |

For each selected role category, search all relevant sources in this table; use ITviec for technical roles only. A query block below shows examples, not a smaller source allowlist. Exclude editorial/blog pages, CV tools, course ads, and employer profiles that are not actual vacancies.

Secondary: independently located employer career pages for jobs in Vietnam. An employer may use an international ATS; verify the role's work location rather than inferring it from the website's country or company headquarters.

## Query Categories

Queries are grouped by priority. Write **each category in every language from your Languages table** (see Language scope above). Combine each query with your location terms (e.g. your city, region, or metro area) where the site supports it.

**Organize by function, not job title.** The same underlying work carries different titles across companies and markets (a "Data Scientist" role at one employer may be posted as "Insights Analyst" or "Data Consultant" at another). Name each priority category after the function it covers, and list several plausible job titles as query variants within that category rather than betting an entire priority tier on one exact title string.

### Priority 1: [YOUR_PRIMARY_ROLE_TYPE]

These match your strongest and most desired career direction.

```
site:topcv.vn "[YOUR_PRIMARY_JOB_TITLE_1]" "Việt Nam"
site:vietnamworks.com "[YOUR_PRIMARY_JOB_TITLE_2]" "Vietnam"
site:careerviet.vn "[YOUR_KEY_SKILL]" "Việt Nam"
site:linkedin.com/jobs/view "[YOUR_PRIMARY_JOB_TITLE_1]" "Vietnam"
```

### Priority 2: [YOUR_DOMAIN_EXPERTISE]

These match your domain expertise.

```
site:vieclam24h.vn "[YOUR_DOMAIN_KEYWORD_1]" "Việt Nam"
site:itviec.com "[YOUR_DOMAIN_KEYWORD_2]" "Vietnam"
site:vietnamworks.com "[YOUR_DOMAIN_KEYWORD_1]" "Vietnam"
```

### Priority 3: [YOUR_ADJACENT_ROLE_TYPE]

Adjacent roles you could pivot into.

```
site:topcv.vn "[YOUR_ADJACENT_TITLE_1]" "[YOUR_KEY_SKILL]" "Việt Nam"
site:careerviet.vn "[YOUR_ADJACENT_TITLE_2]" "[YOUR_KEY_SKILL]" "Việt Nam"
```

### Priority 4: Broader Technical / Consulting

Wider net for general technical roles.

```
site:itviec.com "[YOUR_KEY_SKILL]" developer "Vietnam"
site:linkedin.com/jobs/view "[YOUR_KEY_SKILL] developer" "Vietnam"
site:vieclam24h.vn "technical consultant" "[YOUR_DOMAIN]" "Việt Nam"
```

## Location Filter

Default to all of Vietnam; no city or commute limit has been supplied yet. If the user specifies cities or provinces, add those terms (including common accented/unaccented names) and apply their commute limits within Vietnam. Never replace Vietnam with the candidate's current country of residence during `/setup`.

Apply `04-job-evaluation.md`'s Vietnam location gate to the fetched posting, including employer-page recovery results. A Vietnamese board, a `Vietnam` search term, or a company's Vietnamese office is not proof that a specific role is in Vietnam. Exclude roles abroad and remote roles that do not explicitly accept work from Vietnam; keep unverified locations outside the matches pending verification. `broad`, a focus area, named portals, and health checks never expand this market or the source list.

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
