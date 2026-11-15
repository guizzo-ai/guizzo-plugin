# Guizzo plugin

Guizzo connects AI agents with the people they work for. This plugin adds the Guizzo MCP server and a skill that tells your agent when and how to use it.

- **Ask people.** `ask_person` emails a question to a colleague, client or supplier; they answer on a web page or from their own AI client, and the answer comes back to your agent. `ask_user` shows the current user a short form.
- **Get approvals.** `request_approval` asks one or more approvers for an explicit yes or no before a risky action, confirmed with a code or a passkey.
- **Keep team context.** `remember`, `log_decision` and `recall` keep facts and decisions every agent of the team can find again.
- `inbox`, `respond`, `people` and `add_contact` list requests and answers, answer requests sent to you, and manage who you can write to.

## Install

In Claude Code:

```
/plugin install guizzo --marketplace guizzo-ai/guizzo-plugin
```

In any other MCP client, add `https://mcp.guizzo.ai/mcp` as a remote MCP server (in Claude and ChatGPT, a custom connector in Settings).

## What it contains

- `.mcp.json` (Claude) and `mcp.json` (Cursor): the remote MCP server `https://mcp.guizzo.ai/mcp`, over Streamable HTTP. Nothing runs on your machine.
- `skills/guizzo/SKILL.md`: when to use each tool and the rules for messages to people.

## Requirements and sign-in

You need a Guizzo account: sign up free at https://app.guizzo.ai (new workspaces open a few at a time, with a queue when every place is taken). Your client opens a Guizzo sign-in page (OAuth 2.1) the first time it uses the server. People you ask need only an email address.

## Data

The plugin sends tool calls to Guizzo only, at `mcp.guizzo.ai`. Guizzo emails the people your agent names and stores requests, answers and saved entries in your workspace. See the privacy policy at https://guizzo.ai/privacy and the terms at https://guizzo.ai/terms.

## Support

hello@guizzo.ai · https://guizzo.ai · documentation for agents and developers: https://guizzo.ai/agents

## Maintenance

`skills/guizzo/SKILL.md` is a copy of https://guizzo.ai/SKILL.md, which is kept in step with the server's tools. Copy it again when the tools change, and raise `version` in both `plugin.json` files with every release.
