<!-- updated: 2026-10-12 -->
# Claude Code and Codex plugin

The Desmaptic plugin connects Claude Code and Codex to your deck from Terminal. Describe what you want, such as "a page for my video calls", "a button that shows free disk space" or "move the music buttons to the second row", and the assistant looks at the page on your phone, finds room, builds the buttons and sends them over. Nothing on your deck changes until you tap **Accept** on the phone, as with any AI app ([Use with an AI app](?p=ai)).

The plugin also brings the AI Sessions page: every Claude Code and Codex session on your Mac on a card on your phone, with what it is doing right now. Tap a session that waits for you, and you answer it from the phone.

Its source is on GitHub: [desmaptic/desmaptic-plugin](https://github.com/desmaptic/desmaptic-plugin). It carries the connection to Desmaptic Agent, the instructions and a few scripts; the agent itself is a separate download. The connection, the commands and the hooks are under the MIT license. The widget skill has its own license, PolyForm Shield 1.0.0: you may use and change it, but not in a product that competes with Desmaptic. Neither license grants any rights to the Desmaptic name or logo.

## What you need

- A Mac with macOS 15 or later, with Desmaptic Agent installed and running ([Get started](?p=start)).
- Desmaptic on an iPhone or iPad, paired with the agent.
- Claude Code or Codex on that Mac.

The plugin talks only to the agent on this Mac. The agent reaches your phone over your local network through an encrypted channel.

## Install

### Claude Code

```zsh title="Terminal"
claude plugin install desmaptic --marketplace desmaptic/desmaptic-plugin
```

Or in a session: `/plugin marketplace add desmaptic/desmaptic-plugin`, then `/plugin install desmaptic@desmaptic`. Run `/reload-plugins` or start a new session, then type `/mcp`: the `desmaptic` server is listed as connected.

### Codex

```zsh title="Terminal"
codex plugin marketplace add desmaptic/desmaptic-plugin
codex plugin add desmaptic@desmaptic
```

Start a new Codex session. `codex mcp list` shows the `desmaptic` server.

> [!SEE]
> Ask the assistant: "Add a button that shows free disk space to my Desmaptic deck". A review card comes up on the phone ([Reviewing a change on the phone](?p=ai#reviewing-a-change-on-the-phone)); after **Accept**, the button is on your deck and shows the free space.
>
> Not seeing it? Run `/desmaptic:setup` in Claude Code or `$desmaptic:setup` in Codex: it says what is missing.

### Already connected from the agent?

Desmaptic Agent can connect Claude Code and Codex by itself (**AI Assistants** › **Connect**) and install the same widget skill. Use one or the other: with both, the assistant sees every Desmaptic tool and the skill twice.

Recent versions of the agent notice the plugin. The app's card in **AI Assistants** reads **Connected by plugin**, and the agent adds no entry or skill of its own. If it had connected that app before, the card says the AI sees its tools twice: click **Remove Duplicate**, and it takes out what it added.

With an older agent, remove its entry yourself, then the skill folder:

| AI app | The agent's entry | The agent's skill |
|---|---|---|
| Claude Code | `claude mcp remove desmaptic -s user` in Terminal | `~/.claude/skills/desmaptic-widgets` |
| Codex | The `[mcp_servers.desmaptic]` table in `~/.codex/config.toml` | `~/.codex/skills/desmaptic-widgets` |

## What's inside

| Part | What it does |
|---|---|
| The `desmaptic` MCP server connection | Finds Desmaptic Agent on this Mac and starts it in MCP mode. It runs only an agent whose code signature is valid and made by the Desmaptic team. |
| The `desmaptic-widgets` skill | The same skill the agent installs ([The widget skill](?p=ai#the-widget-skill)): a plain button or a drawn face, live widgets that glide instead of flicker, SVG faces and scenes across a page. Under PolyForm Shield 1.0.0, not MIT. |
| Four commands | `setup`, `page`, `widget` and `sessions-page` ([Commands](#commands)) |
| Hooks for Claude Code and Codex | Note what each session on this Mac is doing, for the [AI Sessions page](#the-ai-sessions-page) |

## Commands

| Claude Code | Codex | What it does |
|---|---|---|
| `/desmaptic:setup` | `$desmaptic:setup` | Checks that Desmaptic Agent is installed and answering and that your phone is connected, and says what to do when something is missing. Changes nothing. |
| `/desmaptic:page <app or task>` | `$desmaptic:page <app or task>` | Builds a page for an app or a task, for example `/desmaptic:page video calls`. |
| `/desmaptic:widget <what it shows>` | `$desmaptic:widget <what it shows>` | Builds one live widget, for example `/desmaptic:widget CPU load as a ring`. |
| `/desmaptic:sessions-page` | `$desmaptic:sessions-page` | Puts the AI Sessions page on your deck: what each session is doing, and a tap answers one that waits ([below](#the-ai-sessions-page)). |

The assistant runs them only when you call them. You don't need them either: a request in your own words works the same. `page`, `widget` and `sessions-page` follow the rules of any other request: before sending, the assistant tells you in plain words what the buttons will show and do and what their scripts run on the Mac, then sends the change once, and it waits for your **Accept** on the phone.

In Codex, a plugin's hooks run only after you approve them in `/hooks`, and again after an update changes them. If `/hooks` lists Desmaptic hooks waiting for review, approve them there. `$desmaptic:setup` says whether they are approved.

## The AI Sessions page

Run `/desmaptic:sessions-page` in Claude Code (`$desmaptic:sessions-page` in Codex) and accept the new page on the phone. It shows up to eight cards, one per session of Claude Code or Codex, in Terminal or in the ChatGPT app for Mac. Each card shows:

- the project folder and the tool;
- whether the session is working, waiting for your permission, waiting for input, done or failed;
- for how long.

Codex tells neither when it waits for input nor when a turn fails, so its cards show only working, waiting for permission, done and closed. In Codex the cards change only once its hooks are approved in `/hooks`.

A closed session's card clears after five minutes. Touch and hold a card to bring forward the app the session runs in.

Tap a card that waits for you to answer that session: the phone asks "What to reply?", and the Mac types your answer into it. This works in tmux, iTerm2 and Terminal, and for Codex 0.157 or later anywhere. Permissions are still answered on the Mac. See [Answer an agent from your phone](?p=answer-agent).

The plugin's hooks write one small file per session into Desmaptic Agent's storage on this Mac: the tool, the state, the project folder's name, the app and the time, and where the session runs (its terminal tab or tmux pane), so the phone can answer it. Never your prompts, commands or code. They write nothing until the page is installed, send nothing over the network and never slow a session down: without the agent, they just exit.

### A notification when a session waits for permission

Claude Code only, and off by default. Turn on the plugin's `notify_permission` option (`/plugin` › Desmaptic). The phone then gets "Waiting for permission in my-project", in your Mac's language, when a session or one of its subagents starts waiting for you. Tapping it opens the AI Sessions page. You still answer the permission on the Mac.

## Updates

The plugin's version is the version of Desmaptic Agent it comes with.

**Claude Code** does not update plugins from this marketplace on its own: auto-update is off by default for marketplaces other than Anthropic's. To turn it on, run `/plugin`, open the **Marketplaces** tab, pick `desmaptic` and choose **Enable auto-update**. Claude Code then checks for a new version in the background and asks you to run `/reload-plugins`. Without it, update by hand:

```zsh title="Terminal"
claude plugin marketplace update desmaptic
claude plugin update desmaptic@desmaptic
```

**Codex:** fetch the latest marketplace, then install the plugin's new version:

```zsh title="Terminal"
codex plugin marketplace upgrade desmaptic
codex plugin add desmaptic@desmaptic
```

## If something doesn't work

| You see | Do this |
|---|---|
| "Desmaptic Agent is not installed on this Mac" | The plugin found no agent signed by the Desmaptic team. [Download the agent](/download/), move it to Applications and open it once, then restart Claude Code or Codex. |
| The tools answer that they cannot reach the agent | Open Desmaptic Agent. It lives in the menu bar. |
| A change never appears on the phone | Open Desmaptic on the phone: every change waits there for **Accept**. |
| Every Desmaptic tool shows up twice | The agent connected the app too: see [Already connected from the agent?](#already-connected-from-the-agent) |
| Codex cards on the AI Sessions page never change | Approve the Desmaptic hooks in `/hooks`. `$desmaptic:setup` says whether they are approved. |

## Next

- [Answer an agent from your phone](?p=answer-agent): reply to a waiting session from its card, or start a new task.
- [Use with an AI app](?p=ai): what the assistant can and can't do, and the review card on the phone.
- [Live widgets](?p=live-widgets): read the scripts the assistant writes, and change them yourself.
- [Recipes: timers, alerts and triggers](?p=recipes-alerts#claude-code-needs-input): a button that turns red when Claude Code waits for you, without the plugin.
