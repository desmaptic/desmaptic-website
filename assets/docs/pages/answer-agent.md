<!-- updated: 2026-10-12 -->
# Answer an agent from your phone

Claude Code or Codex stops and waits for you: the plan is ready, a step is done, it has a question. With the [Desmaptic plugin](?p=plugin), the **AI Sessions** page on your phone shows which session waits. Tap its card, pick "Continue" or say what to do next, and your Mac types the answer into that session, as if you had typed it at the keyboard.

You can also start a new task from the phone with the **Task for an Agent…** button ([below](#start-a-new-task-from-the-phone)). Approving what the agent does stays on the Mac ([What it doesn't do](#what-it-doesnt-do)).

## What you need

- The [Desmaptic plugin](?p=plugin#install) in Claude Code or Codex, and the **AI Sessions** page on your deck: run `/desmaptic:sessions-page` in Claude Code (`$desmaptic:sessions-page` in Codex) and tap **Accept** on the phone.
- The session runs in tmux, iTerm2 or Terminal. A Codex session can also run elsewhere ([Where it works](#where-it-works)).
- Desmaptic on the phone and Desmaptic Agent on the Mac recent enough for [buttons that ask](?p=buttons-that-ask).

> [!NOTE]
> An **AI Sessions** page added with an older plugin only brings the session's app forward when you tap a card. [Update the plugin](?p=plugin#updates), add the page again and remove the old one.

## Answer a waiting session

> [!STEP] 1 · Find the card that waits for you
>
> Open the **AI Sessions** page. Each card shows the project folder, the tool, the state and for how long. A Claude Code session that waits for you is purple, **Waiting for you**. Codex never reports that it waits, so a Codex session ready for its next message shows green **Done**.
>
> The cards you can answer are **Waiting for you**, **Done** and **Error**.

> [!STEP] 2 · Tap the card and answer
>
> The phone asks "What to reply?". Tap a quick reply, **Continue**, **Run the tests** or **Open a PR**, and it goes at once. Or type or dictate your own answer and tap **Send**.
>
> The card that opens shows "AI session", not the project. Check the project on the page before you tap.

> [!STEP] 3 · Watch the card
>
> The Mac types your answer into that session and presses Return. The session gets to work.
>
> Done when the card turns blue, **Working**.

The card works like any [button that asks](?p=buttons-that-ask#what-you-see-on-the-phone): swipe it down or tap ✕ to cancel, and nothing reaches the Mac. Touch and hold a session's card to bring forward the app it runs in, without answering.

## Where it works

The Mac types into the terminal the session started in, so the answer goes only where the phone can type:

| Runs in | Claude Code | Codex |
|---|---|---|
| tmux | Yes | Yes |
| iTerm2 | Yes | Yes |
| Terminal | Yes | Yes |
| Ghostty | No | 0.157+ |
| VS Code or Cursor terminal | No | 0.157+ |
| Warp, kitty, other terminals | No | 0.157+ |
| The Claude app | No | — |
| The ChatGPT app | — | 0.157+ |

Codex 0.157 and later takes the answer through its own `codex queue` command, wherever the session runs. The Mac looks for the `codex` command only in `/opt/homebrew/bin`, `/usr/local/bin` and `~/.local/bin`, where Homebrew and Codex's own installer put it. A `codex` installed with npm under nvm, Volta or `~/.npm-global` is not found there, even when `codex --version` in Terminal works. To see the one the Mac uses, run `/opt/homebrew/bin/codex --version` (or the same in the other two folders).

The first time the Mac types into iTerm2 or Terminal, macOS asks whether Desmaptic Agent may control that app. Click **Allow**.

## What the Mac types

- **One line, then Return.** Line breaks in your answer become spaces, so the agent gets the whole answer as one message.
- **Plain text.** The Mac only types it; nothing on the Mac runs it, and keys like Escape can't be typed through it. The session reads it as if you had typed it, so a `/command` works there as at the keyboard.
- **Only into the session on that card,** and only while it still waits. The Mac checks again right before typing; a session that started working in the meantime gets nothing.
- **Kept nowhere by Desmaptic.** Not in Desmaptic Agent, the plugin's files or a log. The session keeps it as it keeps anything you type, in its history and the terminal's scrollback.

## When nothing is typed

When the Mac can't answer, it types nothing: the card flashes red and a toast says why.

| The toast says | What to do |
|---|---|
| "The session is working — answer once it stops" | Wait until the card shows **Waiting for you** or **Done**. |
| "It asks for permission — answer at the Mac" | The session's app comes forward on the Mac. Permissions are answered there only. |
| "This terminal can't be answered from here" | The session runs where the phone can't type: Ghostty, the terminal of VS Code or Cursor, the Claude app or another terminal. For Codex it can also mean the Mac found no `codex` 0.157 or later in the folders where it looks: update Codex, or install it with Homebrew ([Where it works](#where-it-works)). Otherwise answer at the Mac. |
| "The card has just switched to another session — check it and answer again" | In the last five minutes this card started showing another session, so your answer may have been meant for the earlier one. Check the project on the card and answer again. |
| "The session has ended or left its terminal" | The session closed, or its tool is no longer the program running in that tab. |
| "The session's terminal is not known yet — it says on its next step" | The session has done nothing since the plugin was updated. Answer at the Mac this time; after its next step the phone can answer it. |
| "No permission to control iTerm2" (or Terminal) | You chose **Don't Allow** earlier. Turn Desmaptic Agent's switch for that app on in **System Settings › Privacy & Security › Automation**. |
| "Can't find this session's window in Terminal" (or iTerm2), "Terminal is not running" | The tab or the app was closed. |
| "There is no session on this card" | The card is empty. Pick a card that shows a session. |

## Start a new task from the phone

The **Task for an Agent…** button in the Gallery, under **Ask First**, asks "What should the agent do?" and starts Claude Code or Codex with your words as the first message, in a new window or tab of Terminal, iTerm2, Ghostty or tmux. Its quick replies are **Fix the failing tests**, **Update the dependencies and run the tests** and **Review the last commit**.

The agent works in a worktree of its own: it may commit, never push. Without settings it works in the folder of your last Claude Code or Codex session, else of the front terminal or Finder window, with Claude Code (else Codex), in the terminal of your last session (else Terminal). To choose them yourself, set the folder, the tool and the terminal on the first lines of the button's **Command** ([Buttons that ask › Ready-made in the Gallery](?p=buttons-that-ask#ready-made-in-the-gallery)).

The new session gets a card on the **AI Sessions** page, and you answer it from there. The card may be named after the session's worktree, such as `task-261012-143000`, rather than the project. Pick Terminal, iTerm2 or tmux for Claude Code: a Claude Code session in Ghostty can't be answered from the phone.

> [!SEE]
> Tap **Task for an Agent…**, say "Fix the failing tests" and tap **Send**. A new terminal window or tab opens on the Mac with the task, and a new card shows up on the **AI Sessions** page. When it turns **Waiting for you**, tap it and answer.
>
> Not seeing it? A toast after the tap says what is missing, such as the project folder.

## What it doesn't do

- **No approving from the phone.** When the agent asks for permission to run a command or edit a file, the card shows orange **Needs permission**, and you answer at the Mac. Answer that card from the phone, and the session's app comes forward on the Mac instead. In Claude Code, the plugin can [send a notification](?p=plugin#a-notification-when-a-session-waits-for-permission) when that happens.
- **No notification when a session waits for your answer.** You see it on the **AI Sessions** page while Desmaptic is open on the phone.
- **No agent's question on the card.** A card shows the project, the tool, the state and the time, never what the agent wrote: people next to you can see your phone. Answer from the phone when you know what the session waits for, such as "Continue" after a plan you have read, and read longer questions at the Mac.

## Next

<div class="link-cards">

- [Claude Code and Codex plugin](?p=plugin#the-ai-sessions-page) — install the plugin and the AI Sessions page.
- [Buttons that ask](?p=buttons-that-ask) — the card on the phone and the Gallery's other asking buttons.
- [Recipes: timers, alerts and triggers](?p=recipes-alerts#claude-code-needs-input) — a button that turns red when Claude Code waits for you, without the plugin.

</div>
