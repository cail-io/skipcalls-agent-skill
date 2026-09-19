# OpenAI Plugins Directory submission

Use submission type **With MCP** and include the bundled
`skills/skipcalls-receptionist` skill.

## Public listing

- Plugin name: AI Receptionist - SkipCalls
- Short description: Operate your AI receptionist from ChatGPT and Codex.
- Long description: Review calls and conversations, configure receptionists,
  manage contacts and calendars, and send approved follow-ups through your
  SkipCalls account.
- Category: Productivity
- Website: https://skipcalls.com
- Support: https://skipcalls.com/contact
- Privacy policy: https://app.skipcalls.com/privacy
- Terms: https://app.skipcalls.com/terms
- MCP URL type: Universal
- MCP server: https://be.skipcalls.com/mcp
- Authentication: OAuth 2.0 with PKCE and dynamic client registration
- Countries: select only countries where SkipCalls currently supports service
  and support coverage.

## Starter prompts

1. Show my receptionists and summarize recent inbound calls.
2. Review my receptionist setup and suggest improvements.
3. Show recent customer conversations that need follow-up.

## Tool annotation rationale

- Read-only account tools use `readOnlyHint=true`, `openWorldHint=false`, and
  `destructiveHint=false` because they only retrieve data from the authenticated
  SkipCalls workspace or bounded SkipCalls help/catalog data.
- Additive workspace writes use `readOnlyHint=false`, `openWorldHint=false`, and
  `destructiveHint=false` because they add an internal record without replacing
  or deleting existing state.
- Workspace updates and deletes use `readOnlyHint=false`,
  `openWorldHint=false`, and `destructiveHint=true` because they can overwrite,
  cancel, or remove authenticated workspace state.
- Booking an appointment uses `readOnlyHint=false`, `openWorldHint=true`, and
  `destructiveHint=false` because it creates external calendar state that can be
  cancelled later.
- Calls and outbound messages use `readOnlyHint=false`, `openWorldHint=true`,
  and `destructiveHint=true` because they contact an external person and cannot
  be recalled after delivery.

## Release notes

Initial SkipCalls plugin submission. The plugin bundles the
`skipcalls-receptionist` operating skill and the production OAuth MCP server.
The MCP surface contains 45 annotated operational tools for receptionists,
calls, contacts, appointments, calendars, conversations, follow-ups,
first-time onboarding, billing and Stripe-hosted subscription checkout,
notifications, Agent Functions, forwarding, SMS, and product help.

## Portal-only prerequisites

These cannot be stored in the public repository:

1. Select the verified SkipCalls business identity in the same OpenAI
   organization and project used for submission.
2. Confirm the submitter has **Apps Management: Write**.
3. Provide a reviewer demo account with representative sample data and no MFA,
   SMS, email-confirmation, or private-network dependency.
4. Complete the portal-generated domain challenge at
   `https://be.skipcalls.com/.well-known/openai-apps-challenge` only after the
   portal provides the exact token.
5. Upload the final production logo and submit the positive and negative cases
   in `test-cases.md`.

Do not submit until the backend MCP parity change is deployed and a fresh
**Scan Tools** reports all 40 tools with the expected annotations.
