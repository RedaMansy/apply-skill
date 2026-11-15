# job-apply-kit

A [Claude Code](https://claude.com/claude-code) skill that tailors a LaTeX CV to a job
description and logs the application in an [Obsidian](https://obsidian.md) vault.

Give it a job link and it will:

1. Fetch the job description (including from JS-heavy ATS pages like Workday) and give
   you a quick read on how your CV matches it.
2. Copy your master CV, tailor it to the role and compile it. It reorders and rewords
   bullets, pulls alternatives from your bullet bank and regroups skills, all while
   keeping the CV to **exactly one page**.
3. Stop for your approval, then save the PDF to `Attachments/`.
4. Create an Obsidian note for the application with status, source, salary, the full
   job description and a link to the exact CV you sent, then stop for approval again.

It never invents experience. Everything on a tailored CV comes from your master CV or
from your bullet bank, and when a job asks for something that isn't in either, it asks
you instead of guessing.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- A LaTeX distribution with `pdflatex` on your PATH:
  [MiKTeX](https://miktex.org) (Windows), [MacTeX](https://tug.org/mactex/) (macOS) or
  TeX Live (Linux)
- [Obsidian](https://obsidian.md) (optional, but it's what makes the notes useful)

## Setup

1. Click **Use this template** on GitHub and **make your copy private**: it will hold
   your CV, contact details and every application you make.
2. Clone it and open the folder as a vault in Obsidian.
3. Set your CV file prefix in `CLAUDE.md` (e.g. `JaneDoeCV`).
4. Replace the placeholder content in `cv/master.tex` with your own CV. Fill the page:
   tailoring works by swapping content in and out of a fixed one-page budget.
5. Fill `cv/bullet-bank.md` with true alternative wordings of your bullets, extra
   bullets that didn't fit the master, and facts that are true but aren't on your CV.
   The more you put here, the better the tailoring.
6. Check it compiles:
   ```
   pdflatex -interaction=nonstopmode -output-directory=cv/build cv/master.tex
   ```

## Usage

In Claude Code, from the repo root:

```
/apply https://jobs.example.com/software-engineer-123
```

You can also paste the job description text instead of a link (needed for LinkedIn,
which requires a login).

Any reply other than an approval counts as feedback: Claude applies it and shows you
the result again.

To update an application later, just tell Claude, e.g. "Acme rejected me" or "got a
screening call with Initech on Friday". `CLAUDE.md` tells it how to update the
status, history and activity log.

## Layout

```
CLAUDE.md                         rules for Claude: statuses, file layout, CV prefix
.claude/skills/apply/SKILL.md     the /apply workflow
.claude/skills/apply/entry-template.md   frontmatter + layout for application notes
cv/master.tex                     your base CV (never edited by /apply)
cv/bullet-bank.md                 true alternative bullets and confirmed facts
cv/tailored/<Company>.tex         one tailored CV per application
cv/build/                         LaTeX output (gitignored)
Attachments/<Prefix>_<Company>.pdf  the exact PDF you sent
<Company> - <Role>.md             one Obsidian note per application
```

Because each note has structured frontmatter (`status`, `dateApplied`, `source`,
`statusHistory`...), you can build tables and charts of your applications with
Obsidian Bases or the Dataview plugin.

## Tips

- Make the skill your own. `SKILL.md` is plain English, so add your preferences to it
  (e.g. "always keep my dissertation project", "never drop the second bullet at X").
- When you reword something at the approval step and like the result, ask Claude to
  add it to the bullet bank.
- Keep the PDFs you send in git. Rebuilding one later won't always give the same file
  if the template or your LaTeX install has changed, and the PDF is the record of what
  the recruiter actually saw.

## Credits

The CV template is [Jake Gutierrez's resume](https://github.com/jakegut/resume), based on
[sb2nov/resume](https://github.com/sb2nov/resume), both MIT licensed.
