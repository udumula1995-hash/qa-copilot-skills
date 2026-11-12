# QA skills for AI coding agents (Avo Assure + web testing)

Two Agent Skills for GitHub Copilot and other agents that read `SKILL.md` folders.

| Skill | What it does |
|---|---|
| `qa-web-testing` | Explores a web app, designs and writes manual test cases, scores which ones to automate, and produces Avo Assure automation designs. |
| `avo-assure` | Click paths, keyword syntax and rules for building test cases in Avo Assure Design Studio (folder-structure UI, v24.2 and later, Web). |

## Install

Copy both folders from `skills/` into one of:

- `.github/skills/` in your repository (shared with your team)
- `~/.copilot/skills/` (personal)

Then use your agent in agent mode and ask it to test a feature, write test cases, or add them to Avo Assure.
Fill in `avo-assure/references/site-notes.md` with your project name and naming rules first.

## Status and limits

- Written from Avo Assure's public documentation (https://docs.avoautomation.com). Not affiliated with or endorsed by Avo Automation.
- Not yet validated against a live Avo Assure instance; UI labels may differ by version. Corrections are welcome.
- Avo Assure cannot import hand-made test case files, so the agent needs a browser tool (for example Playwright) to enter test cases, or it produces a build sheet for manual entry.
- The keyword reference covers common Web actions, not the full catalogue.

## Contributing

Open an issue or pull request with the Avo Assure version you tested on and what differed.

## License

MIT. See `LICENSE`.
