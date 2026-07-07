# CIA SECURE TERMINAL — MANUAL
> Configuration guide for customising the template for your own quest

---

## Quick navigation
Open `index.html` in any editor (VS Code, Notepad++) and use **Ctrl+F** to search for the keywords from this manual.

---

## File structure

```
quest/
├── index.html        ← entire site in one file
├── quiz.html         ← authentication terminal 
└── media/
    ├── photo.jpg     ← your images
    ├── video.mp4     ← your videos
    └── ...
```

---

## 1. CASE FILE FOLDERS
**Search:** `FOLDERS_CONFIG`

Each folder is one object `{ }` in the `FOLDERS` array. Add or remove as many as you need.

```js
{
  id:             "folder_01",                  // unique ID, latin letters, no spaces
  name:           "CASE FILE #01",              // displayed name
  desc:           "Short description.",         // subtitle under the name
  classification: "TOP SECRET",                 // badge in the corner of the card
  password:       "mypassword",                 // password to unlock
  hint:           "Hint shown to the player.",  // text in the password prompt
  files: [
    { name: "photo.jpg",  path: "media/photo.jpg",  type: "image" },
    { name: "video.mp4",  path: "media/video.mp4",  type: "video" },
  ]
}
```

**Add a folder** — copy the entire `{ ... }` block and paste it after a comma.  
**Remove a folder** — delete the `{ ... }` block along with its comma.  
**Add a file** — add a line to `files: [ ]`. The file must be in the `media/` folder.

> ⚠️ After adding or removing folders, refresh the page (F5).

---

## 2. PINNED MESSAGES ON DASHBOARD
**Search:** `PINNED_MESSAGES`

A feed of messages from different characters. The first is usually the main briefing, followed by agent notes, analyst reports, etc.

```js
{
  from:     "DIRECTOR HARRIS",          // sender name
  rank:     "LEVEL 4 CLEARANCE",        // rank / clearance level
  date:     "06/07/2026  09:14 UTC",    // date (any string)
  tag:      "MISSION BRIEFING",         // badge label
  tagColor: "amber",                    // colour: "amber" | "green" | "red"
  text:
`Message body here.
Can be multi-line.`
}
```

**Add a message** — copy the `{ ... }` block and paste it after a comma.  
**Remove** — delete the block.  
**Badge colours:** `"amber"` — yellow, `"green"` — green, `"red"` — red.

---

## 3. TERMINAL HISTORY (backstory)
**Search:** `TERMINAL_HISTORY`

Static text in the Terminal Log — events that happened before the player arrived.

```js
{ t: "cmd",  text: "ls /classified/" }           // green prompt + command
{ t: "out",  text: "Session started — OK" }       // light green output
{ t: "info", text: "WARN: unusual packet" }        // yellow — warning
{ t: "err",  text: "ACCESS DENIED — 3 attempts" } // red — error / alert
{ t: "sep" }                                       // horizontal divider
```

**Realistic session line template:**
```js
{ t: "out", text: "[05/07/2026  22:31:08]  AGENT_NAME  @ 10.0.0.44  →  UNLOCKED FOLDER_02" }
{ t: "err", text: "[05/07/2026  23:14:07]  UNKNOWN     @ 45.33.32.156  →  ACCESS DENIED" }
```

---

## 4. SYSTEM MESSAGES (live panel in sidebar)
**Search:** `YOUR_MESSAGES`

Messages appear one by one in random order with a typing animation. Use for hints, jokes, or atmospheric clues.

All messages live in a single `ALL_SYS_MESSAGES` array. The built-in atmospheric lines are at the top — your custom ones go at the bottom, marked with a comment. You can leave everything as-is, add your own lines, replace built-in ones, or remove them entirely.

```js
const ALL_SYS_MESSAGES = [
  { text: "Scanning network interfaces...", cls: ""     },  // built-in (green)
  { text: "WARN: unusual packet — ...",     cls: "warn" },  // built-in (yellow)
  { text: "ERR: ping timeout — node ...",   cls: "err"  },  // built-in (red)
  // ...more built-in lines...

  // ── YOUR MESSAGES — add hints, clues, jokes, lore ──
  { text: "Check the second drawer.",     cls: ""     },  // green
  { text: "WARN: you missed something.",  cls: "warn" },  // yellow
  { text: "ERR: trust no one.",           cls: "err"  },  // red
];
```

---

## 5. SIMPLE TERMINAL COMMANDS (Easter eggs)
**Search:** `SIMPLE COMMANDS`

Player types a word — gets a reply. Great for hints, lore, or jokes.

```js
{ cmd: "atlas",  reply: "Project ATLAS is classified.",  t: "warn" },
{ cmd: "ghost",  reply: "No such agent on record.",      t: "err"  },
```

Reply types: `"out"` — green, `"info"` — yellow, `"err"` — red.

---

## 6. SITE HEADER
**Search:** `CIA SECURE TERMINAL`  (in the HTML, inside `<div class="header">`)

```html
<div class="header-logo">▓ CIA SECURE TERMINAL</div>
<div class="header-sub">CLASSIFIED DOCUMENT MANAGEMENT SYSTEM v4.7.2</div>
```

Change the title and subtitle to your own.

```html
<div>OPERATOR: <span class="status-online">FIELD AGENT #7714</span></div>
```

Change the agent name here, or via the `override name NAME` command in the terminal.

---

## 7. TICKER (scrolling banner)
**Search:** `ticker-text`

