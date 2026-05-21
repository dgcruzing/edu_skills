# edu_skills

Open-source Codex skills and support guides for AI-assisted education workflows.

This repository is intended to hold reusable skills, FAQs, and workflow notes that help educators, trainers, and course builders work more efficiently with tools such as Google Classroom, Google Drive, Google Docs, and learning-resource build systems.

## Included Skills

### google_class_codex

A Codex skill for working with Google Classroom material pages through a user-opened Chrome remote debugging session.

Use it when:

- Google Classroom is open in a signed-in Chrome profile;
- the normal Codex browser cannot access the same signed-in session;
- you need to inspect or update a Classroom material title, description, or visible edit form;
- you want a controlled workflow for testing, editing, saving, and verifying changes.

See [google_class_codex](./google_class_codex/) for the skill files.

### plugin-google-drive_codex

A Codex skill package for working with Google Docs through the Google Drive / Google Docs connector in local Codex plugin sessions.

Use it when:

- you need to create, edit, format, import, or verify Google Docs;
- you want Codex to preserve document structure, headings, tables, links, citations, and template style;
- you are creating ACS/VET learner-facing Google Docs and want the established formatting profile;
- you want to adapt the Google Docs workflow to your own organisation's branding.

The package includes a GitHub-facing FAQ with instructions for adapting the skill to another organisation's brand.

See [plugin-google-drive_codex](./plugin-google-drive_codex/) for the Google Docs skill and FAQ.

## Important Safety Note

The Google Classroom workflow uses Chrome remote debugging. This is powerful because it exposes browser control on the local machine.

Only use debug mode while actively working.

Start Chrome with a temporary profile:

```powershell
& "C:\Program Files\Google\Chrome\Application\chrome.exe" `
  --remote-debugging-port=9222 `
  --user-data-dir="C:\Temp\chrome-codex-debug"
```

When finished, close the temporary Chrome debug window.

Do not leave Chrome remote debugging running in the background.

## Recommended Workflow

1. Launch the temporary Chrome debug profile.
2. Sign in to the correct Google account.
3. Open the Google Classroom material in edit mode.
4. Confirm the debug endpoint is available at:

```text
http://127.0.0.1:9222/json/version
http://127.0.0.1:9222/json/list
```

5. Let Codex inspect the visible fields before editing.
6. Run a small reversible write test before saving real changes.
7. Save only when the user explicitly confirms the live update.
8. Close the debug Chrome window when finished.

## Licence

This repository is released under the MIT Licence.

You can use, copy, modify, share, and adapt the material, provided the licence notice is included.

See [LICENSE](./LICENSE).

## Disclaimer

These skills and guides are provided as workflow support material. They do not bypass Google account permissions, Classroom access controls, institutional policy, or privacy obligations. Use them only with accounts, courses, and materials you are authorised to manage.
