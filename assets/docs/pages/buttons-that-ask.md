<!-- updated: 2026-10-12 -->
# Buttons that ask

A button can ask for a few words before it runs. You tap it, a card opens on the phone with a question, and you type, dictate or pick a ready answer. Your Mac then runs the button's action with that text: it adds a line to your notes, files a bug, opens a search or hands a task to Claude Code.

A name that ends in "…", such as **Note…** or **Bug in Linear…**, means the button asks first, as a menu item with "…" opens a window. Nothing else on the button shows it, so end the names of your own asking buttons the same way.

## What you see on the phone

The card grows out of the button you tapped. It shows the button's icon and its question, a text field, the quick replies, **Paste** and **Send**. On an iPhone held upright the keyboard is already open: type and tap **Send**.

<div class="shot-pair">
<img src="assets/docs/img/buttons-that-ask/card.png" width="240" height="521" alt="The card of a Note button on iPhone: the question “What to note?”, the field with “Pick up the dry cleaning”, the quick replies Buy milk, Call back and Book a table, Paste, Send, and the keyboard open below.">
<img src="assets/docs/img/buttons-that-ask/replies-only.png" width="240" height="521" alt="A Focus button that offers only quick replies: next to the button, a bubble asks “Focus for how long?” with 25 min, 50 min, 1 hour, Until 6 PM and Own Text…; the deck stays visible around it.">
</div>

- **Voice:** tap the microphone on the keyboard and speak. Dictation fills the field; check it and tap **Send**.
- **Quick replies:** a tap on one sends it at once, without the keyboard.
- **Paste** puts in what the phone's clipboard holds, as a start for your text.
- **Return** sends a one-line text. When the button takes several lines (a bug report, a note), Return starts a new line and **Send** sends.
- **Cancel:** swipe the card down or tap ✕. Nothing reaches the Mac.

On an iPad, on an iPhone on its side, and for a button that offers only quick replies, the card is a bubble next to the button and the deck stays visible around it. Tap the field to type; tap outside the bubble to cancel. **Own Text…** at the end of the replies opens the field for anything else.

After **Send** the card shrinks back into the button, and the tap goes on as any tap: success is silent, and an action that fails flashes the button red, with a toast that says why.

| You see | What it means |
|---|---|
| "120 characters left", then "15 characters too many" | A text can be up to 8,000 characters; the count shows near the end. A longer text is not sent, never cut. |
| "Not sent. Check the connection to the Mac and try again." | The phone lost the Mac. Your text stays in the field; tap **Send** again. |
| The button shakes red and a toast says "Update Desmaptic Agent on your Mac" | The agent on the Mac is older than the app and can't take the text yet. |

> [!NOTE]
> What you type stays out of sight: the card does not show in the app switcher, and with **Settings › Security › Hide Code While Recording or Mirroring** on (it is by default), the field is covered while the screen is recorded or mirrored. Tap **Show** to see it anyway.

## When it helps

Typing on the phone makes sense when the text must reach this one Mac, or when the Mac must not lose its focus:

- **An agent is working.** Claude Code runs in a terminal, and you want to note an idea, ask about the project or start a second task without clicking away from its window.
- **The screen is shared.** You are presenting or pairing, and a note or a search should not appear in front of everyone.
- **A call is on.** The call window stays in front while you note what to do afterwards.
- **The text needs the Mac.** A bug report should carry the project's branch and the browser tab that is open on the Mac, a link should open in the Mac's browser, a task belongs in a file on the Mac.

For text that never changes, a **Text Snippet** without a question is one tap. For a choice between two or three fixed actions, give each its own button.

<!-- Anchor used by the iOS app (Gallery: the Learn More… link in the details of a button that asks). Keep this heading text. -->
## Ready-made in the Gallery

The Gallery has 14 buttons that ask, under **Ask First**. The first nine need nothing: add one and tap it. **Ask the Project…**, **To Inbox…** and **Task for an Agent…** work at once too, with defaults you can change. Only the two bug reports need setting up: a Linear key, or the project folder and the GitHub CLI. Open any of them in the editor to read what it does. Settings such as a folder or a file go on the first lines of its **Command**; when the button can't find what it needs, the tap says what to set.

