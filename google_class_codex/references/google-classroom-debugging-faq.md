# Google Classroom Debug Editing FAQ

## Who is this for?

This FAQ is for educators, course builders, and AI-assisted workflow teams who need to update Google Classroom material pages with help from Codex or another local coding agent.

## What problem does this solve?

The normal Codex in-app browser may not be signed in to the same Google account as your everyday Chrome profile. In that case, Codex can see a Google sign-in page but cannot inspect or edit the Classroom material you already have open.

Chrome remote debugging creates a temporary local bridge so Codex can see the tabs you intentionally open in a separate Chrome window.

## What is the basic workflow?

1. Close normal Chrome windows.
2. Launch a separate temporary Chrome profile with remote debugging enabled.
3. Sign in to the correct Google account.
4. Open the target Google Classroom material in edit mode.
5. Let Codex inspect the visible fields and run a small reversible write test.
6. Ask Codex to make the real update and save it.
7. Close the temporary debugging Chrome window when finished.

## What command starts the temporary debug browser?

Use PowerShell:

```powershell
& "C:\Program Files\Google\Chrome\Application\chrome.exe" `
  --remote-debugging-port=9222 `
  --user-data-dir="C:\Temp\chrome-codex-debug"
```

This opens a separate Chrome profile. You may need to sign in again because it is not the same as your normal Chrome profile.

## How do I check whether it worked?

Open these local URLs or ask Codex to check them:

```text
http://127.0.0.1:9222/json/version
http://127.0.0.1:9222/json/list
```

The first endpoint confirms Chrome is exposing a debugging connection. The second lists visible debug targets, including page titles, URLs, and WebSocket debugger URLs.

## Why use a temporary Chrome profile?

A temporary profile keeps this workflow separate from your normal browsing session. It also avoids the common problem where an existing Chrome process quietly reuses the old session and ignores the `--remote-debugging-port` flag.

## Is this safe?

It is safe enough for controlled local work, but it is powerful. Remote debugging allows local tools to inspect and control the debug Chrome window. Only use it on a trusted machine and only while you are actively working.

Close the debugging Chrome window when finished. Do not leave remote debugging running in the background.

## Can Codex see all my normal Chrome tabs?

Not unless those tabs are opened in the Chrome instance launched with remote debugging. Normal Chrome windows may only expose their window title through Windows process checks, not their page content or full tab list.

## What should Codex check before editing?

Codex should confirm:

- the debug endpoint is responding on `127.0.0.1:9222`;
- the target Classroom tab is visible in `/json/list`;
- the Classroom material is open in edit mode;
- the expected account is signed in;
- the description or title field is visible;
- the Save button is present; and
- a small write test can be typed into the field.

## What is a good write test?

Use a short line that is easy to remove:

```text
Codex edit test - YYYY-MM-DD HH:mm:ss
```

The test proves Codex can type into the correct field and that Classroom has enabled the Save button. Remove it before saving real content unless the user explicitly asks to save the test.

## Can Codex save changes?

Yes, if the user explicitly asks for a live update. Codex should verify the new text is present, verify unwanted test text is absent, click Save, and wait for a confirmation such as `Material edited`.

## What if the debug endpoint times out?

Usually Chrome was not launched with remote debugging, or an old Chrome process reused the session. Close Chrome fully and relaunch the temporary profile with the command above.

## What if `/json/list` shows many frames and service workers?

That is normal. Filter for targets where `type` is `page` and the URL or title contains `classroom.google.com` or `Classroom`.

## Can this be used for other Google tools?

Yes, the same debug bridge can inspect user-opened Google Docs, Drive, Sheets, and other browser pages. For native Google Docs and Sheets editing, prefer the official Google Drive connector when available because it is more structured and less brittle.

## What should be included in public instructions?

Always include the shutdown warning:

> Close the temporary Chrome debug window when finished. Remote debugging exposes browser control locally, so do not leave it running.

## What should not be done?

Do not use this workflow to bypass login, access another person's account, scrape private learner information, or modify live course content without explicit user direction.
