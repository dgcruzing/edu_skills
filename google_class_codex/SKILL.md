---
name: google-classroom-debug-editor
description: Inspect, test, and update Google Classroom materials through a user-opened Chrome remote debugging session. Use when Codex needs to work with Google Classroom classwork, material descriptions, assignment instructions, titles, attachments, or visible edit forms in a signed-in Chrome profile, especially when the normal Codex browser is not authenticated. Includes safety checks for temporary Chrome debug mode and shutdown.
---

# Google Classroom Debug Editor

## Purpose

Use this skill to work with Google Classroom pages that are already open in the user's Chrome browser and exposed through Chrome remote debugging. The skill is for legitimate user-directed editing of their own Classroom content; do not use it to bypass access controls or inspect accounts the user has not intentionally opened.

Use Australian English by default for education, VET, RTO, trainer, learner, and course-facing material.

## Safety Rule

Chrome remote debugging gives local control over the open browser profile. Treat it as a temporary work mode.

- Ask the user to close the debugging Chrome window when finished.
- Do not leave `--remote-debugging-port` running after the work is complete.
- Do not save edits unless the user clearly asked for a live update.
- Prefer a visible, reversible test before the first real write.
- Never delete or replace course content unless the user explicitly asks; append or create a new version by default.

## Workflow

### 1. Confirm The Debug Browser Is Running

Tell the user to close normal Chrome instances, then launch a temporary debug profile:

```powershell
& "C:\Program Files\Google\Chrome\Application\chrome.exe" `
  --remote-debugging-port=9222 `
  --user-data-dir="C:\Temp\chrome-codex-debug"
```

The user signs in, opens the target Google Classroom page, and places it in edit mode if editing is required.

Probe the endpoint:

```powershell
Invoke-RestMethod http://127.0.0.1:9222/json/version
Invoke-RestMethod http://127.0.0.1:9222/json/list
```

If this fails, check for an existing Chrome process that was launched without the debug flag. Ask the user to close Chrome and relaunch the temporary profile.

### 2. Find The Target Tab

List page targets and identify the Classroom page by title or URL:

```powershell
$tabs = Invoke-RestMethod -Uri "http://127.0.0.1:9222/json/list" -TimeoutSec 5
$tabs | Where-Object { $_.type -eq "page" -and ($_.url -match "classroom.google.com" -or $_.title -match "Classroom") } |
  Select-Object id,title,url,webSocketDebuggerUrl
```

If multiple Classroom tabs are open, use the material detail URL or title to choose the exact tab.

### 3. Inspect Before Writing

Before editing, inspect visible fields and buttons through the Chrome DevTools Protocol. Look for:

- `textarea` title field.
- `[role="textbox"][aria-label*="Description"]` for the material description.
- `Save` button state.
- Any visible warning, loading state, or disabled control.

Do not assume the page is editable merely because the URL loaded.

### 4. Run A Small Write Test

For first-time access in a session, use a short reversible test line such as:

```text
Codex edit test - YYYY-MM-DD HH:mm:ss
```

Verify the line appears in the description and that the `Save` button becomes enabled. Do not click Save unless the user confirms the test should be saved. Remove the test line before saving real content.

### 5. Write The Real Content

When the user asks for a live update:

1. Read the current field content.
2. Prepare the replacement or appended text in Australian English.
3. Replace only the intended field.
4. Verify the new text is present and unwanted test text is absent.
5. Click `Save`.
6. Wait for a confirmation such as `Material edited`.
7. Report the result clearly.

For Classroom descriptions, plain text with simple emoji markers works well. Keep emojis functional and sparse, for example:

- `🔎` question focus.
- `🧰` checklist/resource example.
- `✅` learner task requirements.
- `💡` hints.

### 6. Shut Down Debug Mode

At the end of the task, remind the user:

```text
Close the temporary Chrome debug window when finished.
```

If a script or tool launched the profile, stop only the debug Chrome process if it is clearly tied to `C:\Temp\chrome-codex-debug`. Do not close unrelated browser sessions without user approval.

## Reference Material

Load these only when needed:

- `references/google-classroom-debugging-faq.md` for public FAQ and troubleshooting copy.
- `references/x-launch-post.md` for a short launch post/thread draft.

## Validation Checklist

- Debug endpoint responds on `127.0.0.1:9222`.
- Target Classroom tab is visible in `/json/list`.
- User is signed in to the expected Google account.
- Target material is in edit mode before writing.
- A reversible write test succeeded, if this is a new session.
- Real content was verified before saving.
- Save confirmation was observed.
- User was reminded to close the debug Chrome window.
