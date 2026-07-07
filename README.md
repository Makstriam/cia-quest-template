# CIA Secure Terminal — Quest Template

> An interactive detective quest template with a CIA terminal aesthetic.  
> Drop in your own files, set passwords, and run a quest — no coding required.

![screenshot](docs/screenshot.png)
<!-- Replace with an actual screenshot before publishing -->

---

## Contents

- [Features](#features)
- [Getting started](#getting-started)
- [How it works](#how-it-works)
  - [Dashboard](#dashboard)
  - [Case Files](#case-files)
  - [Terminal Log](#terminal-log)
  - [Authentication Quiz](#authentication-quiz)
  - [Progress and Notes](#progress-and-notes)
  - [System Messages and Ticker](#system-messages-and-ticker)
- [How to customize](#how-to-customize)
- [File structure](#file-structure)
- [License](#license)

---

## Features

- **Works offline** — open `index.html` directly in any browser, no server needed
- **No coding required** — all content is configured through simple text arrays in the file
- **Any language** — write quest content in English, Russian, German, or any other language
- **CIA / CRT aesthetic** — green-on-black terminal UI with scanlines and typing animations
- **Password-locked case file folders** — players unlock folders one by one by solving clues
- **Built-in media viewer** — display images, video, audio, and PDF files inside the terminal
- **Authentication quiz** (`quiz.html`) — Q&A terminal that players must pass to gain access
- **Scripted sound system** — cue live actors or build atmosphere with timed ambient sounds
- **GM override commands** — host-only terminal commands to unlock, lock, hint, or reset on the fly
- **Agent Notes** — players can type notes directly in the sidebar; saved across page refreshes

---

## Getting started

1. Download or clone this repository
2. Open `index.html` in Chrome, Firefox, or Edge
3. That's it — the demo quest is ready to play

> No npm, no Python, no server. Double-clicking the file is enough.

---

## How it works

### Dashboard

![Dashboard](docs/screenshot-dashboard.png)

The dashboard is the first thing players see. It sets the tone for the entire quest.

**Pinned messages** appear as a feed of transmissions from different characters — a director, an analyst, a field agent. Use them to brief players on their mission, give context clues, or build atmosphere through the tone and language of the "senders". Each message has a sender name, rank, date, body text, and a colour-coded badge (`amber` for briefing, `green` for confirmed intel, `red` for alert).

**Progress indicators** show how many folders are locked and unlocked. The access level bar updates automatically as folders are opened.

---

### Case Files

![Case Files](docs/screenshot-folders.png)

Case Files are the core of the quest. Each folder is a locked container of evidence that players unlock with a password.

The password can be hidden anywhere: in the physical room, inside another folder, given as a reward for solving a puzzle, or spoken by a live actor at the right moment. Each folder has a **hint** shown in the password prompt — use it to point players in the right direction. There is no attempt limit.

**Supported file types inside a folder:**

| Type | Use case |
|---|---|
| Image | Photos of physical evidence, crime scenes, portraits, maps |
| Video | Security footage, recorded messages, atmospheric cutscenes |
| Audio | Voice recordings, intercepted transmissions, sound puzzles |
| PDF | Documents, reports, dossiers, letters |
| Note | Inline text written directly in the config — no file needed |

All files open in a built-in overlay viewer — players never leave the terminal interface.

---

### Terminal Log

![Terminal Log](docs/screenshot-terminal.png)

The terminal log has two layers.

The **static history** (configured in `TERMINAL_HISTORY`) shows events that happened before the players arrived — previous logins, access attempts, error messages. Use it to plant a clue or imply that something went wrong before the session began.

**Live session events** are logged automatically as players interact — unlocking folders, running commands, failed password attempts. This creates a natural record of progress.

At the bottom is the **command line**. Players can type commands to interact with the terminal. Public commands include `help`, `ping`, `whoami`, `status`, and `ls`. Custom commands with their own replies can be added — useful for Easter eggs, hidden clues, or lore.

The most important command is `restore` — it opens the authentication quiz in a new tab.

The `override` command is hidden from the help list and invisible to players. It gives the host full control: unlock folders, reveal passwords, skip the quiz, reset the session.

---

### Authentication Quiz

![Authentication Quiz](docs/screenshot-quiz.png)

The quiz is a separate page (`quiz.html`) opened via the `restore` terminal command. Players answer a series of questions; up to 2 wrong answers are allowed before the protocol resets. On passing, a success screen appears and links back to the main terminal.

Write questions that test knowledge gathered during the quest — a code found in a folder, a name from a document, a detail from a video. The quiz is the payoff for engaging with the evidence.

**Sound system — live actor cues and atmosphere**

The quiz plays sounds on a configurable timer. Sounds can serve different purposes:

- **Live actor cues** — each sound is a signal for a specific performer. Sound 1 fires, Actor A enters. The sequence is scripted and precise. Set the number of sounds to match the number of actors.
- **Atmospheric pressure** — ambient sounds (static, rumbles, distant tones) play at random intervals to build tension without cueing any real-world action.
- **Mixed** — some sounds are scripted actor cues, others loop randomly throughout.

To disable sounds entirely, set `SOUND_FILES = []`.

---

### Progress and Notes

![Sidebar](docs/screenshot-sidebar.png)

The **access level indicator** in the sidebar shows unlocked folders as a fraction and as a dot-bar. It updates automatically.

The **Agent Notes** area is a free-form text field. Players can jot down codes, names, or anything they want to remember. Notes persist across page refreshes.

---

### System Messages and Ticker

The **system message panel** displays short lines of text in rotation with a typing animation — atmospheric filler, mid-quest hints, or character voice lines. All configurable.

The **ticker** at the top is a scrolling banner. Edit it to match the quest's tone: operation codename, classification level, or any repeating atmospheric phrase.

---

### Typical quest flow

```
Players arrive → read pinned briefing on the dashboard
  → explore the terminal log and discover commands
  → find passwords (in the room, in files, from actors)
  → unlock Case File folders and examine evidence
  → run `restore` → quiz opens with ambient sounds / actor cues
  → answer questions based on gathered evidence
  → pass the quiz → success screen → quest complete
```

This is a template — cut steps, reorder them, or add your own branches.

---

## How to customize

Open `index.html` in any text editor and search for the config keywords below. No programming knowledge is required — all settings are plain text lists.

| What to change | Search for |
|---|---|
| Folder names, passwords, hints | `FOLDERS_CONFIG` |
| Briefing messages on the dashboard | `PINNED_MESSAGES` |
| Terminal backstory / log | `TERMINAL_HISTORY` |
| Ambient system messages in the sidebar | `YOUR_MESSAGES` |
| Easter egg terminal commands | `SIMPLE COMMANDS` |
| Quiz questions and answers | `QUESTIONS` (in `quiz.html`) |
| Sound sequence during the quiz | `SOUND_SCRIPT` (in `quiz.html`) |

Add your media files to the `media/` folder and reference them in the `FOLDERS` config. All five types are supported: image, video, audio, PDF, and inline text notes.

Full reference: [`docs/MANUAL.md`](docs/MANUAL.md)  
Demo walkthrough with passwords and answers: [`docs/DEMO.md`](docs/DEMO.md)

---

## File structure

```
/
├── index.html       — the entire site (dashboard, folders, terminal, media viewer)
├── quiz.html        — authentication terminal with quiz and sound system
├── media/           — your images, videos, audio, and PDFs
├── sounds/          — audio files for the quiz sound system
└── docs/
    ├── MANUAL.md    — full configuration reference
    ├── DEMO.md      — demo passwords, quiz answers, and GM commands
    └── screenshot.png
```

---

## License

MIT — use it, modify it, run your own quests.
