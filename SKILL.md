---
name: store-screenshots
description: >-
  Design App Store and Play Store screenshots for a mobile app with design_ss.
  Use when the user asks for App Store screenshots, Play Store screenshots,
  store listing images, a screenshot strip, or ASO screenshots for iPhone,
  iPad, Android phone or Android tablet, or asks how to use design_ss. Sets up
  design_ss inside this skill's folder, tells the user what to put in the input
  folder, hands the design work to design_ss's own instructions, and points the
  user to the finished PNGs.
---

# Store screenshots

You are the coordinator for **design_ss**, a toolkit that designs App Store and
Play Store screenshot strips and renders them at the exact sizes the stores
require. You do not design anything yourself, and this file holds no design
rules. design_ss has its own instructions for that, and once the input is ready
you follow them.

Your four jobs, in order: **set up** design_ss, **check the input** and show
the user where it goes, **hand over** to design_ss's instructions, and **point
to the output**. Between those, answer the user's questions about design_ss.

## Paths

- `SKILL_DIR`: the absolute path of the folder that holds this `SKILL.md`.
- `DS`: `SKILL_DIR/design_ss`, the design_ss checkout this skill manages.
- Pinned version: **`v0.1.6`**. This is the only place the version is named.

Always give the user **absolute** paths, never `~` or relative ones, so they
can paste them into Finder or a terminal.

## Step 1: Set up (skip when done)

Setup is done when all three exist:

- `DS/AGENTS.md`
- `DS/node_modules/playwright/`
- `DS/strip_editor/node_modules/`

If they do, go straight to Step 2 without mentioning setup.

Otherwise:

1. **Check the requirements:** `git`, Node.js 22 (20 is the minimum: check
   `node --version`), npm, a network connection, and about 350 MB of disk. If
   one is missing, say which one and stop.
2. **Ask before downloading.** Tell the user it will clone design_ss `v0.1.6`
   into `DS` and download its dependencies, a headless Chromium and the
   editor's packages, about 350 MB once. Wait for a yes.
3. **Warn about a project install.** If `SKILL_DIR` sits inside a git
   repository whose top level is not `SKILL_DIR` itself (check with
   `git -C "SKILL_DIR" rev-parse --show-toplevel`), the skill was installed
   inside the user's project. Tell them to add the `design_ss/` path to that
   project's `.gitignore`, so 350 MB does not end up in their repository.
4. **Run**, from `SKILL_DIR`. If `DS` already exists but setup is incomplete,
   skip the clone:

   ```bash
   git clone --depth 1 --branch v0.1.6 https://github.com/MeasnaLazi/design_ss.git design_ss
   cd design_ss
   npm install
   npm run setup
   ```

   `npm run setup` is safe to run again. It skips what is already installed.
   Its messages suggest `design-ss …` commands. Ignore them: this skill does
   not use the `design-ss` command (see Step 4 for the editor).
5. **If the Chromium download fails,** design_ss falls back to an installed
   Google Chrome and says so. That is fine. Tell the user, and continue only if
   one of the two browsers is available.
6. **Never edit design_ss files to make setup work.** If a step fails, show the
   error and stop.

**Version check.** If `DS` exists, compare `git -C "DS" describe --tags` with
the pinned version. If they differ, tell the user, and ask before replacing
the checkout: their `input/` and `strips/` live inside it, so move them
across first.

## Step 2: Check the input

The input folder is **`DS/input/`**. Do not ask the user questions to collect
the app's details. Tell them what the folder needs and give them its path.

The input is ready when it has both of these:

1. **`DS/input/app.md`** with the app's name as the `# ` heading and a
   `## Summary` section.
2. **At least one target folder with at least one screenshot in it.** The
   folder name is the target:

   | Folder | Store |
   | --- | --- |
   | `iphone/` | App Store |
   | `ipad/` | App Store |
   | `phone/` | Play Store |
   | `tablet_7/` | Play Store |
   | `tablet_10/` | Play Store |

Also worth telling the user:

- Name each screenshot for what it shows (`timeline.png`, not `IMG_4821.PNG`).
  Five or more per target is comfortable.
- An app icon (`*icon*.png`) at the root of `input/` is optional and helps the
  palette.
- Images loose at the root of `input/`, other than the icon, are ignored.
- The full format, including tone, theme, panel copy and `follows:`, is in
  `DS/input/README.md`.

If anything is missing, tell the user exactly what is missing, give the
absolute path of `DS/input/`, show this minimal `app.md`, and stop and wait:

```markdown
# App Name

## Summary

One or two sentences: what the app does and who it is for.
```

Do not write `app.md` or copy screenshots yourself unless the user asks you to.

**One app at a time.** `DS/input/` holds one app's brief. If it already holds a
different app's brief than the one the user is talking about, say so before
they replace it. Never delete anything in `input/` or `strips/` yourself.

## Step 3: Hand over to design_ss

When the input is ready and the user wants a design (or a re-run, or an edit to
an existing strip):

1. Work from **`DS`** as your working directory. design_ss's instructions use
   paths relative to it.
2. Read **`DS/AGENTS.md`**, then **`DS/skills/strip-design/SKILL.md`**, and
   follow them in full. From this point they are the authority. If anything in
   this file seems to disagree with them, they win.
3. Pass the user's request through as they gave it: which targets, any
   direction ("warmer palette"), or the specific change they want.
4. **You are the designer.** Do not run the `design-ss` command or start
   another agent.

## Step 4: Point to the output

When design_ss's instructions are finished, tell the user:

- **The PNGs:** `DS/strips/<target>/rendered/`, as an absolute path.
  `panel0.png`, `panel1.png` and so on are the store uploads, in order, and
  `strip.png` is the whole set side by side.
- **The copy:** any panel headlines that were drafted for them were written
  into `DS/input/app.md`. They should review them there.
- **The editor, for hand adjustments:** run
  `cd "DS/strip_editor" && npm run dev` (with the absolute path), then open
  `http://localhost:4714/?strip=strips/<target>/strip.html`. Offer it; do not
  start it unasked.
- **Keep a copy.** The next run replaces `DS/strips/<target>/` entirely, so
  the user should copy the PNGs somewhere else to keep them. `strip.html`
  only renders inside design_ss, so later edits happen here.

## Guiding the user

When the user asks how design_ss works or what an option does, answer from
design_ss's own docs: `DS/README.md`, `DS/input/README.md` and `DS/docs/`. If
setup has not happened yet, use https://github.com/MeasnaLazi/design_ss.
Answer from those docs, not from memory, and say which file the answer came
from.

## Rules

- Do not copy design rules, sizes or formats from design_ss into this file.
  design_ss's own files are the authority.
- Change nothing in `DS` except `input/` (only when the user asks) and
  `strips/` (only through design_ss's instructions).
- Never delete the user's input or output without asking.
