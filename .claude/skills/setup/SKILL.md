---
name: setup
description: First-run onboarding for this workspace. Checks the LaTeX install, imports the user's existing CV into cv/master.tex, inventories their skills, interviews them to seed cv/bullet-bank.md, and records their details and preferences in CLAUDE.md. Use when the workspace still has placeholder content, or when the user asks to get started, set up, or add to their bullet bank.
argument-hint: "[bullets | skills | cv]  (optional: run just one part)"
---

# Setup

Input: $ARGUMENTS

You're guiding someone through setting up this workspace for the first time. They may
not be technical and may never have used Claude Code before. Your job is to make this
feel easy and to get true, detailed material out of them, because the quality of every
tailored CV depends on what goes into the master CV and the bullet bank here.

## How to run this

- **One topic at a time.** Never ask more than three questions in one message. Use the
  AskUserQuestion tool for choices (page size, spelling, yes/no); ask open questions in
  plain text.
- **Explain why** you're asking in half a sentence, and give an example of a good answer
  when the question is open-ended.
- **"Skip" and "I don't know" are always fine.** Never fill a gap with a guess. No
  invented numbers, tools, team sizes or outcomes. If a bullet would be better with a
  metric they don't have, ask once, and if they don't know, write it without one.
- **Save as you go.** Write each piece to disk as soon as it's agreed, so stopping
  halfway loses nothing. Tell them they can stop at any time and run `/setup` again
  later to pick up where they left off.
- **Show progress.** Start each phase with a line like "Step 3 of 6: your CV".
- Never stage, commit or push.

## 0. Work out where they are

Check what's already done:

| Phase | Done when |
|---|---|
| 1. Tools | `pdflatex --version` succeeds and `cv/master.tex` compiles |
| 2. About you | `CLAUDE.md` has a CV file prefix other than `YourNameCV` |
| 3. Your CV | `cv/master.tex` no longer contains `Jane Doe` |
| 4. Skills | `cv/bullet-bank.md` has a filled-in `## Skills` section |
| 5. Bullet bank | `cv/bullet-bank.md` has no `*Example:*` entries left |
| 6. Finish | all of the above |

If the input names one part (`bullets`, `skills`, `cv`), run just that phase, even if
it's already done (to add more or redo it).

Otherwise, on a fresh workspace, welcome them in a few lines: what this workspace does,
what setup involves (the six steps above, about 20-40 minutes, most of it the bullet-bank
interview), and that they can stop at any time. Then ask them to put their files in
`cv/sources/` (create it if missing):

- **Their current CV** (required). PDF is best. For a Word file, ask them to save it as
  PDF, or paste the text into the chat.
- **Optional extras that make the bullet bank much better:** older CVs or versions
  tailored for other jobs, their LinkedIn profile as a PDF (on their profile: More >
  Save to PDF), cover letters, a portfolio or personal website link, and their GitHub
  username.

If some phases are already done, say which ones and continue from the first one that
isn't.

## 1. Tools

- Run `pdflatex --version`. If it's missing, detect the OS and tell them what to install:
  MiKTeX (https://miktex.org) on Windows, MacTeX (https://tug.org/mactex/) on macOS, or
  `texlive-latex-extra` (e.g. `sudo apt install texlive-latex-extra`) on Linux. They will
  need to restart the terminal / Claude Code afterwards so `pdflatex` is on the PATH.
  Then stop and wait for them to come back.
- Compile the placeholder CV to check packages are present:
  `pdflatex -interaction=nonstopmode -halt-on-error -output-directory=cv/build cv/master.tex`
  On MiKTeX, if it fails over a missing package, run
  `initexmf --set-config-value=[MPM]AutoInstall=1` so packages install on demand, and
  retry.
- Ask whether they use Obsidian. If yes, tell them to open this folder as a vault
  (Obsidian > Open folder as vault). If no, that's fine: the application notes are plain
  Markdown files that work anywhere.

## 2. About you

Read everything in `cv/sources/` first, so you can confirm details rather than ask
for them.

- Confirm the contact details from their CV: name, location, phone, email, LinkedIn,
  GitHub or portfolio. Ask whether any should be left off.
- Ask the questions below, using AskUserQuestion for the choices:
  - Page size: A4 (UK, Europe and most of the world) or US Letter (US, Canada).
  - Spelling: UK or US English.
  - What roles they're targeting: job titles, seniority, industries, locations,
    remote/hybrid/on-site, and anything else that affects which jobs fit (e.g. needing
    visa sponsorship).
- Suggest a CV file prefix from their name (e.g. `JaneDoeCV`) and confirm it. This
  becomes the start of every saved PDF's name, so recruiters see their name.
- Write to `CLAUDE.md`: set the prefix, and fill the `## About me` section with the
  target roles, page size and spelling.

## 3. Your CV

- Rewrite `cv/master.tex` with their CV in this template. Keep the preamble and custom
  commands exactly as they are, and only change the page size option in
  `\documentclass` if they chose US Letter.
- **Keep their wording.** This is their CV, not a rewrite. Move content into the
  template's structure (Education, Experience, Projects, Technical Skills; add sections
  like Publications or Certifications only if their CV has them) and escape LaTeX
  specials (`& % $ # _`).