<div class="stack-table">

| Button | It asks | What happens on the Mac | You set up |
|---|---|---|---|
| **Type…** | "What to type on the Mac?" | The text is typed into the app in front, where the cursor is, as if from the Mac's keyboard. | Nothing. The first time, macOS may ask to allow Desmaptic Agent under **Accessibility**. |
| **To the Mac…** | "What to send to the Mac?" | The text goes to the Mac's clipboard. A link alone, starting with `http://` or `https://`, also opens in the default browser. The button shows only a ✓, never the text: it may be a password. | Nothing. |
| **Ask Claude…** | "What to ask Claude?" | A new chat with your question opens in the Claude app, which starts if it is closed. On a Mac without the app, it opens on claude.ai in the default browser. | Nothing. |
| **Ask ChatGPT…** | "What to ask ChatGPT?" | A new chat with your question opens on chatgpt.com in the default browser, signed in there or not. | Nothing. |
| **Search…** | "What to search for?" | Searches Google in a new tab of the default browser. | Nothing. Another search engine: change the button's **URL**. |
| **Reminder…** | "What to remind you of?" | A new reminder in the default list of Reminders, with no date; the first line is its title. | Nothing. The first time, macOS asks to give Desmaptic Agent access to Reminders. |
| **Note…** | "What to note?" | A new note in the default folder of Notes, with the first line as its title. Notes starts if it is closed. | Nothing. The first time, macOS asks to let Desmaptic Agent control Notes. |
| **Email…** | "What to write?" | A draft with the text opens in the default mail app, Mail unless you chose another. You address and send it yourself. | Nothing. Mail without an account asks you to add one first. |
| **Say Aloud…** | "What should the Mac say?" | The Mac says the text aloud, in a voice for its language, up to about a minute of speech. It is also on the **Just for Fun** shelf. | Nothing. |
| **Bug in Linear…** | "What broke?" | The first line becomes the issue's title. The Mac adds the branch and last commit of your project, the front app and the browser tab. The quick reply **+ screenshot** adds the Simulator's screen to the next report. | A Linear API key: the button says how. Your team's key, such as `ENG`, when you have more than one team. Optionally the project folder. |
| **Bug on GitHub…** | "What broke?" | The same, as a GitHub issue in your project's repository. GitHub takes no picture with a new issue, so **+ screenshot** puts it on the Mac's clipboard and opens the issue in the browser: paste it into a comment with ⌘V. | The project folder and the GitHub CLI (`gh`), signed in once with `gh auth login` in Terminal: the button says how. |
| **Ask the Project…** | "What do you want to know?" | The answer comes as a notification and names the project. The ready questions "What did I do yesterday?", "Status of my PRs" and "What isn’t committed?" are answered at once, without AI. Any other question goes to Claude Code, which may only read, for up to 50 seconds. | Nothing. The project is the folder of your last Claude Code or Codex session that the [Desmaptic plugin](?p=plugin) knows and that still runs, else of the front Terminal, iTerm2, Ghostty or Finder window. Set the folder to always ask about one project. |
| **To Inbox…** | "What to note?" | A new reminder in the default list of Reminders. With a Markdown file set, such as your Obsidian inbox or a project's `INBOX.md`, it adds `- [ ] 14:05 your text` at the file's end instead. | Nothing. The first time, macOS asks to give Desmaptic Agent access to Reminders. Optionally the file's path, such as `~/Notes/Inbox.md`. |
| **Task for an Agent…** | "What should the agent do?" | Starts Claude Code or Codex with your task in a new window or tab of your terminal (Terminal, iTerm2, Ghostty or tmux), in a worktree of its own. It may commit, never push. | Nothing. It works in the same folder as **Ask the Project…**, with Claude Code (else Codex), in the terminal of your last session (else Terminal). Set the folder, the tool or the terminal to choose them yourself. |

</div>

