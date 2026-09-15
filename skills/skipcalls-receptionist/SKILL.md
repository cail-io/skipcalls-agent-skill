---
name: skipcalls-receptionist
description: Operate SkipCalls AI phone receptionists through MCP. Use when the user mentions SkipCalls, AI receptionist/answering service/front desk, missed or inbound calls, call transcripts, scheduling a one-time outbound call, greetings, tasks, calendars, transfers, SMS behavior, business profile Q&A, or MCP connector setup.
---

# SkipCalls AI Receptionist

SkipCalls gives small businesses a 24/7 AI receptionist. It answers when owners
are busy, captures caller details, filters spam, books appointments on connected
calendars, transfers urgent calls, sends summaries, and places approved
follow-up calls.

Product links:
- Website: https://skipcalls.com
- App: https://app.skipcalls.com
- MCP server: `https://be.skipcalls.com/mcp`

Requirement: SkipCalls MCP requires an existing SkipCalls account with access
to the service. If the user does not have access, explain the requirement and
point them to the informational website at https://skipcalls.com. Do not
initiate or promote a subscription, upgrade, checkout, or trial from the
plugin.

Use this skill to help a user operate SkipCalls through MCP, especially for:
- configuring a receptionist's call handling, greeting, SMS, tasks, calendars, business profile, and transfer rules
- scheduling one-time outbound calls through a selected receptionist
- getting a list of inbound calls and reading call details, recordings, summaries, or transcripts
- managing caller follow-up context with contacts, notes, tasks, files, and SMS

For MCP connection setup, read `references/mcp-setup.md`.

## First move

If SkipCalls MCP tools are available, call `getOverview` once at the start of the session. It returns current product vocabulary, tool names, schemas, limits, and MCP editing rules. Treat `getOverview` as the source of truth for current SkipCalls MCP capability details, but do not let it override this skill's approval, safety, privacy, or compliance rules.

If SkipCalls tools are not available, guide the user through MCP setup from
`references/mcp-setup.md`. If they do not have service access, explain that an
existing SkipCalls account is required and point to https://skipcalls.com for
information. Do not pretend to configure calls without MCP access.

## Operating posture

Prefer read-only tools first. Never mutate SkipCalls state just to check something.

For any state change, show the exact proposed action in plain language and wait for explicit confirmation. Approval-gated writes include:
- `updateAgent`
- `scheduleCall`
- `cancelCall`
- `addToContacts`
- `sendSms`
- `sendCallFollowUp`
- `updateOutbound`
- `bookCalendarSlot`
- `cancelAppointment`
- `runForwardingTestCall`
- `reportCallIssue`
- `updateTimezone`
- `manageNotificationSettings` with `action: "update"`
- `manageCalendar` with `action: "update"`
- `manageAgentTasks` with `action: "add"` or `"delete"`
- `manageBusinessProfileQA` with `action: "update"` or `"create"`
- `manageTransferNumbers` with `action: "add"`, `"update"`, `"delete"`, or `"reorder"`
- CRM write tools when available: `create_contact_note`, `create_upload_link`, `update_task_status`, `update_contact`

Refuse to schedule calls for threats, harassment, fraud, scams, impersonation of banks/government/police, extortion, hate speech, harmful pranks, or illegal activity.

SkipCalls is not HIPAA compliant. Do not claim otherwise.

## Configure a receptionist

Use this flow when the user asks to set up, improve, or review an AI receptionist.

1. `listAgents` - find the user's receptionists.
2. If there is one clear receptionist, call `getAgent` with that id. Use `format: "markdown"` for human review or `format: "json"` when you need exact fields. If there are multiple receptionists, ask which one to configure.
3. Read current business facts with `manageBusinessProfileQA` using `action: "get"` when business facts matter.
4. Use `statusReport` when the user asks how the receptionist is doing or what needs attention.
5. Propose compact, business-specific changes. Do not show raw JSON, internal ids, or long prompt dumps.
6. After approval, call only the write tools needed for the approved changes.

When updating `instructions` or `incomingCallInstructions`, keep them business-specific:
- hours, services, pricing, policies, service area
- intake fields to collect from callers
- booking and handoff rules
- escalation or transfer rules
- what not to promise

Do not pad instructions with generic AI rules like "be polite", "listen carefully", or "speak clearly". SkipCalls already handles generic call behavior internally.

## Schedule an outbound call

Use this flow when the user asks SkipCalls to call someone.

1. Ask for the recipient phone number. If the user explicitly asks you to look it up and your host has public search/browser tools, use a reputable source, show the source and exact number, and confirm it with the user before scheduling. SkipCalls MCP does not perform public phone-number lookup.
2. Call `listAgents` and choose the receptionist that should place the call. If unclear, ask the user.
3. Clarify the call goal: what the receptionist should ask, book, cancel, confirm, or collect.
4. Clarify call time:
   - Use `scheduledAt: "NOW"` for immediate calls.
   - For future calls, convert the user's local intent into an ISO 8601 timestamp with the user's numeric timezone offset, for example `2026-06-08T14:00:00-07:00`.
   - Do not use a bare `Z` timestamp for a local time like "tomorrow at 2".
