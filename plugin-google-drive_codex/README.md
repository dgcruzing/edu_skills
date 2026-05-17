# Google Docs Codex Skill FAQ

A reusable Codex skill package for working with Google Docs through the Google Drive / Google Docs connector in local Codex plugin sessions.

This skill was built for document-first education workflows where Codex needs to read, edit, format, import, or verify native Google Docs while preserving template structure and document quality.

## What is this skill for?

Use this skill when Codex needs to work with Google Docs, including:

- creating learner-facing Google Docs from structured content;
- editing existing Google Docs safely;
- preserving headings, tables, links, citations, and document structure;
- importing `.docx` files into native Google Docs;
- formatting ACS/VET learner readings;
- checking connector-visible document structure before handoff.

## What makes it useful?

The skill keeps the main workflow lean and routes detailed rules into reference files. That means Codex can load only the guidance needed for the current task instead of carrying every formatting rule all the time.

It also records hard-won lessons from real Google Docs work, including:

- tab-aware document reads and writes;
- full heading-range styling so the last character is not missed;
- using `foregroundColor` correctly in Google Docs formatting payloads;
- matching local template typography instead of guessing;
- verifying connector-visible output after writes.

## What is the ACS learner-reading format?

For ACS/VET learner readings, the skill uses this default profile unless the live document template says otherwise:

- Australian English;
- Calibri-compatible typography;
- Heading 1: 20 pt, bold, ACS navy `#1B2A6B`;
- Heading 2 / Heading 3: 16 pt, bold;
- 5E headings: ACS orange `#E8891A`;
- body text: 14 pt, dark neutral `#1F2937`;
- 115% line spacing;
- full paragraph-range styling for headings.

## Why does the skill care so much about headings?

Google Docs connector edits can look technically correct while still reading as unfinished or visually inconsistent. This skill treats document structure as part of completion.

If a new section heading does not match nearby headings in font, size, weight, hierarchy, or paragraph style, the output is not considered ready.

## What is the safest workflow for a new Google Doc?

For net-new documents, the preferred path is:

1. Create a local `.docx` first using the Documents skill and its QA workflow.
2. Upload and convert the `.docx` into a native Google Doc.
3. Verify the imported Google Doc through the connector.
4. Continue any follow-on edits using the Google Docs connector.

This avoids fragile direct creation paths and gives the document a proper structure before it becomes a live Google Doc.

## What is the safest workflow for editing an existing Google Doc?

1. Confirm the target document URL and document ID.
2. Read the full document, including tabs where present.
3. Identify the exact section, paragraph, table, or range to edit.
4. Preserve existing structure unless the user asks for a rewrite.
5. Apply connector writes with the correct `tabId` where required.
6. Read back the result and verify the inserted content, formatting, links, and structure.

## What are common failure points?

- Writing into the wrong tab because `tabId` was omitted.
- Assuming a plain text read is complete when the document uses tabs.
- Styling a heading range that excludes the final visible character.
- Using a shorthand colour field instead of `foregroundColor`.
- Leaving connector-default fonts in a document that uses a different local template style.
- Simulating headings with bold normal text instead of matching the actual heading structure.

## What reference files are included?

The `google-docs/references` folder contains focused guidance for:

- citations and hyperlinks;
- connector runtime and safety;
- figures and image insertion;
- foreground and active document guardrails;
- headings and question format;
- importing DOCX files into native Docs;
- request shapes and write safety;
- response and list formatting;
- section completeness and final pass;
- table formatting.

## Can this be used outside ACS?

Yes. The Google Docs workflow is general, but the ACS formatting defaults are specific to ACS/VET learner material. If your organisation has its own template or style guide, the live document style should override the ACS defaults.

## How do I adapt this skill to my own branding?

This skill includes ACS learner-reading defaults because that is where the workflow was developed. Other organisations can reuse the same Google Docs editing workflow and replace the branding rules with their own.

Recommended prompt:

```text
Use this Google Docs Codex skill, but adapt the document formatting to my organisation's branding. Ask me for the brand details you need, then update the style rules before editing the Google Doc.
```

Provide Codex with:

- organisation name;
- preferred spelling style, such as Australian English, UK English, or US English;
- primary and accent colours, preferably as HEX values;
- preferred font family;
- heading sizes and hierarchy;
- normal body text size;
- line spacing or paragraph spacing preferences;
- logo or image-use rules, if relevant;
- examples of a finished document that already matches the brand.

A simple branding handoff might look like this:

```text
Organisation: Example Training Group
Language: Australian English
Primary colour: #123456
Accent colour: #E8891A
Font: Arial
Heading 1: 20 pt, bold, primary colour
Heading 2: 16 pt, bold, primary colour
Body: 12 pt, dark neutral #1F2937
Line spacing: 115%
Style rule: Keep documents clean, practical, and learner-facing.
```

Ask Codex to update the relevant guidance in:

```text
google-docs/SKILL.md
google-docs/references/reference-headings-and-question-format.md
```

Keep the connector-safety rules, tab-aware write rules, `foregroundColor` guidance, and verification workflow unchanged. Those are not brand choices; they are reliability rules.

## Does this skill replace the Google Drive connector?

No. It tells Codex how to use the connector carefully. The connector still performs the actual Google Docs reads, writes, imports, and formatting operations.

## Is this open source?

This folder is shared as part of the `edu_skills` repository. See the repository-level `LICENSE` file for licence terms.

## Practical prompt examples

Use prompts like:

```text
Use the Google Docs skill to format this learner reading in ACS style.
```

```text
Update this Google Doc section, preserve the existing headings and tables, and verify the result after writing.
```

```text
Import this DOCX into Google Docs and return the native Google Docs link after verification.
```

## Final reminder

For Google Docs work, the text being correct is not enough. The document should look and behave like it belongs in the template: correct headings, readable spacing, working links, preserved tables, and verified connector readback.
