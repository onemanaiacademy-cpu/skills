# Claude Skills from One Man AI

Free Claude skills from the One Man AI YouTube video **"Claude Code Skills"**. Copy them, change them to your own style, and use them.

A skill is a folder with a `SKILL.md` file (and, optionally, templates, checklists and scripts). Claude reads only the short description until a task matches, then it loads the full instructions.

## The skills

| Skill | What it does | Shown in the video |
|---|---|---|
| [`clean-code`](clean-code/SKILL.md) | Audits code, especially AI-written code: bugs, injection, typos, infinite loops, API safety, credentials and HTTPS, API call volume, lazy loading, page load | Found 6 bugs in a Gemini tic-tac-toe game, then fixed them |
| [`mini-web-app`](mini-web-app/) | Builds small single-file web tools in one house style: one `index.html`, offline, mobile-first, light/dark mode, saves in the browser, Polish zł and date formats | The same expense-tracker prompt, before and after |
| [`ui-bootstrap-guidelines`](ui-bootstrap-guidelines/SKILL.md) | Bootstrap 5 + React UI rules with 5 themes (Classic Light, Material 3, iOS 26 Liquid Glass, Dark Pro, Windows Fluent) | Two skills working together |
| [`meeting-action-items`](meeting-action-items/SKILL.md) | Turns messy meeting notes into an action table (owner, due, status, source quote), open questions and a follow-up message | Bonus |

`mini-web-app` uses Polish defaults (zł, `DD.MM.YYYY`) and the One Man AI colours. Change the tokens in its `SKILL.md` and `assets/template.html` to make it yours.

## How to install

**Claude Code (terminal or desktop app)**

Copy the skill's folder into one of these:

```bash
# for all your projects
mkdir -p ~/.claude/skills && cp -R clean-code ~/.claude/skills/

# for one project only
mkdir -p .claude/skills && cp -R clean-code .claude/skills/
```

Start a new session and ask: `What skills do you have available?`

**Claude app (Settings → Skills → Upload a skill)**

Zip the **whole folder** and upload the `.zip`. If you upload only `SKILL.md`, the extra files (for example `mini-web-app/assets/template.html`) are left out.

## How to use

- Just ask normally ("check my code", "build me a simple expense tracker"). The description tells Claude when to use the skill.
- Or call it by name: `/clean-code my_file.html`

## Safety

Only install skills you have read. A skill can tell Claude to run commands. These skills contain no scripts that call the internet, and no keys or passwords.

## License

MIT. See [LICENSE](LICENSE).
