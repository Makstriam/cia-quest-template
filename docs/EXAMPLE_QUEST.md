# How to Design a Quest — Feature Walkthrough

> A practical guide through every part of the site, with notes on how each section can be used in a real quest.  
> This is not a fixed scenario — it's a tour of the tools available to you.

---

## Dashboard

![Dashboard screenshot](screenshot.png)
<!-- Replace with a screenshot of the main page -->

The dashboard is the first thing players see. It sets the tone for the entire quest.

**Pinned messages** appear as a feed of transmissions from different characters — a director, an analyst, a field agent. Use them to:
- Brief players on their mission at the start
- Give context clues or lore as the quest progresses
- Create atmosphere through the tone and language of the "senders"

Each message has a sender name, rank, date, body text, and a colour-coded badge (`amber` for briefing, `green` for confirmed intel, `red` for alert or warning). The order, content, and number of messages are fully configurable.

**Progress indicators** on the dashboard show how many folders are locked, unlocked, and the total count. The access level bar at the bottom of the sidebar updates automatically as folders are opened.

---

## Case Files

![Case Files screenshot](screenshot-folders.png)
<!-- Replace with a screenshot of the folders view -->

Case Files are the core of the quest. Each folder is a locked container of evidence — text documents, photos, audio recordings, video footage, or PDF files.

Players must enter a password to unlock each folder. The password is whatever you decide — it can be:
- Hidden somewhere in the physical room
- Embedded in another folder's content
- Given as a reward for solving a puzzle
- Spoken by a live actor at the right moment

Each folder has a **hint** — a short line shown in the password prompt. Use it to point players in the right direction without giving the answer outright. There is no attempt limit, so players can guess freely.

**Supported file types inside a folder:**

| Type | Use case |
|---|---|
| `image` | Photos of physical evidence, crime scenes, portraits, maps |
| `video` | Security footage, recorded messages, atmospheric cutscenes |
| `audio` | Voice recordings, intercepted transmissions, ambient sound puzzles |
| `pdf` | Documents, reports, dossiers, letters |
| `note` | Inline text — no file needed, written directly in the config |

All files open in a built-in overlay viewer — players never leave the terminal interface.

---

## Terminal Log

![Terminal Log screenshot](screenshot-terminal.png)
<!-- Replace with a screenshot of the terminal view -->

The Terminal Log is mostly atmospheric, but it has practical uses too.

The **static history** at the top (configured in `TERMINAL_HISTORY`) shows events that happened before the players arrived — previous logins, access attempts, error messages. Use it to tell a backstory, plant a clue, or imply that something went wrong before the session began.

Below it, **live session events** are logged automatically as players interact — unlocking folders, running commands, failed password attempts. This creates a natural record of what the group has done, useful if they lose track of their progress.

At the bottom is the **command line**. Players can type commands to interact with the terminal. Public commands include `help`, `ping`, `whoami`, `status`, and `ls`. You can add custom commands with their own replies — useful for Easter eggs, hidden clues, or lore.

The most important command is `restore` — it opens the authentication quiz in a new tab.

> The `override` command is hidden from the help list and from players. It gives the host full control: unlock folders, reveal passwords, skip the quiz, reset the session. Communicate this to the host before the quest starts.

---

## Authentication Quiz

![Quiz screenshot](screenshot-quiz.png)
<!-- Replace with a screenshot of quiz.html -->

The quiz is a separate page (`quiz.html`) launched via the `restore` terminal command. It presents players with a series of questions and a countdown pressure of ambient sounds.

**Structurally:** players answer questions one by one. Up to 2 wrong answers are allowed before the protocol resets and they must start over. Passing the quiz can be the logical end of the quest, or a mid-point gate — on completion, the page shows a success screen and links back to the main terminal.

**Questions** are fully configurable — write them to test knowledge players have gathered during the quest: a code found in a folder, a name from a document, a detail from a video. The quiz is the payoff for engaging with the evidence.

### Sound system — live actor cues and atmosphere

The quiz has a scripted sound system. Sounds can serve two very different purposes:

**1. Live actor cues** — if your quest uses live performers, each sound can be a cue for a specific actor. Sound 1 fires, Actor A enters. Sound 2 fires, Actor B enters. The sequence is scripted and precise.

```js
const SOUND_FILES = [
  'sounds/actor_a_cue.wav',   // sound 1 → Actor A
  'sounds/actor_b_cue.wav',   // sound 2 → Actor B
];
```

Set the number of entries in `SOUND_FILES` to match the number of actors — or cues — you need. Add as many as required; remove entries you don't need. If you want no sounds at all, set `SOUND_FILES = []`.

**2. Atmospheric pressure** — ambient sounds (static, rumbles, distant tones) play at random intervals to build tension during the quiz, without cuing any real-world action.

**3. Mixed** — combine both: some sounds are actor cues (played once at scripted times), others are atmospheric (played randomly throughout). The `SOUND_SCRIPT` defines the scripted phase; everything else loops randomly.

---

## Progress and Notes

The **access level indicator** in the sidebar shows how many folders have been unlocked as a fraction and as a dot-bar. It updates automatically — no configuration needed.

The **Agent Notes** area at the bottom of the sidebar is a free-form text field. Players can use it to jot down codes, names, or anything else they want to remember. Notes persist across page refreshes via `localStorage`.

---

## System Messages and Ticker

The **system message panel** (lower left sidebar) displays short lines of text in rotation with a typing animation. These can be:
- Atmospheric filler ("Scanning network interfaces...")
- Hints dropped mid-quest ("WARN: check the second drawer")
- Lore or character voice ("ERR: AGENT_7714 has not checked in")

The **ticker** at the top of the page is a scrolling banner. Edit it to match your quest's tone — operation codename, classification level, or any repeating atmospheric phrase.

Both are configured in `index.html` and documented in [`MANUAL.md`](MANUAL.md).

---

## Typical quest flow

```
Players arrive and see the dashboard
  → read pinned briefing messages
  → note the access level: 0 / N folders open

Players explore the terminal log
  → read the backstory
  → discover the `restore` and `override` commands

Players attempt to unlock Case File folders
  → find passwords in the room / in other files
  → open media evidence inside each folder

Players run `restore`
  → quiz opens in a new tab
  → sounds begin playing (actor cues or atmosphere)
  → players answer questions based on evidence gathered

Players pass the quiz
  → success screen appears
  → session complete
```

This is a template — cut steps, reorder them, or add your own branches. The site is the stage; you write the story.
