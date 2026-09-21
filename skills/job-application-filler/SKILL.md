---
name: job-application-filler
description: Fill and verify a job application form that the user has already opened or linked, using a resume-backed local profile, registered attachments, and remembered preferences. Use only when explicitly invoked; do not search for jobs or submit applications.
---

# Job Application Filler

Help a job seeker fill an application accurately, improve their reusable profile after each application, and by default hand the completed page back before any save, preview, advance, consent, or submission action.

## Scope and invariants

- Work only on a role the user has already found and opened or linked. Do not search for, rank, or recommend jobs.
- Ask which browser to use during first-time setup, remember it in `填写偏好.md`, and reuse it unless the user requests another browser.
- Prefer an existing matching tab. If the user supplied only a URL, open it in the remembered browser.
- Never click final submission. By default also leave `暂存`, `保存草稿`, `下一步`, `预览`, declarations, consent, and similar workflow-advancing buttons to the user.
- Hand off login, CAPTCHA, authorization/consent, declarations, signatures, and final submission to the user.
- Never fabricate a factual answer. Gather all unresolved factual and subjective questions and ask them in one batch.
- Respect the active browser/computer-use confirmation policy. Explicit invocation is not a substitute for any confirmation the platform requires before transmitting personal data or files.

## First-time initialization

1. Ask the user for their resume. Accept PDF, Word, and image resumes; use the relevant document/PDF capability when available and warn when extraction is unreliable.
2. If no existing job-profile directory is discoverable, create `求职资料` under the user's Documents directory. Do not ask the user to choose a path.
3. Copy the templates from `assets/` into that directory without overwriting existing records: `个人资料.md`, `附件清单.md`, `填写偏好.md`, and `投递记录.csv`.
4. Ask whether the user wants sensitive information distinguished from ordinary information. Offer exactly two modes: do not persist sensitive values and ask for them when needed; or persist them separately in `敏感资料.md`. Create `敏感资料.md` only for the second mode.
5. Extract initial facts from the resume, record their source as `简历提取`, and leave uncertain values unresolved. Ask the user to confirm or correct a single consolidated summary.
6. Ask for and remember the default browser, target-role priorities, location preferences, salary expectation, resume-to-role mappings, and recurring subjective preferences.

Read [references/data-model.md](references/data-model.md) when initializing or modifying the local profile.

## Per-application workflow

### 1. Inspect before transmitting

- Connect to the remembered browser and inspect the exact role page.
- Identify the company, role, location, recruitment system, required fields, optional professional sections, upload controls, and page actions.
- Select a resume using the stored role-to-resume mapping. If no mapping fits, ask once and remember the answer.
- Determine which photo and required attachments will be used from `附件清单.md`.
- Map the available sections and their fields once. On a step-by-step site, inspect the current step rather than repeatedly traversing inaccessible later steps.

### 2. Show one preflight summary

Before entering personal data or uploading files, show the company, role, destination site, selected browser and attachments, profile facts that will be transmitted, remembered choices, proposed option mappings, and all missing facts or exact dates.

Wait for the user's confirmation when required by the active browser policy. After confirmation, complete the form continuously unless new material uncertainty appears.

### 3. Parse first, then correct

When the site supports resume parsing, upload the selected resume to the parser first. Wait for parsing to finish, then correct every populated field against the trusted records.

Use this source priority:

1. user-confirmed local profile;
2. user-confirmed answer in the current conversation;
3. resume content;
4. website parser output.

Actively check for implausible dates, wrong schools, merged or duplicated projects, truncated descriptions, incorrect degree mappings, and missing attachments.

### 4. Fill using these rules

- Fill required personal fields and optional fields that materially improve the application.
- Fill all available education, project, award, campus-role, language, and other professional sections when the profile contains reliable data.
- Upload extra transcripts or certificates only when required. Resume and photo follow the site's required upload fields and the attachment registry.
- If a date requires a month but only a year is known, ask the user; never invent the month.
- If the exact rank is unavailable and the site offers `其他`, choose `其他` rather than a false range.
- If the user has no internship/work history but the site forces one undeletable row, enter `无` for organization and content and use the application month for both start and end dates. Record this as a website-required placeholder.
- Apply stored role, city, salary, relocation, travel, overtime, and other subjective preferences. If a preference is unknown, ask once and store the confirmed answer.
- Preserve user wording where possible. Condense only to satisfy field limits, without adding unsupported claims.
- Use the browser's documented file-chooser flow for uploads and verify the displayed filename afterward.

### 5. Fill efficiently without weakening checks

- Prepare a section-level field-to-value plan from the trusted profile before editing. Reuse the profile and the current browser/tab handle instead of reopening files, tabs, or the same page between fields.
- For stable, already-inspected fields, perform several ordinary browser `fill`/selection actions consecutively, preferably in one tool call. Use whole-value fill or paste, not character-by-character typing. Do not mutate the page through read-only inspection APIs, hidden network requests, or unsupported scripts to gain speed.
- Check the cheapest authoritative state after a meaningful group of actions (normally once per section, before saving or leaving it), not a full DOM snapshot or screenshot after every field. Reinspect immediately when an action changes the form structure, a dependent field, validation, or upload state; do not continue against stale locators.
- Avoid redundant clicks and blur operations: when the next field naturally commits the previous field, use that transition and verify the resulting value at the section boundary. Do not use fixed sleeps unless the page is actually waiting for a known transition.
- On multi-step forms, finish and verify the visible step, then move on only if the user has explicitly authorized that site's save/next action and the active browser policy permits it. Otherwise leave the step ready for the user. Never infer authorization for consent, declarations, or final submission.
- If ordinary browser actions repeatedly take unusually long, time a small read-only operation, distinguish page delay from control-channel delay where possible, and tell the user what was observed. Reduce redundant inspections/retries, but do not promise a specific speedup or assume a proxy is the cause.
- Report progress at section boundaries and during long-running work; do not stop for a new conversational turn after each field. If a tab becomes stale or a connection fails, avoid reloading or navigating away from unsaved entries; explain when their state cannot be verified.

### 6. Verify and hand off

- Re-scan all required fields and verify upload filenames.
- Check that resume parsing did not overwrite corrected values.
- List every blank optional field, user-owned question, placeholder, inference, and unresolved mismatch.
- Do not click save, next, preview, consent, declaration, or submit controls by default. A specific authorization can override save/next/preview only, subject to the active browser policy; consent, declarations, and final submission remain with the user.
- Keep the live page available and hand it back to the user.

### 7. Update the profile

- Add newly confirmed facts and preferences after the application.
- Register newly supplied attachments and role mappings.
- Append a row to `投递记录.csv` using one of these statuses: `填写中`, `等待使用者补充`, `已填完未提交`, or `已提交`.
- Record `已提交` only after the user explicitly says the application was submitted.
- Preserve audit notes for parser corrections and forced placeholders.

The completion report must include filled sections, uploaded files, blank fields, placeholders or inferences, user actions still required, confirmation that submission was not clicked, and local-record updates.

## Browser and connection failures

Retry the same failed browser or upload operation at most twice after re-observing the page. If it still fails, stop and give the user a precise manual step.

For missing extensions, Edge/Chrome family mismatches, local extension communication failures, or proxy/Clash conflicts, read [references/browser-troubleshooting.md](references/browser-troubleshooting.md). Diagnose before changing anything. Describe the exact proposed proxy, browser-permission, or system-setting change and obtain confirmation immediately before making it.
