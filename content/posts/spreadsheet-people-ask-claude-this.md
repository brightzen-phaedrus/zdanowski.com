---
title: "If You Work With Spreadsheets, Ask Claude This Question"
date: 2026-04-15
draft: false
tags: ["building-with-ai", "spreadsheets", "tools"]
summary: "Google just released next-gen command line tools for Workspace. Sheets automation is back on the table — and Claude Code Routines are the right way to build it."
---

Google just shipped a new generation of command line tools for Workspace. The Sheets tools in particular are genuinely good now.

That means it's time to relearn how to automate spreadsheet work. Not the old way — not macros, not Apps Script. The new way: Claude Code Routines.

Here's the challenge Jeff gave me, and what I learned building it.

---

## The Challenge

For a given Google Sheet:

1. Open it
2. Review the forecasting tab — what we call the IFM tab
3. Recommend changes
4. Use [Google's Workspace CLI](https://github.com/googleworkspace/cli/blob/main/README.md) to do it

Clean scope. One input (a Sheet URL), one output (recommendations). That's what makes it a good first Routine.

---

## What a Routine Actually Is

A Claude Code Routine is a saved configuration: a prompt, optional repos, and connectors — packaged once and run on demand. Each Routine has a trigger: scheduled, GitHub event-based, or API.

For this use case — "give it a Sheet link, get back analysis" — the right trigger is API. You pass the URL in, get findings out.

---

## Phase 1: Prerequisites

**Plan check.** Routines are available for Claude Code users on Pro, Max, Team, and Enterprise with Claude Code on the web enabled.

**Connector.** The `googleworkspace/cli` (gws) is a single CLI covering Drive, Gmail, Sheets, Docs, Calendar, and more. You'll use it as a shell tool inside the Routine's cloud session — not API calls in code. Claude runs `gws` the same way you would at a terminal.

---

## Phase 2: Build the Routine

Go to `claude.ai/code/routines` → New Routine.

**Name:** IFM Readability Reviewer

**Prompt:**
```
You are a financial model readability analyst. You will be given a Google Sheets URL.

Your job:
1. Use the `gws` CLI to open the spreadsheet and read the tab named "IFM"
2. Analyze the IFM tab for readability across these dimensions:
   - Section structure and visual hierarchy (headers, grouping, indentation)
   - Formula complexity (are formulas self-documenting or opaque?)
   - Naming conventions (ranges, labels, row/column headers)
   - Color coding and formatting consistency
   - Navigation (can a reader orient quickly without a guide?)
   - Input/assumption separation from calculations
3. Produce a prioritized list of specific, actionable recommendations
4. Format output as: FINDING | SEVERITY (High/Med/Low) | RECOMMENDATION

The Google Sheets URL will be passed in the `text` field of the API request.
```

**Setup Script:**
```bash
npm install -g @google/workspace-cli
gws auth login --headless
```

You'll store your Google OAuth credentials as environment variables in the Routine config.

**Trigger:** API.

The modal shows you the endpoint URL and a one-time bearer token. Copy it immediately — it's shown once.

---

## Phase 3: Call It

```bash
curl -X POST https://claude.ai/code/routines/api/YOUR_ROUTINE_ID \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"text": "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit"}'
```

That starts a session. You get a URL back, follow it to watch Claude work and retrieve findings.

---

## Phase 4: Where This Goes Next

Once v1 runs clean:

- **Persist output** — add a Notion connector so Claude writes recommendations directly to your document hub
- **Parameterize the tab name** — accept a tab name alongside the URL, so this works across any sheet, not just ones named "IFM"
- **Schedule it** — add a weekly trigger, have it post findings to Slack every Monday

---

## The One Thing to Solve First

The `gws` CLI requires OAuth. In a headless cloud environment, interactive login doesn't work — you need a service account credential stored as an environment variable.

The [googleworkspace/cli README](https://github.com/googleworkspace/cli/blob/main/README.md) covers service account setup. That's your first implementation task. Get auth working locally before you build the Routine around it. Proving auth in isolation eliminates the most likely failure point before it costs you iteration cycles.

Start there. Then build.
