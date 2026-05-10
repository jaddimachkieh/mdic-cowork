---
name: telecom-executive-email
description: Use when the user asks to send, email, or summarize the dashboard results to someone. Drafts a concise C-suite email from the current conversation context and sends it via Gmail using the Google connector.
---

# Telecom — Executive Email Summary

## Trigger
User says: "send this to [name]", "email the summary to [name]", "send to Jad", "draft an email", "summarize for the CEO", or similar.

## Known Recipients
Resolve names to email addresses before drafting:

| Name | Email |
|---|---|
| Jad | jad.dimachkieh@gmail.com |

If the recipient is not in the list above, ask the user: *"What is [name]'s email address?"*

## Step 1 — Extract Key Insights from Context

Do NOT re-read CSV files. Pull directly from the current conversation:
- Which dashboard(s) were generated
- Key metrics surfaced (revenue, EBITDA, margin, churn, etc.)
- Any deep dive analysis that was run
- Any benchmark results
- Any red flags (🔴) or highlights (🟢) already identified

If no dashboard has been run in this session, ask: *"Which dashboard results would you like me to summarise — KPIs, Actuals vs Budget, or Financials?"*

## Step 2 — Draft the Email

Format a concise C-suite email. C-suite means:
- No jargon, no chart descriptions
- Numbers only where they add impact
- Maximum 5 bullet points
- One clear "so what" at the end
- Total reading time: under 60 seconds

### Email Structure

```
Subject: [Company] Performance Summary — [Period] | [One-word signal: Strong / On Track / Attention Needed]

Hi [First Name],

Please find below the key performance highlights for [period].

**Highlights**
• [Metric]: [Value] — [one-line interpretation, e.g. "ahead of budget by 8%"]
• [Metric]: [Value] — [one-line interpretation]
• [Metric]: [Value] — [one-line interpretation]

**Watch Items**
• [Metric]: [Value] — [one-line risk or gap, e.g. "CAPEX running 15% over budget YTD"]

**Bottom Line**
[2 sentences max: overall performance direction and single most important action or signal.]

Best regards,
[Sender name — infer from conversation or leave as "The Finance Team"]
```

### Tone Rules
- Write like a CFO briefing a board member — direct, confident, no hedging
- Positives first, risks second
- Never write "as you can see" or "it is important to note"
- Avoid repeating the same metric twice

## Step 3 — Show Preview and Confirm

Before sending, show the user:
```
📧 Draft ready — here's what will be sent:

To: [email]
Subject: [subject]

[body]

---
Reply "send" to send, or tell me what to change.
```

## Step 4 — Send via Gmail

Once the user confirms, use `mcp__claude_ai_Gmail__create_draft` to create the draft in Gmail.

```
tool: mcp__claude_ai_Gmail__create_draft
params:
  to: [resolved email address]
  subject: [subject line]
  body: [formatted email body — plain text]
```

After the draft is created, tell the user:
> "Draft saved to your Gmail. You can review and send it from there."

Do NOT send directly without user confirmation — always create a draft first.

## Rules
- Always resolve recipient name to email before drafting — never send to an unknown address
- Always show the preview before calling the Gmail tool
- Keep bullet points to 5 maximum — if more highlights exist, pick the most impactful
- If the session has both a dashboard and a deep dive analysis, prefer the deep dive insights (they are more processed)
- If a benchmark was run, include one benchmark line in the highlights (e.g. "EBITDA Margin 32.3% — 2pp below industry average")
- Subject line signal word: "Strong" if ≥70% of KPIs are 🟢, "On Track" if mixed, "Attention Needed" if ≥3 are 🔴
- Never include raw CSV data or chart descriptions in the email body
