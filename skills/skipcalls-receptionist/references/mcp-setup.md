# SkipCalls MCP Setup

Use this reference when SkipCalls MCP tools are missing or the user asks how to connect SkipCalls to an AI agent.

## Connection facts

- Product: https://skipcalls.com
- App: https://app.skipcalls.com
- MCP server URL: `https://be.skipcalls.com/mcp`
- Authentication: OAuth through the user's SkipCalls account. Do not ask for raw API keys for normal MCP setup.
- Requirement: an active SkipCalls subscription on the service. New users can start with a free trial at https://app.skipcalls.com.
- The MCP server exposes tools for receptionists, calls, contacts/CRM, calendars, transfer numbers, business profile Q&A, SMS, status reports, help search, and call forwarding instructions.

## ChatGPT setup

ChatGPT custom MCP apps are web-only and gated by plan/workspace settings. Full MCP with write/modify actions is currently for ChatGPT Business, Enterprise, and Edu workspaces. Pro users may be limited to read/fetch custom apps. Workspace admins or owners may need to enable Developer mode and approve/publish the app before members can use it.

1. Open ChatGPT in a desktop browser.
2. Enable Developer mode if your plan/workspace requires it.
3. From the workspace or user app settings, create a new custom app/connector.
4. Set:
   - Name: `SkipCalls`
   - MCP server URL: `https://be.skipcalls.com/mcp`
   - Authentication: OAuth
5. Scan tools. If OAuth opens, sign in with the same account used at https://app.skipcalls.com and finish the authorization flow.
6. Create the app/connector. If this is a workspace app, publish or approve it according to the workspace's ChatGPT controls.
7. In a new chat, enable the SkipCalls app/connector before asking for call or receptionist work.

If tool definitions change later, a ChatGPT workspace admin may need to refresh or republish the app actions before new tools appear.

## Claude setup

1. Open Claude.
2. Go to Settings -> Connectors.
3. Add a custom connector.
4. Name it `skipcalls`.
5. Paste the remote MCP server URL: `https://be.skipcalls.com/mcp`.
6. Approve the SkipCalls OAuth flow.
7. Enable the connector in the conversation.

## Cursor setup

Use Cursor's MCP settings UI or config file.

```json
{
  "mcpServers": {
    "skipcalls": {
      "url": "https://be.skipcalls.com/mcp"
    }
  }
}
```

Approve the SkipCalls OAuth popup on the first tool call.

## First test after connecting

Ask the agent:

```text
Use SkipCalls MCP. Call getOverview, listAgents, and summarize my recent inbound calls before suggesting any changes.
```

Expected first tools: `getOverview`, then `listAgents`. For recent inbound calls, the agent should use `getCallHistory` with `type: "INCOMING"`.

If `getOverview` works but other tools are missing, use the client's tool discovery/search UI. SkipCalls registers the tools on the server.

If OAuth fails, ask the user to sign in to the same SkipCalls account they use at https://app.skipcalls.com.

If the MCP client reports `client_not_allowlisted`, the user should contact SkipCalls support with the MCP client name and redirect URI host.

## What this MCP can and cannot do

Can:
- list and configure AI receptionists
- schedule one-time outbound calls
- list inbound and outbound calls
- open one call's transcript/details
- manage contacts/CRM follow-up context when CRM tools are exposed
- manage business profile answers, tasks, connected-calendar settings/events, transfer numbers, SMS, help search, and forwarding instructions

Cannot through MCP:
- manage recurring call schedules
- directly create or cancel calendar appointments
- connect new calendar OAuth providers
- edit knowledge base files or saved knowledge notes
- perform public phone-number lookup
- manage full SMS inbox/thread state
- manage team admin

When a missing capability is needed, explain that it must be done in the SkipCalls app or with the user's own external tools.
