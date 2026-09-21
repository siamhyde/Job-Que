Absolutely — paste this directly into `README.md`:

````md
# Job Que

Job Que is a personalised job-discovery and application-tracking system.

It turns fragmented vacancy alerts into structured job records that can be deduplicated, enriched, assessed against an individual profile and reviewed through a prioritised queue.

The system currently supports two users with independent profiles, search strategies and vacancy queues.

## Why I Built It

Job searching was fragmented across alert emails, job boards, saved links and application records.

I built Job Que to turn that into one workflow: collect vacancies, recover fuller descriptions, remove duplicates, assess each opportunity against the user and make the reasoning visible.

## Demo

<!-- Add reviewed screenshots here using synthetic or non-sensitive demonstration data.

Suggested screenshots:
1. Main vacancy queue
2. Expanded vacancy with fit reasoning and concerns
3. Profile / personalisation view
4. Application tracking view
-->

## Architecture

```text
Job Alerts          Reed API          Manual URLs
     │                  │                  │
     └──────────────────┼──────────────────┘
                        ↓
               Intake & Normalisation
                        ↓
                  Deduplication
                        ↓
               Supabase / PostgreSQL
                        ↕
             Playwright Enrichment
                        ↓
              Profile-Based Scoring
                        ↓
                 Job Que Interface
                        ↓
             Review → Save → Apply → Track
````

Vacancies are stored independently for each user.

LinkedIn and Indeed listings can be enriched through a local browser worker before being reassessed against the user's profile.

## Personalisation

Each user has an independent profile containing target roles, skills, experience evidence, location preferences, salary requirements and exclusions.

The current users search across two different domains:

* **Technology & data**
* **Publishing**

The same underlying pipeline generates separate, personalised queues for both.

## Stack

**TypeScript · Next.js · React · Tailwind CSS · PostgreSQL / Supabase · Resend · Playwright · Zod**

## Scale

Verified **21 September 2026**:

**527 stored vacancies · 107 job-alert emails processed · 629 vacancy candidates extracted · 307 duplicates detected · 288 vacancies enriched · 2 independent user profiles**

Sources currently represented include **LinkedIn, Indeed and Reed**.

## Current Status

Job Que is in active private use.

Applications remain human-controlled: Job Que identifies, organises and assesses opportunities, while the user makes the final decision and applies on the original site.

Application code and personal user data remain private.

```

This is much closer to the **Hyde README style**: short introduction, origin, visual proof, architecture, stack, scale, status. Once you add 2–4 screenshots, it should read very cleanly as an employer-facing portfolio piece. 
```
