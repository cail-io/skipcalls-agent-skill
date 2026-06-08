# Public Design Notes

Date: 2026-06-08

This file explains the public packaging and product choices behind the
`skipcalls-receptionist` skill. It is safe to publish with the skill package.

## Skills.sh Packaging Facts

- skills.sh skills are installed with the `skills` CLI. The public docs show
  `npx skills add <owner>/<repo>` and GitHub URLs as install sources.
- Public listings appear automatically through anonymous aggregate telemetry
  after users install a repository with `npx skills add <owner/repo>`.
- A custom skill should live in a GitHub repository with a skill definition file
  and a README that explains usage.
- Root `skills.sh.json` customizes the skills.sh listing page. It does not
  change the instructions inside `SKILL.md`.

## Skill Design

The skill should not teach users to assemble telephony infrastructure. It should
teach an AI agent to use SkipCalls as the product layer:

- SkipCalls handles phone numbers, answering, outbound calling, transcripts,
  summaries, calendars, SMS, transfers, and receptionist runtime behavior.
- The assistant uses the SkipCalls MCP server to read current state, draft
  changes, ask for confirmation, and call write tools only after approval.
- `getOverview` is the live MCP orientation document for current tool names,
  schemas, limits, and product vocabulary.
- The skill keeps public user-facing language product-oriented: receptionist,
  caller, transfer, booking, contact, task, summary.

## Core Workflows

1. Call `getOverview` first.
2. Read with `listAgents`, `getAgent`, `getCallHistory`, `getCallDetails`, `statusReport`, and `search`.
3. Use CRM read tools when available: `get_contact`, `get_contact_timeline`, `list_customer_files`, and `list_tasks`.
4. Write only after explicit confirmation with `updateAgent`, `scheduleCall`, `cancelCall`, `addToContacts`, `sendSms`, `updateTimezone`, calendar/task/business-profile/transfer write actions, and CRM write tools.
5. For outbound calls, confirm phone number, receptionist, goal, and local scheduled time before `scheduleCall`.
6. For inbound calls, use `getCallHistory` with `type: "INCOMING"` and drill into selected calls with `getCallDetails`.
7. For configuration, keep receptionist instructions business-specific and avoid generic assistant behavior.

## Product Boundaries

- MCP supports one-time outbound calls. Recurring call scheduling is not exposed
  through MCP.
- MCP can list existing calendar events and update connected calendar settings,
  but it does not directly create or cancel calendar appointments.
- Knowledge-base content is not readable or editable through MCP. `updateAgent`
  can adjust which knowledge an agent may access, not the uploaded files or
  saved notes themselves.
- SMS is limited to search and confirmed sending. Full SMS inbox/thread
  management stays in the SkipCalls app.
- Public phone-number lookup is outside SkipCalls MCP. If the host agent has a
  web/search tool and the user asks for lookup, the agent should show the source
  and confirm the number before scheduling a call.

## Sources reviewed

- https://www.skills.sh/docs
- https://www.skills.sh/docs/cli
- https://www.skills.sh/docs/api
- https://www.skills.sh/docs/faq
- https://www.skills.sh/docs/customize
- https://skills.sh/moltbot/moltbot/voice-call
- https://skills.sh/elevenlabs/skills/agents
- https://skills.sh/cartesia-ai/skills/line-voice-agent
- https://skills.sh/skillhq/concierge/concierge
- https://skills.sh/smithery/ai/cli
- https://skills.sh/agentiveau/myagentive/twilio-phone
