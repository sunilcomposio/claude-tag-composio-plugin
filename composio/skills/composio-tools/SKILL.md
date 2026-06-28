---
name: composio-tools
description: >-
  Use when a task needs an external app that Composio provides (Gmail, Slack,
  Linear, Notion, GitHub, calendars, CRMs, and 1000+ others) through the
  Composio MCP server. Covers the search-then-execute pattern, picking the right
  tool slug, and connecting an account before acting.
---

# Using Composio tools

The Composio MCP server (`https://connect.composio.dev/mcp`) is a single endpoint
that exposes 1000+ tools across many apps. Rather than loading every tool, it
works on a **search-then-execute** pattern. Follow this order.

## 1. Find the right tool first

Before acting, search for the tool that matches the task instead of guessing a
slug. Use the discovery tool to list candidates for the app and action you need
(for example "send email", "create issue", "list calendar events"). Tool slugs
follow an `APP_VERB_NOUN` convention, e.g. `GMAIL_SEND_EMAIL`,
`LINEAR_CREATE_ISSUE`, `GOOGLECALENDAR_LIST_EVENTS`.

## 2. Check the connection

Many tools act on a user's connected account. If a call fails with an auth or
"no connected account" error, the account for that app has not been linked yet.
Surface this to the channel and have an admin connect the account in Composio
rather than retrying blindly. Do not attempt to enter credentials.

## 3. Get the tool's input schema, then execute

Once you have the slug, fetch its required parameters, fill them from the
request, and execute. For multi-step jobs, prefer the parallel/multi-execute
tool over many sequential calls.

## 4. Confirm before side effects

Sending email, creating tickets, posting messages, and any other write action
should be confirmed in the channel before you run it, and the result reported
back in the thread.

## Scoping

This plugin can reach any app the connected Composio account exposes. Keep the
attached credential scoped to the smallest set of apps a channel actually needs;
an incident channel does not need write access to a CRM, and vice versa.

## Common gotchas

- The endpoint is rate limited and tool responses can be large; ask for only
  what you need (filters, date ranges, specific fields).
- Slugs change occasionally; if a hardcoded slug 404s, re-run discovery to get
  the current name.
- Read actions are safe to run unprompted; write actions are not.
