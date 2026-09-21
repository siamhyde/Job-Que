# Job Que

Job Que is a private job-discovery and application-tracking system with personalised review queues.

It turns fragmented job alerts and vacancy listings into structured records that can be enriched, assessed against an individual profile, reviewed and tracked through the application process.

I built the intake pipeline, matching logic, review interface and multi-user workflow. This repository presents the system as a portfolio project; application code and user data remain private.

## Why I Built It

Job searching creates information across alert emails, job boards, saved links and application records. The same vacancy can appear repeatedly, while an email preview often leaves out the details needed to decide whether a role is worth pursuing.

I built Job Que to bring that workflow together: collect vacancies, resolve duplicates, recover fuller descriptions where possible and make the reasons behind each assessment visible.

A second user searches for publishing roles, while my own search focuses on technology and data. Supporting both meant making profiles, matching criteria and queues independent.

## Demo

| View | What the user can do |
| --- | --- |
| **Queue** | Review prioritised vacancies, inspect fit explanations and concerns, and browse across sources and role groups. |
| **Saved** | Keep roles to revisit without losing them in the active queue. |
| **Applications** | Record a manual application and track later status changes. |
| **Profile** | Edit target roles, skills, experience evidence, practical preferences and matching weights. |

```text
Open vacancy → Review details and reasoning → Save or skip
                              ↓
                  Apply on the original site
                              ↓
                 Record and track the application
```

Incomplete records are labelled **Assessment pending** or **Manual review needed**. A score derived from limited metadata is not presented as a completed assessment.

<!-- Add reviewed screenshots of the real interface using synthetic demonstration
 data here. Suggested images: queue overview, expanded vacancy, contrasting demo
 profiles and application tracking. Label them as demonstration data. -->

## End-to-End Workflow

1. **Collect:** receive job-alert emails through Resend, discover vacancies through the Reed API, or accept manual entries and supported vacancy URLs.
2. **Extract and normalise:** parse vacancy candidates into a common record structure, retaining original source links.
3. **Deduplicate:** compare source identities, canonical URLs and vacancy content within each user's records. Record intake outcomes for later inspection.
4. **Enrich:** use a local browser worker to retrieve fuller LinkedIn and Indeed descriptions. Preserve pending and failure states when details cannot be recovered.
5. **Assess:** apply profile-specific rules and exclusions, producing a score breakdown, fit explanations and concerns. Reassess eligible records when fuller descriptions arrive.
6. **Review and track:** let the user save, skip or apply on the original site, then maintain application status and history in Job Que.

## Multi-user Personalisation

Each user has an independent profile and queue. Matching uses target and adjacent roles, skills, experience evidence, location and workplace preferences, salary expectations and exclusions.

The two current use cases cover **technology/data** and **publishing**, demonstrating how the same workflow can support different search criteria.

For the publishing search, I researched specialist vacancy sources and configured The Bookseller alerts. That source is newly configured; successful vacancy ingestion has not yet been verified. A separate Independent Publishers Guild adapter is implemented locally, with production rollout still pending.

## Architecture

```text
Job-alert emails       Reed API       Manual intake
       │                   │                │
       └───────────────────┼────────────────┘
                           ↓
                 Next.js server pipeline
          Extract → Normalise → Deduplicate
                           ↓
                  Supabase / PostgreSQL
         Per-user vacancies, profiles and intake history
                           ↕
                Local Playwright worker
          LinkedIn / Indeed description enrichment
                           ↓
                Profile-based rule assessment
                           ↓
                   Next.js / React UI
             Review → Save → Apply manually → Track
```

The database holds the persistent records. The server coordinates intake, assessment and user-scoped operations, while the browser worker handles description enrichment separately from the web interface.

The private application repository includes automated tests for parsing, deduplication, ownership boundaries, database operations and browser review flows.

## Stack

**TypeScript · Next.js · React · Tailwind CSS · PostgreSQL / Supabase · Resend · Playwright · Zod**

Cheerio supports HTML parsing. SQL migrations define persistence and database operations.

## Scale

Verified against the live database on **21 September 2026**:

| Metric | Count |
| --- | ---: |
| Distinct stored vacancies | **527** |
| Job-alert emails processed | **107** |
| Vacancy candidates extracted from processed alerts | **629** |
| Duplicate candidates detected during alert intake | **307** |
| Vacancies successfully enriched | **288** |
| Users with independent profiles and populated queues | **2** |

**LinkedIn, Indeed and Reed** are represented in the stored vacancies. Of the 288 successful enrichments, 282 came through alert intake and six through manually submitted URLs.

These are a dated operational snapshot. Extracted candidates, duplicate detections and stored vacancies measure different stages of the pipeline; they are not interchangeable totals.

## Current Status and Limitations

Job Que is in private use with two populated user queues. This public portfolio documents its design and verified scale.

- **Human review remains central.** Applications are submitted manually on the original site. Matching scores are rule-based heuristics, not probabilities of receiving an interview or offer.
- **Source coverage is incomplete.** Alerts and provider responses determine what enters the system; it does not claim exhaustive coverage of the job market.
- **Enrichment can require intervention.** The local worker must be available, and missing listings, navigation failures or provider verification can prevent description retrieval.
- **Specialist sources are still being established.** The Bookseller vacancy intake is unverified, and the IPG adapter is not yet enabled in production.
- **AI assessment is optional, not part of the verified current results.** An optional integration exists in the code, but all vacancy records in the audited snapshot mark AI assessment as off.

Time savings, matching accuracy and employment outcomes have not been measured. The demonstrated work is the implemented workflow, its operational records and its support for distinct user profiles.