With the [Desmaptic plugin](?p=plugin#the-ai-sessions-page), the session that **Task for an Agent…** started gets a card on the **AI Sessions** page. You can answer it from there when it runs in Terminal, iTerm2 or tmux, or when the tool is Codex 0.157 or later; otherwise the card says why and types nothing. How: [Answer an agent from your phone](?p=answer-agent).

<!-- Anchor used by the iOS app (Editor: the Learn More… link on the Ask Before Running screen). Keep this heading text. -->
## Make a button ask

Any button whose tap is a **Shell Command**, **AppleScript**, **Text Snippet**, **Open URL** or **Run Shortcut** can ask. Only the tap asks; a long press runs at once.

> [!STEP] 1 · Open Ask Before Running
>
> Open the button in the editor. Under **Tap**, below **Type** and **Command**, tap **Ask Before Running**. The row reads "Off" until the button asks, then shows the question.
>
> <img src="assets/docs/img/buttons-that-ask/editor-row.png" width="240" height="466" alt="Edit Button for a Note button: under Tap, Type “Shell Command”, the Command row, and Ask Before Running with “What to no…”; Appearance with the Name “Note” below.">

> [!STEP] 2 · Write the question and the replies
>
> Turn on **Ask Before Running** and tap **End the Name with “…”**. Then fill in the card:
>
> - **Question**: what to type, such as "What to note?". Left empty, the card asks "What should this button send?".
> - **Field Hint**: the grey example inside the empty field, such as "A thought, a link…".
> - **Quick Replies**: tap **Add Reply** for each ready answer. **Edit** reorders them.
> - **Multiple Lines**: for notes and bug reports, where Return starts a new line.
> - **Quick Replies Only**: the card opens as a row of your replies by the button, without the keyboard. **Own Text…** still lets you type anything, so the action must handle any text. It needs at least one reply.
>
> **Preview** shows the card as it will look, and the note under it says how your action gets the text.
>
> <img src="assets/docs/img/buttons-that-ask/ask-screen.png" width="240" height="776" alt="The Ask Before Running screen: the switch on, End the Name with “…”, Card with the Question “What to note?” and the Field Hint “A thought, a link…”, Quick Replies Call back, Buy milk and Idea with Add Reply, the switches Multiple Lines and Quick Replies Only, the Preview of the card, and the note “The script reads the text from $DESMAPTIC_INPUT, as data: nothing in it runs.”">

> [!STEP] 3 · Use the text in the action
>
> Go back and make the action read the text, as the next section shows for each action. Tap **Save**.
>
> Done when a tap on the button opens the card, and **Send** runs the action with what you typed.

## How the action gets the text

| Action | Where the text goes |
|---|---|
| **Shell Command** | the environment variable `$DESMAPTIC_INPUT` |
| **AppleScript** | the same variable, read through `do shell script` |
| **Text Snippet** | in place of every `{{INPUT}}` in the snippet, or typed alone when the snippet has none |
| **Open URL** | in place of `{{INPUT}}` in the address, encoded for a link |
| **Run Shortcut** | the shortcut's input |

> [!WARNING]
> The text is always data, never code. Desmaptic hands it over as a value, and nothing in it runs, whatever it contains: quotes, `;`, `$( )`. Keep it that way in your script: no `eval`, no `sh -c "$DESMAPTIC_INPUT"`, no `osascript -e` or JSON built by gluing the text in.

### Shell Command

Read `"$DESMAPTIC_INPUT"`, always in double quotes. This **Note…**, a button of your own like the one in the pictures above, adds what you typed to the end of a Markdown file, as **To Inbox…** does with a file set (the Gallery's **Note…** makes a note in Notes instead):

```zsh title="Shell Command"
# Note: adds the text from the card to ~/Notes/Inbox.md as a task
[[ -n $DESMAPTIC_INPUT ]] || { print -u2 'No text came with the tap'; exit 1; }
print -r -- "- [ ] $(date '+%H:%M') ${DESMAPTIC_INPUT//$'\n'/ }" >> "$HOME/Notes/Inbox.md"
```

`${DESMAPTIC_INPUT//$'\n'/ }` joins several lines into one, so a note stays one line of the list.

Other ways to use the text, each a tap script of its own:

```zsh title="Shell Command"
# onto the Mac's clipboard
print -rn -- "$DESMAPTIC_INPUT" | pbcopy

# as an argument: after --
grep -rl -- "$DESMAPTIC_INPUT" "$HOME/Notes" > "$TMPDIR/found.txt"

# in JSON: let jq quote it
webhook="https://example.com/hook"   # ← your webhook
jq -n --arg text "$DESMAPTIC_INPUT" '{text: $text}' |
  curl -s -m 10 -H 'Content-Type: application/json' --data @- "$webhook"
```

- As an argument of a command, put the text after `--`, so a text that starts with `-` is not read as an option.
- In JSON, let jq quote it with `--arg` ([How scripts run › Text in JSON](?p=scripts#text-in-json)).
- Desmaptic writes nothing in place of `{{INPUT}}` in a script: the editor warns in orange, and an AI app's tools refuse it.

### AppleScript

Read the variable through the shell, then use `t` as a value:

```applescript title="AppleScript"
-- Note in Notes: makes a note with the text from the card
set t to do shell script "printf %s \"$DESMAPTIC_INPUT\"" without altering line endings
tell application "Notes" to make new note with properties {body:t}
```

- `system attribute "DESMAPTIC_INPUT"` reads the text in an old encoding and garbles every letter outside English: use `do shell script` as above.
- To hand `t` to a shell command, pass it only as `quoted form of t`, as in `do shell script "pbcopy <<< " & quoted form of t`, never joined in as it is.

### Text Snippet and Open URL

Write `{{INPUT}}` where the text goes:

| Action | You write | You type "Lisbon" and get |
|---|---|---|
| **Text Snippet** | `Hi {{INPUT}},` and a new line | Hi Lisbon, |
| **Text Snippet** | a snippet without `{{INPUT}}`, such as `Hi there` | Lisbon, typed alone |
| **Open URL** | `https://duckduckgo.com/?q={{INPUT}}` | the search for Lisbon in the default browser |

In an address the text is encoded for a link, so spaces, `&` and `#` stay part of the one value. An address without `{{INPUT}}` drops the text: the editor says so in orange.

### Run Shortcut

The shortcut gets the text as its input, in a text file. In the Shortcuts app, let the shortcut receive Text and Files, and make its first action **Get Text from Input**; the rest of the shortcut uses that text.

### When no text comes

A long press never asks, and a phone with an older Desmaptic runs the tap without the card. A script then finds `$DESMAPTIC_INPUT` not set, and a Text Snippet or an Open URL uses its stored text as it is, `{{INPUT}}` included. Check it first, as the Note button does: the tap fails with your words in the toast instead of writing an empty line.

```zsh title="Shell Command"
[[ -n $DESMAPTIC_INPUT ]] || { print -u2 'No text came with the tap'; exit 1; }
```

## Ask your AI app

Claude Code, Codex or any app [connected to Desmaptic](?p=ai#connect-your-ai-app) can make such a button for you. Say what it should ask and what to do with the answer:

```text title="Prompt"
Make a button that asks what to note and appends it to my INBOX.md in ~/Developer/my-app.
```

The assistant writes the script, ends the name with "…" and words the question. Before anything changes, the phone shows its proposal: "Before running, asks: “What to note?”", the quick replies, and what the tap does. Tap **Accept** to add the button.

<img src="assets/docs/img/buttons-that-ask/review.png" width="280" height="390" alt="The review card “The AI wants to add 2 buttons”: Search… with “Before running, asks: “Search for what?””, the quick replies weather, train times and news, and “Tap: opens https://duckduckgo.com/?q={{INPUT}}”; Type with “Before running, asks for text.” and “Tap: types this text: {{INPUT}}”; at the bottom “Nothing changes until you tap Accept.”">

The assistant sets the card through the button's `input` field: `question`, `placeholder` (the **Field Hint**), `quickReplies`, `multiline` and `quickRepliesOnly`. To change an asking button, ask in the same words: "Add the quick replies Lunch and In a meeting to Status…", or "Make Note… stop asking".

## Next

<div class="link-cards">

- [How scripts run](?p=scripts) — the environment, time limits and errors of every script, `$DESMAPTIC_INPUT` included.
- [Use with an AI app](?p=ai) — connect Claude Code or Codex and let it build buttons.
- [Button logic](?p=button-logic) — long presses, buttons that change each other, one script for several buttons.

</div>
