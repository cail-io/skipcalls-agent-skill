# SkipCalls Agent Plugin

Use SkipCalls from ChatGPT, Codex, and other agents that support Agent Plugins,
Agent Skills, or MCP. SkipCalls gives
small businesses a 24/7 AI receptionist that answers calls, captures caller
details, filters spam, books appointments on connected calendars, transfers
urgent callers, sends summaries, and places approved follow-up calls.

This repository is a portable Agent Plugin. It bundles the
`skipcalls-receptionist` skill with the production SkipCalls MCP server, so a
compatible host can load the workflow guidance and authenticated tools
together. The skill teaches agents how to safely operate
receptionists, calls, contacts, calendars, transfer numbers, SMS, and business
profile Q&A.

## Requirement

SkipCalls MCP requires an existing SkipCalls account with access to the
service. Product and account information is available at https://skipcalls.com.

## Install

Install the complete plugin from the ChatGPT/Codex Plugins Directory after its
public listing is approved. For direct Agent Skills installation during
development:

```bash
npx skills add cail-io/skipcalls-agent-skill --skill skipcalls-receptionist
```

Direct GitHub URL also works:

```bash
npx skills add https://github.com/cail-io/skipcalls-agent-skill --skill skipcalls-receptionist
```

## What The Skill Helps With

- Configure SkipCalls AI receptionists without rewriting business-specific
  instructions into generic prompt filler.
- Schedule one-time outbound calls after the user confirms recipient, number,
  call goal, receptionist, and local call time.
- Review inbound call lists, missed calls, call summaries, recordings, and
  transcripts through SkipCalls MCP.
- Manage follow-up context with contacts, notes, tasks, customer files, SMS, and
  transfer destinations where the MCP client exposes those tools.
- Set up the remote MCP server in ChatGPT, Claude, Cursor, and other MCP
  clients.

The bundled MCP connection uses OAuth and exposes the current operational tool
surface documented by `getOverview`. The external agent should call
`getOverview` once at the start of a session before using other SkipCalls
tools.

## Links

- Product: https://skipcalls.com
- App: https://app.skipcalls.com
- MCP server URL: `https://be.skipcalls.com/mcp`
- Repository: https://github.com/cail-io/skipcalls-agent-skill
