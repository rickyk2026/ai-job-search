# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (your market's job boards - scaffold one with `/add-portal`):
- **naukri.com** - India's largest general job board
- **linkedin.com/jobs** - LinkedIn job listings (filter: India / Bangalore); also covered by `linkedin-search` CLI
- **instahyre.com** - a niche board strong on management/strategy/BA roles (optional)
- **iimjobs.com** - MBA/management-focused board, strong for Management Trainee & Strategy roles (optional)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. Combine each query with your location terms (Bangalore / India) where the site supports it.

**Organize by function, not job title.** The same underlying work carries different titles across companies and markets (a "Data Scientist" role at one employer may be posted as "Insights Analyst" or "Data Consultant" at another). Name each priority category after the function it covers, and list several plausible job titles as query variants within that category rather than betting an entire priority tier on one exact title string.

### Priority 1: Strategy & Consulting

These match your strongest and most desired career direction.

```
site:naukri.com "Strategy Consultant" Bangalore
site:naukri.com "Management Consultant" Bangalore
site:naukri.com "Business Analyst" Bangalore
site:linkedin.com/jobs "Strategy Consultant" India
site:linkedin.com/jobs "Management Consultant" India
site:iimjobs.com "Strategy" Bangalore
```

### Priority 2: Management Trainee / MBA Programs

Rotational/leadership development programs aimed at MBA graduates.

```
site:naukri.com "Management Trainee" Bangalore
site:naukri.com "MBA" "Management Trainee" India
site:linkedin.com/jobs "Management Trainee" Bangalore India
site:iimjobs.com "Management Trainee" OR "MT Program"
```

### Priority 3: Business Process Consulting / Business Analysis (domain expertise)

These match your domain expertise in BPMN, SAP Signavio, and process transformation.

```
site:naukri.com "Business Process Consultant" Bangalore OR India
site:naukri.com "Process Analyst" SAP Bangalore
site:linkedin.com/jobs "Business Process Consultant" Bangalore India
site:instahyre.com "Business Analyst" process
```

### Priority 4: Broader B2B / Project Management

Wider net for adjacent roles you could pivot into.

```
site:naukri.com "Project Manager" stakeholder Bangalore
site:linkedin.com/jobs "Business Analyst" "stakeholder management" Bangalore
site:naukri.com "B2B" "account manager" OR "partnerships" Bangalore
```

### Priority 5: Brand Marketing

Broadened at the candidate's request (2026-11-06) to widen the daily application pool beyond Strategy & Consulting, after that run showed most brand-name strategy roles gate on 2-6+ years post-MBA experience the candidate doesn't yet have. These are entry/associate-level MBA-fresher tracks more realistically winnable at the ₹10L+ target.

```
site:naukri.com "Brand Manager" MBA fresher Bangalore
site:naukri.com "Assistant Brand Manager" Bangalore
site:linkedin.com/jobs "Brand Manager" OR "Marketing Manager" MBA Bangalore India
site:iimjobs.com "Brand" "Management Trainee" OR fresher
```

### Priority 6: Business Analyst (entry/associate level)

```
site:naukri.com "Business Analyst" MBA fresher Bangalore
site:naukri.com "Associate Consultant" MBA Bangalore
site:linkedin.com/jobs "Business Analyst" MBA fresher India
site:iimjobs.com "Business Analyst" 0-2 yrs
```

### Priority 7: Operations Analyst / Operations Associate

```
site:naukri.com "Operations Analyst" MBA Bangalore
site:naukri.com "Operations Associate" fresher Bangalore
site:linkedin.com/jobs "Operations Analyst" MBA India
site:iimjobs.com "Operations" "Management Trainee" OR fresher
```

## Location Filter

Updated 2026-11-06 at the candidate's request: target cities widened from Bangalore-only to Bangalore, Pune, Chennai, Hyderabad, and Gurgaon.

When evaluating results, verify the job location is within reasonable commute distance from your home, or is remote/hybrid. Define acceptable areas:
- **Primary target cities:** Bangalore (and surrounding areas - Whitefield, Electronic City, Koramangala, Indiranagar, etc.), Pune, Chennai, Hyderabad, Gurgaon (Gurugram)
- Other major Indian metros (Delhi NCR beyond Gurgaon, Mumbai) - acceptable given openness to relocation
- Fully remote roles (India-based) - acceptable
- Outside India without visa/relocation support - too far (per Deal-breakers in CLAUDE.md)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
