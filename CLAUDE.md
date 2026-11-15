# Job application tracker

An Obsidian vault tracking job applications, kept in git.

**First run:** if the CV file prefix below is still `YourNameCV`, or `cv/master.tex`
still contains `Jane Doe`, this workspace hasn't been set up yet. Before doing anything
else, welcome the user and run the `setup` skill.

CV file prefix: `YourNameCV`

## About me

<!-- Filled in by /setup: target roles, seniority, locations, page size, spelling. -->

## CV preferences

<!-- Standing rules for tailoring, e.g. "always keep my dissertation project".
     /setup and /apply add to this when I give a rule that should always apply. -->

## Workspace rules

- One note per application in the repo root, named `<Company> - <Role>.md`, using the
  frontmatter in `.claude/skills/apply/entry-template.md`. Status values in use:
  Applied, Screening, Interviewing, Offer, Accepted, Rejected, Inactive (no response
  4+ weeks after applying; can still move on if they reply). When changing status,
  update `status`, `lastUpdated`, append to `statusHistory`, and add a line to the
  Notes & Activity Log.
- `Attachments/` holds the CV PDF sent for each application (`<prefix>_<suffix>.pdf`),
  linked from the note's `CV used` field.
- `cv/master.tex` is the base CV. Never edit it unless I ask to change the base CV.
  Tailored versions live in `cv/tailored/`; build output goes to `cv/build/` (ignored).
  `cv/bullet-bank.md` holds true alternative bullets, skills and confirmed facts to
  draw from. `cv/sources/` holds my original CVs and other source documents.
- New applications go through the `/apply` skill.
- When I give feedback that should apply to every CV ("always", "never", "from now
  on"), add it to CV preferences above. When I confirm a new fact about my experience,
  add it to the bullet bank.
- Never invent experience on a CV. Never stage, commit or push unless I ask.