- Then list the improvements you'd suggest as a separate short list: weak openers
  ("Helped", "Responsible for"), vague bullets that need a number they might know,
  repeated verbs, bullets that wrap by one or two words, UK/US spelling. Apply only the
  ones they approve.
- **It must be exactly one page, and it should fill the page.** Tailoring works by
  swapping content in and out of a fixed budget, so the master should be full.
  - If their CV is too long, keep the strongest and most general content in the
    master and move the rest **word for word** into `cv/bullet-bank.md` under the right
    role. Nothing true is thrown away. Tell them what moved.
  - If it's too short, note which roles need more and cover them in the bullet bank
    interview (phase 5), then come back and fill the page.
- Compile, read the PDF to check it looks right, and show them the path to
  `cv/build/master.pdf`. **Stop and wait for approval** before moving on. Anything
  other than approval is feedback: apply it and show it again.

## 4. Skills

- List every skill you found in their sources, grouped (Languages, Frameworks, Tools,
  Cloud, Data, Domain knowledge, or whatever fits their field; not everyone is a
  software engineer).
- Ask what's missing: "Anything you've used that isn't here? Tools from old jobs,
  university, side projects all count." Prompt with categories relevant to their target
  roles.
- For anything not already backed up by a bullet, ask how they used it: professionally,
  in a project, or briefly/at university. That decides whether it can only go on the
  skills line or can support a bullet.
- Write the result to the `## Skills` section of `cv/bullet-bank.md`, noting depth
  where it isn't obvious (e.g. "Terraform: used professionally at Example Corp,
  managed staging only").

## 5. Bullet bank interview

This is the most valuable step. Explain that the bullet bank lets each tailored CV pull
the most relevant true material for that job, and that the more is in it, the better
the tailoring.

Go through their roles from most recent, then their main projects. For a long career,
cover the last three roles and their best two projects, and offer to do older ones
later. For each one:

1. Show what's already on the CV and in the bank for it.
2. Ask up to three of these per message, picking the ones that will uncover the most:
   - What did you build, own or change that isn't on your CV yet?
   - Any numbers: users, requests, money, time saved, team size, % improvement?
   - Who did you work with (team size, other teams, clients, senior leadership)?
   - Which tools and technologies did you use here?
   - What are you proudest of? What would a manager there say you were good at?
   - Did you lead, mentor, present, write documentation, or deal with production
     incidents?
3. From their answers and their sources, draft alternative bullets: different angles
   for different kinds of job (e.g. backend vs. data vs. leadership), and a short version
   of each long bullet. Mark which kind of job each angle suits.
4. **Ask them to confirm every drafted bullet is true** and fix anything that isn't
   quite right. Only confirmed bullets go in the bank.
5. Write them to `cv/bullet-bank.md` under the role, and put facts that are true but
   don't make a bullet (e.g. "worked directly with the CEO") under `## Confirmed facts`.

After each role, ask whether to continue to the next or stop for now. They can run
`/setup bullets` any time to add more.

If the master CV was too short in phase 3, go back and fill it now from the new
material, recompile and get approval.

## 6. Finish

- Remove any `*Example:*` entries left in `cv/bullet-bank.md` and any placeholder text in
  `CLAUDE.md`.
- Ask whether there's anything they always want on their CV, or never want ("always
  keep my dissertation", "don't mention my first job"). Add these to the
  `## CV preferences` section of `CLAUDE.md`.
- Compile `cv/master.tex` one last time to check it's still one page.
- Summarise what's set up in a few lines, then tell them how to use it:
  - `/apply <job link>` (or paste the job description) to tailor a CV and log the
    application.
  - Tell Claude about updates in plain words, e.g. "Acme rejected me" or "I've got an
    interview with Initech on Friday".
  - `/setup bullets` to add to the bullet bank later.
- If the folder is a git repo, suggest they commit their setup and remind them to keep
  the repo private, since it holds their CV and contact details. Don't commit for them.
