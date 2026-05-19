# Installing the EA Handoff Skill

Your Silent Partners SE has sent you a folder called `ea-handoff-llm-agent` (or similar). This document walks you through the one-time setup to install it on your machine so Claude Code can use it.

You only need to do this once. After that, when you're ready to write your handoff document, you can just ask Claude Code something like "help me write the handoff doc for this POC" and the skill activates automatically.

If anything below doesn't work, message your SE and they'll get you unstuck — this takes about two minutes when it goes smoothly.

---

## What you need before you start

- The skill folder from your SE (a folder named like `ea-handoff-llm-agent`, containing a `SKILL.md` file inside)
- Claude Code installed and working on your machine
- About five minutes

---

## Step 1: Find your Claude Code skills folder

Claude Code looks for skills in a specific folder on your computer. The location depends on your operating system.

**On Mac or Linux:** The folder is `~/.claude/skills/`. If it doesn't exist yet, you'll create it in the next step.

**On Windows:** The folder is `C:\Users\YourName\.claude\skills\`. If it doesn't exist yet, you'll create it in the next step.

**On Windows with WSL (Windows Subsystem for Linux):** Use the Mac/Linux instructions inside your WSL terminal. Don't put the skill on the Windows side — it works but is slower.

---

## Step 2: Create the skills folder if it doesn't exist

Open a terminal (Terminal app on Mac, PowerShell on Windows, or your WSL terminal).

**Mac, Linux, or WSL:**
```
mkdir -p ~/.claude/skills
```

**Windows PowerShell:**
```
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills"
```

If the folder already exists, these commands do nothing harmful. Safe to run either way.

---

## Step 3: Copy the skill folder into the skills folder

Take the skill folder your SE sent you (named something like `ea-handoff-llm-agent`) and move it inside the skills folder you just made sure exists.

The final structure must look like this:

```
~/.claude/skills/
└── ea-handoff-llm-agent/
    └── SKILL.md
```

The `SKILL.md` file must be one level deep inside the skill folder, not two. If you accidentally end up with `~/.claude/skills/ea-handoff-llm-agent/ea-handoff-llm-agent/SKILL.md`, Claude Code will not find it. Fix this by moving the inner folder up one level.

**The easiest way to verify it's right:**

```
ls ~/.claude/skills/ea-handoff-llm-agent/
```

(or `dir` on Windows PowerShell instead of `ls`)

You should see `SKILL.md` listed directly. If you see another folder name instead, the skill is one level too deep — move the contents up.

---

## Step 4: Restart Claude Code

Claude Code loads skills when a session starts, so if you had Claude Code running, exit it and start a new session.

---

## Step 5: Confirm it's installed

In your new Claude Code session, type:

```
/skills
```

You should see `ea-handoff-llm-agent` in the list along with a short description. If you see it, you're done.

If you don't see it, the most common causes are:

- The folder is nested one level too deep (see Step 3)
- The file is named something other than `SKILL.md` exactly (capital letters matter)
- You're in a Claude Code session that started before you installed the skill — restart it

If none of those is the issue, send your SE a screenshot of the output of `ls ~/.claude/skills/ea-handoff-llm-agent/` (or the Windows equivalent) and they'll diagnose from there.

---

## Using the skill

Once installed, you don't need to do anything special to trigger it. When you're working in the Claude project that contains your POC and you're ready to start the handoff document, say something like:

- "Help me write the handoff document for this POC"
- "I need to take this to our enterprise architect"
- "Can you draft the EA handoff doc based on what we built?"

Claude Code will pick up the skill, read your project context, draft the document, and ask you any clarifying questions it needs along the way. When the draft is solid, it'll offer to export the final document as a Word file for you to send to EA.
