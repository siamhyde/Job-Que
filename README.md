# Job Que

Job Que is a personalised job-discovery and application-tracking system.

It turns fragmented vacancy alerts into structured job records that can be deduplicated, enriched, assessed against an individual profile and reviewed through a prioritised queue.

The system currently supports two users with independent profiles, search strategies and vacancy queues.

## Why I Built It

Job searching is fragmented across alert emails, job boards, saved links and application records.

Searching requires constant motivation, context-switching and discernment.

I built Job Que to turn that into one workflow: collect vacancies, recover fuller descriptions, remove duplicates, assess each opportunity against the user and make the reasoning visible.

## Demo

<table>
  <tr>
    <td width="65%">
      <img width="900"
        alt="Job Que personalised vacancy queue"
        src="assets/job-que-overview.png" />
    </td>
    <td width="35%" valign="top">
      <h3>Personalised job queue</h3>
      <pre>
PROFILE-BASED REVIEW

Strong matches:       1
Potential matches:    4
Explore directly:    71

503 vacancies available

Each vacancy is assessed
against the individual
user's profile.

Save, skip or inspect
before applying.
      </pre>
    </td>
  </tr>

  <tr>
    <td width="65%">
      <img width="755"
        alt="Job Que detailed vacancy assessment"
        src="assets/strong-match-example.png" />
    </td>
    <td width="35%" valign="top">
      <h3>Explainable assessment</h3>
      <pre>
IMPLEMENTATION CONSULTANT

89 / 100 · Strong

Quick read:
Worth a closer look

The assessment surfaces:

• Why the role could fit
• What could rule it out
• Information still unknown
• Role responsibilities
• Profile-specific reasoning

Users can open the original
listing, save, skip or record
an application.

Scores support review rather
than replacing user judgement.
      </pre>
    </td>
  </tr>
</table>

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
