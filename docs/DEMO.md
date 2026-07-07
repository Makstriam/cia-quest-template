# CIA SECURE TERMINAL — DEMO WALKTHROUGH
> Use this file to verify all site functionality end-to-end.

---

## 1. OPEN THE SITE

Open `index.html` in any modern browser (Chrome, Firefox, Edge).  
No server required — double-click works fine.

---

## 2. CASE FILE PASSWORDS

| Folder | Password |
|---|---|
| CASE FILE #01 — SUSPECT | `hunter2` |
| CASE FILE #02 — EVIDENCE | `shadow9` |

Click a folder card → enter the password → folder opens.

---

## 3. TERMINAL COMMANDS

Type these in the terminal input at the bottom of the **Terminal Log** screen:

| Command | What it does |
|---|---|
| `help` | list available commands |
| `ping` | check connection |
| `whoami` | show current operator |
| `status` | mission progress |
| `ls` | list folders and lock status |
| `restore` | launch the authentication quiz in a new tab |
| `override` | show hidden GM command list |

---

## 4. AUTHENTICATION QUIZ (quiz.html)

Launch via the `restore` command in the terminal, or open `quiz.html` directly.

### Answers to the built-in demo questions:

| # | Question | Answer |
|---|---|---|
| Q1 | Which encryption algorithm is active? | **B** — AES-256 |
| Q2 | Clearance level required for folder #04? | **C** — LEVEL 4 |
| Q3 | Current version of the document management system? | **C** — v4.7.2 |
| Q4 | Number of the agent running the current session? | **B** — #7714 |
| Q5 | How many case file folders are in the system? | **C** — 4 |

> Keyboard shortcuts: press **A**, **B**, or **C** to answer without clicking.  
> Up to **2 wrong answers** allowed before the protocol restarts.

---

## 5. GM OVERRIDE COMMANDS

Type in the terminal — not shown in `help`, invisible to players:

```
override unlock all     — unlock all folders at once
override lock all       — reset all folders to locked
override hint 1         — show password hint for folder #1
override passwd 2       — reveal the actual password for folder #2
override skip           — unlock everything instantly
override reset          — full session reset (locks all, clears log)
override name AGENT_X   — change operator name in the header
```
