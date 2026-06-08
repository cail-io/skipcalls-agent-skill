# SkipCalls Agent Skill

Use SkipCalls from AI agents that support Agent Skills and MCP. SkipCalls gives
small businesses a 24/7 AI receptionist that answers calls, captures caller
details, filters spam, books appointments on connected calendars, transfers
urgent callers, sends summaries, and places approved follow-up calls.

This repository contains the `skipcalls-receptionist` skill for skills.sh. It
teaches agents how to connect to the SkipCalls MCP server and safely operate
receptionists, calls, contacts, calendars, transfer numbers, SMS, and business
profile Q&A.

## Install

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

## Links

- Product: https://skipcalls.com
- App: https://app.skipcalls.com
- MCP server URL: `https://be.skipcalls.com/mcp`
- Repository: https://github.com/cail-io/skipcalls-agent-skill
