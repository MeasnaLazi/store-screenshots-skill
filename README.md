# store-screenshots-skill

**An agent skill that designs App Store and Play Store screenshots for your app,
using [design_ss](https://github.com/MeasnaLazi/design_ss).**

Install the skill, ask your agent for store screenshots, and it takes care of
the rest: it sets up design_ss, tells you what to put in the input folder,
lets design_ss's own instructions design the strip, and tells you where the
finished PNGs are.

This skill is a coordinator. It contains no design rules of its own. All of
that lives in design_ss, so the skill and the toolkit can never disagree.

## How it works

```
store-screenshots/            ← this skill
├── SKILL.md                  the coordinator your agent reads
└── design_ss/                cloned here on first use (pinned to v0.1.6)
    ├── input/                ← you put app.md and your screenshots here
    └── strips/<target>/
        └── rendered/         ← the finished, store-size PNGs
```

1. **Set up, once.** On first use the agent asks before cloning design_ss into
   this skill's folder and installing its dependencies (about 350 MB). After
   that, setup is skipped.
2. **Input.** The agent tells you what `design_ss/input/` needs and gives you
   its full path. You add the files.
3. **Design.** The agent follows design_ss's own instructions (`AGENTS.md` →
   `skills/strip-design/SKILL.md`): it drafts any missing headlines, chooses a
   design concept, writes the strip, checks it and renders it.
4. **Output.** The agent gives you the path to the PNGs and, if you want to
   adjust them by hand, how to open the visual editor.

## What you provide

```
design_ss/input/
  app.md            # App Name  +  ## Summary  (everything else is optional)
  appicon.png       optional
  iphone/           one folder per target, holding that device's screenshots
    welcome.png
    timeline.png
```

Targets: `iphone` and `ipad` for the App Store; `phone`, `tablet_7` and
`tablet_10` for the Play Store. The full format is in
[design_ss's `input/README.md`](https://github.com/MeasnaLazi/design_ss/blob/v0.1.6/input/README.md).

## Requirements

- An agent that supports `SKILL.md` skills (Claude Code, for example)
- `git`, and Node.js 22 (20 is the minimum)
- About 350 MB of disk, and a network connection for the first setup
- Developed and tested on macOS. Linux should work. Windows is untested.

## Install

Put this folder in your agent's skills folder. For Claude Code, a personal
install that works in every project:

```bash
git clone https://github.com/MeasnaLazi/store-screenshots-skill.git ~/.claude/skills/store-screenshots
```

The repository is `store-screenshots-skill`, and the skill inside it is named
`store-screenshots`. Clone it into a folder with the skill's name, as above.

A personal install is recommended. If you install the skill inside a project
instead, add `design_ss/` to that project's `.gitignore`, because the design_ss
checkout lives inside the skill folder.

## Use

Ask your agent:

> make App Store screenshots for my app

> design the Play Store phone screenshots

> make panel 2's device bigger

> how do I set the colour theme in design_ss?

## Updating

The skill pins design_ss to one version (`v0.1.6`, named once in `SKILL.md`).
When a new version of this skill pins a newer one, the agent notices the
mismatch and asks before replacing the checkout. It moves your `input/` and
`strips/` across first.

Reinstalling the skill deletes the `design_ss/` folder inside it, including
your input and output. Copy anything you want to keep first.

## Uninstall

Delete the skill folder. design_ss and everything it downloaded live inside it.
The headless Chromium is cached by Playwright outside the folder (on macOS,
`~/Library/Caches/ms-playwright/`); delete that too if nothing else uses it.

## License

[MIT](LICENSE). design_ss is MIT too, and it ships typefaces under the SIL Open
Font License 1.1.
