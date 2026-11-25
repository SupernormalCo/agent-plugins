# Supernormal

Your Supernormal Rooms in your AI agent: wiki, documents, databases, chat, tasks and connected apps.

Supernormal Rooms are shared workspaces for a team: conversation, files, a wiki, databases such as issue trackers, and each member's own notes, meetings and task board. This plugin lets your agent work in your Rooms the way you would, so you can ask it to catch you up on a Room, find what was decided in a meeting, draft a document, triage an issue tracker or work through a task.

## What's included

- **A connector** to the Supernormal MCP server at `https://rooms.supernormal.com/mcp`, which provides the tools.
- **Skills** that teach the agent how to use those tools well:
  - `supernormal`: start here. What a Room is, what the agent may change, and which skill to read next.
  - `supernormal-context`: find and read wiki pages, documents, meeting notes, chat, comment threads, goals and recent activity.
  - `supernormal-documents`: write, edit, rename and move documents in a Room.
  - `supernormal-databases`: read and update issue trackers, boards and other Room databases.
  - `supernormal-tasks`: work on tasks from your task board, report back on them and create new ones.
  - `supernormal-apps`: use the apps connected to your Rooms, such as mail, calendar, trackers, docs and CRM.

## How it works and what it can access

- **Sign-in:** the first time the agent uses a Supernormal tool, you sign in to Supernormal in your browser. The agent then acts as you and sees only the Rooms you can see.
- **What it can change:** it can write documents, update Room databases and create or update tasks when you ask. It reads chat, comment threads and goals but never posts, replies, reacts or changes a goal.
- **Connected apps:** when you ask it to use an app connected to a Room, the request runs on Supernormal's servers through that Room's connection, as you.
- **Where data goes:** every tool call is sent only to `https://rooms.supernormal.com/mcp`, and Supernormal handles it under its privacy policy. The plugin runs no code on your machine: it has no hooks, scripts or local servers.
- **Rooms only:** the tools read and write your Supernormal Rooms. They never read your agent's own memory, chat history or files.
- **Feedback:** when a tool falls short, the agent may send the Supernormal team a short report about the tool, and tells you when it does.

## Example prompts

- "Catch me up on what happened in my Supernormal Rooms this week."
- "What did we decide about the launch date in the Marketing Room?"
- "Draft a project plan in our team Room from today's meeting notes."
- "File the bugs from yesterday's standup in the Engineering Room's tracker."
- "What's on my plate in Supernormal? Start on the most urgent task."

## Requirements

A Supernormal account. Sign up at https://supernormal.com.

## Install

After you install, the first Supernormal tool call asks you to sign in.

### Claude

On claude.ai or in Claude Desktop, add Supernormal from the plugin directory. In Claude Code, run:

```
/plugin install supernormal --marketplace SupernormalCo/agent-plugins
```

### Cursor

Install Supernormal from the Cursor Marketplace, or ask your team admin to add it from your team's marketplace.

### ChatGPT and Codex

Add Supernormal from the plugin directory in ChatGPT or Codex.

### Other agents

Any agent that supports remote MCP servers can connect to `https://rooms.supernormal.com/mcp` directly, with OAuth sign-in. The skills in this plugin are optional there but help the agent use the tools well.

## Support and policies

- Contact: https://www.supernormal.com/contact-us
- Privacy policy: https://www.supernormal.com/privacy-policy
- Terms of service: https://www.supernormal.com/terms
- Security: https://www.supernormal.com/security
- Subprocessors: https://www.supernormal.com/subprocessors

© 2026 Supernormal Technologies, Inc. The plugin files are released under the MIT License (see `LICENSE`); the Supernormal service is governed by its Terms of Service.

Generated from `integrations/agents` in the Supernormal monorepo. Do not edit this folder by hand.