```html
<span class="ticker-text">
  ⚠ CLASSIFICATION: TOP SECRET ⚠ &nbsp;&nbsp;&nbsp;
  OPERATION CODENAME: [REDACTED] &nbsp;&nbsp;&nbsp;
  ...
</span>
```

Just edit the text inside. `&nbsp;&nbsp;&nbsp;` adds spacing between phrases.

---

## 8. GM COMMANDS (hidden — host only)
**Search:** `override`  (type `override` in the terminal to see the list)

| Command | Action |
|---|---|
| `override unlock 2` | unlock folder #2 without password |
| `override unlock all` | unlock all folders |
| `override lock 2` | lock folder #2 again |
| `override lock all` | lock everything |
| `override hint 3` | show the hint for folder #3 |
| `override passwd 3` | reveal the password for folder #3 |
| `override skip` | unlock everything at once |
| `override reset` | full session reset |
| `override name NAME` | change the operator name in the header |

> This command does not appear in the standard `help` — players won't find it by accident.

---

## 9. MEDIA FILES — how to add

1. Place the file in the `media/` folder next to `index.html`
2. Add a line to `files: [ ]` in the relevant folder:

```js
{ name: "clue.jpg",    path: "media/clue.jpg",    type: "image" }
{ name: "footage.mp4", path: "media/footage.mp4", type: "video" }
```

**Supported formats:**
- Images: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp` → `type: "image"`
- Video: `.mp4`, `.webm` → `type: "video"`
- Audio: `.mp3`, `.wav`, `.ogg` → `type: "audio"` (plays in a built-in player)
- PDF: `.pdf` → `type: "pdf"` (opens in built-in viewer)
- Inline text — no file needed, text stored directly in the config (always works):
  ```js
  { name: "Report", type: "note", content: `Document text here.
  Can be multi-line.` }
  ```
- Text from file: `type: "text"` — works **only when served via a web server**, not by double-clicking the file.

---

## 10. QUIZ.HTML — SOUNDS (scripted sequence)
**Search in quiz.html:** `SOUND_SCRIPT`

### Sound files
Place files in the `sounds/` folder next to `quiz.html` and set the paths:
```js
const SOUND_FILES = [
  'sounds/sound1.wav',
  'sounds/sound2.wav',
  'sounds/sound3.wav',
  'sounds/sound4.wav',
];
```

Add or remove entries freely — the system adapts to whatever number you provide.  
Reference sounds in `SOUND_SCRIPT` by their position: `1` = first entry, `2` = second, and so on.

**To use 2 actors / 2 sounds** — keep only 2 entries:
```js
const SOUND_FILES = [
  'sounds/actor1_cue.wav',
  'sounds/actor2_cue.wav',
];
```

**To disable sounds entirely** — set the array to empty:
```js
const SOUND_FILES = [];
```

### Launch script
`SOUND_SCRIPT` is a list of `[sound number, delay in seconds]` pairs.  
Number is 1-based (1 = first file in `SOUND_FILES`).  
The first entry fires after its delay from start; each subsequent entry fires after the previous one.

```js
const SOUND_SCRIPT = [
  [1, 30],   // +30s from start  — sound 1
  [2, 30],   // +30s after that  — sound 2
  [3, 30],   // +30s after that  — sound 3
  [4, 30],   // +30s after that  — sound 4
  [1, 15],   // +15s after that  — sound 1 again
  [2, 1],    // +1s after that   — sound 2
];
```

After its first scripted play, each sound switches to its own **random timer** and continues independently. Sounds absent from the script go straight to random mode.

### Random mode interval
```js
const RANDOM_MIN = null;  // null = auto-calculated from SOUND_SCRIPT
const RANDOM_MAX = null;  // or set a number in seconds, e.g. 15 and 60
```

**Auto-calculation:** analyses the gaps between repeated appearances of each sound in the script, takes the global min/max and adds a ±20% buffer.

**Example:** script `[1,20],[2,20],[3,20],[1,15],[2,1],[3,1]`  
→ gaps: sound 1 = 55s, sound 2 = 36s, sound 3 = 17s  
→ auto range ≈ 13–66 s

**Manual mode:** set both values explicitly:
```js
const RANDOM_MIN = 15;  // minimum pause, seconds
const RANDOM_MAX = 60;  // maximum pause, seconds
```

---

## 11. MINOR SETTINGS

| What | Where to look | How to change |
|---|---|---|
| Typing speed of system messages | `setTimeout(typeChar, 55)` | value in ms — higher = slower |
| Message rotation interval | `setInterval(rotateSysMsg, 4500)` | value in ms |
| Browser tab title | `<title>CIA SECURE TERMINAL` | edit the text |
| Number of dots in Access Level | automatic | depends on the number of folders in `FOLDERS` |

---

## Quick checklist for a new quest

- [ ] Updated folder names and descriptions
- [ ] Set your own passwords (`// ← CHANGE PASSWORD`)
- [ ] Written password hints (`hint:`)
- [ ] Added files to `media/` and set their paths
- [ ] Written the briefing in `PINNED MESSAGES`
- [ ] Written the backstory in `TERMINAL HISTORY`
- [ ] Added your own system messages to `MY_SYS_MESSAGES`
- [ ] Changed the agent name and title in the header
- [ ] Updated the ticker text (`ticker-text`)
- [ ] Replaced placeholder sounds in `sounds/`
