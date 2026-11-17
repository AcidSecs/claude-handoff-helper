---
name: handoff-helper
description: Hand a long Claude chat over to a fresh chat. Use when the user says "new chat", "hand off", "this chat is getting long", "continue in a new chat", or when a chat passes about 15 exchanges or large pastes. Windows with Desktop Commander and Claude in Chrome.
---

# Handoff Helper

## Step 0: settings
Read `Documents\ClaudeSetup\settings.json`. If `modules.handoff_helper` is false, say "Handoff Helper is off" and stop. If the file is missing, offer the Setup Wizard. Use `naming_style` (default `PROJECT | TOPIC | V#`).

## Chat names
Format `PROJECT | TOPIC | V#`. V# counts handoffs (V1 = first chat). The next chat is the same name with V# plus 1. Example: `SHOP | Price List | V3`.

## What you need
Windows, the Desktop Commander connector (runs the launch script), Claude in Chrome (renames the new chat). If one is missing, say which, and use the fallback ladder below.

## Steps
1. **Write the handoff note.** SHORT: current goal, what is done, what is open, the next step, file locations. First line = the next chat's exact name, then one line explaining the name format. Do not copy old chat text. Save it where the new chat can open it by itself (a file in the user's project folder or in their Drive).
2. **Write the start prompt** to a text file (UTF-8). It must say: "Your exact chat name is: <NAME>", the rename steps below, where the handoff note is, "start your first reply with your exact name in a copy-ready block", and "continue by yourself".
3. **Ask for OK, in bold:** "The handoff is written. The new chat will open and take your screen for about a minute. Please leave the keyboard alone. Type OK when ready." Wait. If the user types anything other than OK / next chat / yes, answer normally and do not fire.
4. **Fire (last action).** With Desktop Commander start_process run:
   `powershell -File "<plugin folder>\skills\handoff-helper\launch_handoff.ps1" -PromptFile "<start prompt file>"`
   The script opens a new chat with the prompt, waits 8 seconds, brings the Claude window forward and presses Enter.
5. **The new chat renames itself** (start prompt steps): load the Claude in Chrome tools in one ToolSearch call; open a new tab on claude.ai; find itself as the newest chat in Recents and check its first message starts with "Your exact chat name is"; three-dot menu, Rename, ctrl+a, type the exact name, Enter; close its tab.

## Fallback ladder for renaming
1. Claude in Chrome. 2. The browser pane inside the Claude desktop app. 3. The new chat shows its name in a copy block and the user pastes it as the title.

## Known limits (tested on Windows)
- The deep link cannot set the chat name or send the message; the app makes its own short title and drops the pipes and V number, so the rename step is needed.
- Only a script on the PC can press Enter; Claude cannot click inside the Claude app.
- A scheduled task cannot start a normal desktop chat; do not use one for this.
- If the user types while the script runs, Enter can land in the wrong place; that is why you wait for OK first.
- If no Claude window is found, the user presses Enter in the new chat.

## Health check
When asked to check: confirm the script file exists, Desktop Commander and Claude in Chrome tools are present, and PowerShell runs. If something fails, search Anthropic's current docs, PROPOSE a fix, and ASK before changing anything.

## Safety
Nothing runs before the user's OK. The script is small and readable: it opens a link, waits, presses Enter. Nothing is sent anywhere.

## Guides for steps the user must do
Whenever the user must do a multi-step job themselves (connect an app, install something, request a download), do not write a long reply. Make a tick-box guide: copy `guide/GUIDE_TEMPLATE.html` (in this plugin's folder) to a new file in the user's Documents folder, change only the GUIDE object near the bottom, and follow the instructions at the top of the template. Make it extremely detailed: about five small steps for each real step, plain English, the reason in brackets, a copy chip for anything to type, and only the steps a human must do. Keep the chat reply short and point to the guide.
