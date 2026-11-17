# Handoff Helper for Claude (vn-handoff-helper)

Long chats cost more usage. This plugin hands a long chat over to a fresh one so you keep working without starting from zero.

## What it does
1. Names chats in a clear pattern: `PROJECT | TOPIC | V#`.
2. Writes a short handoff note (goal, done, open, next step, file locations).
3. Asks you to type OK. Then it opens a new Claude desktop chat with a start prompt and presses Enter for you.
4. The new chat renames itself through Claude in Chrome, reads the note and continues.

## What it needs
- Windows.
- The Desktop Commander connector (to run the small launch script).
- Claude in Chrome (to rename the new chat). If missing, the new chat gives you its name to paste as the title.

## What it runs
One short PowerShell script, stored in the plugin as `launch_handoff.ps1.txt` so it cannot run by accident. Claude shows it to you and copies it to your Documents\ClaudeSetup\scripts folder only after you agree, and runs it only after you type OK. It opens a `claude://` link, waits 8 seconds, brings the Claude window forward and presses Enter. Read it before you use it; it is about 15 lines.

## What it sends
Nothing leaves your PC. It does not read your chats.

## Known limits
The link cannot set the chat title, so the rename step is needed. Leave the keyboard alone for about a minute after OK. If no Claude window is found, press Enter yourself.

Created by Soroosh Hedayat

Designed in Collaboration by: VectorNine Systems Inc. & Acid Secs Productions Inc.

## Data handling

It writes handoff notes (a summary of your chat) to your own PC or Drive. A short script is stored as a .txt file. Claude shows it to you and copies it only after you agree, and runs it only after you type OK. Nothing is sent to the authors of this plugin. We run no server and receive no data, so nothing is retained by us. The plugin sends nothing to any outside service. Everything it makes stays on your own PC or in your own Google Drive and Claude account, and you can delete it any time.
