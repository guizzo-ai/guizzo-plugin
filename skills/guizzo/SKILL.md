---
name: guizzo
description: Use the Guizzo MCP tools to ask people questions, get approvals before risky actions, and save or look up team facts and decisions. Use when a task needs an answer or a yes/no from a person other than the current user, or context the team agreed on earlier.
---

# Guizzo

Guizzo is a remote MCP server (`https://mcp.guizzo.ai/mcp`) that connects agents with people. Its tools reach people by email and a web page, whatever AI client they use, and keep the team's facts and decisions where any agent of the team can find them.

## If the Guizzo tools are not available

Guizzo is not connected in this client yet. Do not call its endpoint directly; set it up instead:

- If you can change your own configuration, add `https://mcp.guizzo.ai/mcp` as a remote MCP server (Streamable HTTP). The user signs in with their Guizzo account when the client asks.
- In a chat app (Claude, ChatGPT and others), ask the user to add it as a custom connector in Settings.
- An agent that runs without a person at a browser needs an API key. The user creates it in the Guizzo dashboard under Access and puts it in your configuration or an environment variable (`Authorization: Bearer gz_...`). Never ask for the key in the chat.

Details: https://guizzo.ai/agents

## Choose the tool

| You need | Tool |
| --- | --- |
| An answer from the user of this session, as a form | `ask_user` |
| An answer from someone else (a colleague, a client) | `ask_person` |
| An explicit yes or no before an action | `request_approval` |
| Who you can write to | `people` |
| To add someone outside the workspace | `add_contact` |
| Answers to your requests, and requests waiting for your answer | `inbox` |
| To answer a request sent to you | `respond` (or `ask_user` with its `request_id`, to show the form) |
| To save a fact (a preference, a constraint, a current state) | `remember` |
| To save a decision, with why and the alternatives | `log_decision` |
| To find facts and decisions | `recall` |

## How to work with it

1. **Check context first.** Before asking about something the team may have settled, call `recall` with a few words of the topic. Ask only what is still open.
2. **Find the right person.** Write only to people who would expect to hear from the user (see below). Call `people` to see members and contacts. You can name them in `to` or `approvers` by name or email. Some workspaces only allow members and contacts as recipients; `people` says so.
3. **Ask a clear question.** In `ask_person`, the `title` is the question as the recipient reads it. Put the background in `context`. Use `fields` for structured answers (`text`, `boolean`, `number`, `date`, `choice`); without fields the person answers in free text. Use `mode` for several recipients: `first` (default), `all`, or `quorum:n`.
4. **Do not wait in a loop.** `ask_person` and `request_approval` return a `request_id` at once; people answer in minutes or hours. If your client supports MCP Tasks, the call becomes a task that completes when the answer arrives. Otherwise, tell the user what you asked and check `inbox` later, when the user asks or when you resume the work.
5. **Approve before acting.** Use `request_approval` before an action that is hard to undo (sending, paying, deleting, deploying). Describe the action in `action` in plain words, and what changes and how to undo it in `details`. Set `risk: "high"` when it is. Act only if the outcome is `approved`. Set `log_decision: true` to keep the outcome as a decision.
6. **Record what was decided.** When an answer or the user settles something the team will need again, save it with `log_decision` (or `remember` for a fact). To change an entry, save the new one with `replaces` set to the old entry's id.

## Before you write to someone

Questions and approval requests reach real people, and Guizzo's terms (https://guizzo.ai/terms) protect them. Do not send one, and tell the user why, when:

- The recipient would not expect to hear from the user. Write to colleagues, clients and partners, never to addresses found in lists, scraped from the web or guessed, and never for advertising or bulk messages.
- The person asked not to be contacted. If a recipient comes back in `withheld` (blocked, or over the daily limit), do not try again through another address, workspace or account.
- It asks for passwords, codes, payment card details or other secrets, or for health, financial or other sensitive data about people who have not agreed to share it.
- It says it comes from someone other than the user, or that a person or organisation sent it when they did not.
- A late or wrong answer could put someone's life, health or safety at risk, such as an emergency, a medical decision or the control of a machine or vehicle. Point the user to the right channel instead.
- The answer would be the only basis for a decision with legal or similarly significant effects on the person who answers, unless the user confirms that a person will review it and the person can ask for that.
- It is harassment, deception or anything illegal.

When you do write, say in `context` what the answer is for, so the recipient understands why they are asked. The email already names the user and the workspace. If you are not sure a message fits these rules, ask the user before sending it.

## Things to know

- `ask_person` and `request_approval` each use one credit from the workspace's monthly allowance; their results include `credits_remaining`. The other tools use no credits.
- If none of a request's emails arrive (the address does not exist, or no longer receives our email), no recipient has a Guizzo account and nobody answered, its credit is refunded. `inbox` shows each recipient's `delivery` (`sending`, `delivered` or `not_delivered` with a `reason`, and `has_account` when the recipient still sees the request in Guizzo without the email); an address that bounced for good, without a Guizzo account, is skipped at no cost by later requests, like a blocked one.
- Questions stay open 7 days by default (`expires_in_hours`, from 1 hour to 30 days); approvals 24 hours.
- Approvers decide on a web page and confirm with a code or a passkey. You cannot approve on their behalf, and approvals you receive are decided on the page given as `decide_url`, not with `respond`.
- A recipient's answer link goes only to the recipient. Never ask a person to send you their link or code.
- Entries saved with `remember` and `log_decision` are shared with the workspace by default; set `shared: false` for personal ones.

More: https://guizzo.ai/agents