5. Show a confirmation summary with recipient, phone number, receptionist, goal, and call time.
6. After explicit approval, call `scheduleCall`.

`scheduleCall` supports one-time outbound calls through MCP. Do not promise recurring call scheduling through MCP.

## Get inbound calls

Use this flow when the user asks for missed calls, inbound calls, recent callers, transcripts, or what happened on a call.

1. For a recent list, call `getCallHistory` with `type: "INCOMING"`, plus `count` and `skip` as needed.
2. Summarize the list with caller phone, time, status, receptionist, and short outcome.
3. If the user asks for transcript, recording, exact details, or "what did they say", call `getCallDetails` with the call id.
4. For topic, contact, company, message text, or broad lookup, use `search` first, then `getCallDetails` for any specific call.

Use the transcript/details returned by `getCallDetails`. Do not mention backend field names to the user.

## Manage caller follow-up

Use CRM tools when the user wants to follow up on callers, leads, tasks, notes, or customer files.

Read-only CRM tools may appear with snake_case names:
- `get_contact` - one contact record by contactId.
- `get_contact_timeline` - notes, calls, SMS, tasks, and file events for a contact.
- `list_customer_files` - files attached to a contact/customer.
- `list_tasks` - follow-up tasks.

Write CRM tools require explicit confirmation:
- `create_contact_note`
- `create_upload_link`
- `update_task_status`
- `update_contact`

For text conversations, use `listTextConversations` and
`getTextConversation` for read-only inbox access. Use `sendCallFollowUp` for an
approved email or SMS tied to an inbound call, or `sendSms` for an eligible
one-off SMS. Direct replies inside an arbitrary existing thread remain in the
SkipCalls app because that path requires first-party chat approval state.

## Useful tool map

Orientation:
- `getOverview` - product and MCP operator overview. Call once first.
- `statusReport` - read-only health report and setup issues.

Receptionists:
- `listAgents` - list receptionists and ids.
- `getAgent` - full config for one receptionist; supports `format: "json"` or `"markdown"` when exposed by the client.
- `updateAgent` - update only approved fields.
- `getAvailableVoices` - list voice options.

Calls:
- `scheduleCall` - place or schedule a one-time outbound call.
- `cancelCall` - cancel a scheduled outbound call.
- `getCallHistory` - list inbound and outbound calls.
- `getCallDetails` - details and transcript for one call.
- `search` - search contacts, calls, and SMS conversations.
- `updateOutbound` - edit the goal of a queued outbound call.
- `reportCallIssue` - record one privacy-safe issue on a reviewed call.

Contacts and CRM:
- `addToContacts` - create or update a contact by phone.
- `get_contact`, `get_contact_timeline`, `list_customer_files`, `list_tasks` - read CRM context.
- `create_contact_note`, `create_upload_link`, `update_task_status`, `update_contact` - confirmed CRM writes.

Appointments:
- `findCalendarSlots` - find real availability in connected calendars.
- `bookCalendarSlot` - book one exact returned slot after confirmation.
- `findAppointments` - find appointments already tracked by SkipCalls.
- `cancelAppointment` - cancel one exact returned appointment after confirmation.

Conversations and follow-up:
- `listTextConversations` - list recent SMS, email, and website-chat threads.
- `getTextConversation` - read one bounded page without marking it read.
- `sendCallFollowUp` - send an approved email or SMS tied to one completed inbound call linked to a contact.

Agent Functions:
- `manageAgentFunctions` - list functions; read the Code guide; read or test a Code function; or create, update, and attach approved HTTP or Code functions. Call `action: "guide"` before drafting Code and `action: "get_code"` before editing it.

Setup:
- `manageBusinessProfileQA` - business profile facts used during calls.
- `manageAgentTasks` - structured call objectives.
- `manageCalendar` - connected calendar settings and event list.
- `manageTransferNumbers` - live handoff destinations.
- `getCarrierForwardingInfo` - verified carrier forwarding setup with the user's SkipCalls number filled in.
- `runForwardingTestCall` - approved automated forwarding verification call.
- `manageNotificationSettings` - read or update owner notification preferences.
- `searchHelpCenter` - SkipCalls help/docs search.
- `updateTimezone` - update the user's stored timezone after confirmation.
- `sendSms` - send confirmed SMS.

Not available through SkipCalls MCP:
- recurring call schedules
- public phone-number lookup
- connecting new calendar OAuth providers
- knowledge-base content read/write, although `updateAgent` can change knowledge access scope
- replying directly inside an existing SMS/email/website-chat thread
- team admin

## Response style

Talk like an operator helping a business owner, not like a telephony engineer.

Use short summaries. For approvals, be precise:

```text
I can schedule this call:
- Receptionist: Ara
- To: Luigi's Italian Restaurant, +14155551234
- When: now
- Goal: ask whether they have a table for 2 this Saturday after 7 PM and book it if available.

Confirm and I will place it through SkipCalls.
```

For setup changes, summarize the exact fields conceptually:

```text
I will update the receptionist to:
- collect caller name, phone, preferred service, and preferred appointment window
- avoid quoting final prices
- send a short post-call text with the booking link

Confirm and I will save these SkipCalls settings.
```
