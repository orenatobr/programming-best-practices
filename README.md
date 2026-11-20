# programming-best-practices

Six rules for writing or reviewing code in any language: TDD, SOLID without speculative abstraction, complete static typing, Google Style docstrings, linting, and logging instead of `print`. Before finishing, the agent walks the six checks and reports each one as OK, FIXED or RISK.

## Install

For Claude Code, copy the skill folders into your personal skills directory:

```bash
git clone https://github.com/orenatobr/programming-best-practices.git
mkdir -p ~/.claude/skills
cp -R programming-best-practices/skills/* ~/.claude/skills/
```

Start a new Claude Code session. Skills load from the `description` in each `SKILL.md`, so they trigger on their own and can also be called by name.

Other agents: each skill is a folder with a `SKILL.md` (YAML frontmatter plus Markdown). Tools that read the Agent Skills format can load `skills/` as it is. For the rest, paste the body of the `SKILL.md` into the agent's instruction file.

## Usage

Invoke `/programming-best-practices`, optionally with a file, diff or path to audit. It also triggers on "TDD", "typing", "lint" and "logging".

## Requirements

Nothing beyond an agent that loads skills. The TDD rule defers to `tests-guidelines` and the logging rule points to `attack-protection`, so install [tests-guidelines](https://github.com/orenatobr/tests-guidelines) and [attack-protection](https://github.com/orenatobr/attack-protection) as well.

## Related skills

`tests-guidelines`, `attack-protection`.

## What is in this repo

| Path | Purpose |
|------|---------|
| `skills/programming-best-practices/` | The `programming-best-practices` skill. |
| `skills/prs-fixes/` | PR helper: apply review feedback and resolve threads. |
| `skills/prs-reviews/` | PR helper: post a Requested Changes review. |
| `CHANGELOG.md` | Notable changes, in Keep a Changelog format. |
| `AGENTS.md` | Rules for AI agents that edit this repo. |

The two PR helpers ship in every repo of this family:

- `prs-fixes`: Applies the suggested fixes on one or more GitHub PRs, pushes them, replies to every review comment and resolves each thread. Example: `/prs-fixes 12, 15`.
- `prs-reviews`: Reviews one or more GitHub PRs and posts a Requested Changes review, one inline comment per finding, each with an exact location and a suggested change. Example: `/prs-reviews 12, 15`.

They need the `gh` CLI, and they call `git-guidelines`, `humanizer`, `programming-best-practices` and `attack-protection` when those skills are installed.

## Contributing

`main` is protected: changes land through a pull request with one approval. See [AGENTS.md](AGENTS.md) for the conventions.

## License

MIT, see [LICENSE](LICENSE).
