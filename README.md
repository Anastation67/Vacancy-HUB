# Vacancy Hub

A personal job-search tracker for an active job hunt — a single HTML file, no build step or dependencies, opens straight in the browser (and installs as a PWA on a phone).

## Why

Job hunting produces a steady stream of vacancies from different sources (email, Telegram, web search) that need to be scored, stored, and tracked without losing status on any of them. Vacancy Hub is the storage and workbench for that process: the actual sourcing and scoring of vacancies against a set of rules happens in a Claude project chat, and this app takes the resulting JSON and turns it into a workable list with filters, application statuses, and notes.

## What it does

- **Inbox** — import a JSON array of vacancies from the chat (one at a time or in bulk), tagged by source (email / Telegram / web search), with deduplication by company + title
- **Match scoring** — each vacancy shows a fit score and a confidence level (email-only / partial / from full description), with filters by company, platform, skills, and minimum score
- **Shortlist** — application statuses (saved → applying → applied → interview / rejected / closed) with a status history log and a notes field
- **Companies** — a target list for manual scans, which one click turns into a ready-made prompt for the chat
- **Profile** — target roles, skills, what to exclude (for reference — the actual scoring logic lives in the chat's own project instructions)
- **Rescoring** — one button collects a prompt with all open vacancies, for re-running them through the chat once new details emerge or a deadline passes
- Quick links (e.g. Google Drive) — open automatically the first time a vacancy is marked "applying"
- Storage — in the browser (localStorage / artifact storage), or synced across devices if a backend address is set in the profile
- Backup — export and restore the full state as a JSON file

## How it works

1. In the Claude project chat, following its own rules, you get vacancies back as JSON (already scored with fit/confidence/why/gaps).
2. Paste that JSON into Inbox → Import — new vacancies are added, duplicates are updated (confidence never drops from high back to low/medium).
3. Review the list, filter by score/skills/company, mark the interesting ones → "Add to shortlist" (or hide the rest).
4. In the shortlist, move the status forward as the application progresses, leave notes; marking "applying" opens a quick link (e.g. the CV drive folder).
5. If a vacancy is still "provisional" (confidence = low/medium), paste the job page text into the chat, get a rescored result, and import it back.
6. The Companies section keeps a target list and generates a prompt for finding new vacancies on their sites.

## Stack

Vanilla JS, HTML, CSS — no frameworks, no build. Storage via `window.storage` (when run as an artifact) with a `localStorage` fallback; optionally, a custom REST backend (`GET/PUT /state`).
