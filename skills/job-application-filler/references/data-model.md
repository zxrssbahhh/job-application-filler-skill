# Local profile model

Use a single `求职资料` directory per job seeker. Reuse it across applications and never overwrite user edits.

## `个人资料.md`

Store ordinary facts as tables grouped by personal details, education, languages, skills, projects, awards, campus roles, work/internship history, and recurring application answers.

Every non-obvious value should carry a source/status such as `用户确认`, `简历提取，待确认`, `网页解析，待确认`, `网站选项映射`, or `网站强制占位`. Do not silently promote an extracted or inferred value to `用户确认`.

## `敏感资料.md`

Create only when the user chooses separate persistence. Store identifiers, full addresses, family/emergency contacts, health/legal information, and similar values here. If the user chooses non-persistence, request these values only when a form requires them and do not copy them into local records.

## `附件清单.md`

For each file record the category, absolute path, role/language/version tags, verified date, upload rule, and notes. Support multiple resumes and maintain a role-to-resume mapping. Never choose the newest filename merely because it is newer when a confirmed mapping exists.

## `填写偏好.md`

Record the default browser, sensitive-data mode, role priorities and resume mappings, location and salary preferences, relocation/travel/overtime preferences, subjective-question answers, known site-option mappings, approved placeholder rules, and dates that still require clarification.

## `投递记录.csv`

Use these columns:

`date,company,role,location,url,system,status,resume,photo,filled_sections,remaining_actions,placeholders,notes`

Quote fields containing commas or line breaks. Allowed status values are `填写中`, `等待使用者补充`, `已填完未提交`, and `已提交`.

## Conflict handling

User-confirmed profile data wins over current answers already superseded, resume text, and parser output. Do not overwrite a confirmed value when a parser disagrees; record the correction in the application notes.
