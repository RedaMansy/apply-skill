# Job application tracker

An Obsidian vault tracking job applications, kept in git.

<!-- Set this to your own name, e.g. JaneDoeCV. Saved CVs are named <prefix>_<suffix>.pdf -->
CV file prefix: `YourNameCV`

- One note per application in the repo root, named `<Company> - <Role>.md`, using the
  frontmatter in `.claude/skills/apply/entry-template.md`. Status values in use:
  Applied, Screening, Interviewing, Offer, Accepted, Rejected, Inactive (no response
  4+ weeks after applying; can still move on if they reply). When changing status,
  update `status`, `lastUpdated`, append to `statusHistory`, and add a line to the
  Notes & Activity Log.
- `Attachments/` holds the CV PDF sent for each application, linked from the note's
  `CV used` field.
- `cv/master.tex` is the base CV. Never edit it unless I ask to change the base CV.
  Tailored versions live in `cv/tailored/`; build output goes to `cv/build/` (ignored).
  `cv/bullet-bank.md` holds true alternative bullets and confirmed facts to draw from.
- New applications go through the `/apply` skill.
- Never invent experience on a CV. Don't push without my approval.
