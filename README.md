# Code With Judgment

An Agent Skill for writing features, fixing bugs, reviewing changes, and removing unnecessary complexity. It works across languages and follows the idioms of the repository it runs in.

The skill starts with callers and the behavior they rely on. It uses those contracts to guide implementation, simplify branching and state, review the actual diff, and choose checks that exercise real entry points. A short refactoring reference covers common transformations and the behavior each can accidentally change.

## Quick start

Run the command for your agent from the root of the project where you want to use the skill. The Codex command installs in `.agents/skills`, which Cursor also reads. The Claude Code command installs in `.claude/skills`.

```sh
gh skill install denelbjohn18/code-with-judgment code-with-judgment --agent codex --scope project
gh skill install denelbjohn18/code-with-judgment code-with-judgment --agent claude-code --scope project
```

For a manual install, copy the entire `skills/code-with-judgment` directory into the corresponding skills directory. Use a fresh `code-with-judgment` destination or inspect and reconcile an existing copy before replacing it.

| Agent | Project directory | Home directory |
| --- | --- | --- |
| Codex and Cursor | `.agents/skills` | `~/.agents/skills` |
| Claude Code | `.claude/skills` | `~/.claude/skills` |

Invoke the skill explicitly at the start of a prompt.

| Agent | Example prompt |
| --- | --- |
| Codex | `$code-with-judgment Fix the duplicate notification when a delivery is retried. Add a regression check through the delivery entry point.` |
| Cursor | `/code-with-judgment Review this diff for caller regressions and state bugs. Leave the files untouched.` |
| Claude Code | `/code-with-judgment Simplify the order status logic while preserving the API and existing records.` |

Agents can also load the skill when a request matches its description. Selection depends on the host. For a standing project preference, add this line to your existing `AGENTS.md` for Codex or Cursor, or `CLAUDE.md` for Claude Code.

> Load the `code-with-judgment` skill before ordinary code changes and code reviews in this repository.

Keep existing project instructions in place. See the host docs for discovery and invocation details. [Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills), [Cursor](https://cursor.com/help/customization/skills), [Claude Code](https://code.claude.com/docs/en/skills), [Agent Skills specification](https://agentskills.io/specification).

## What informed the skill

- Linus Torvalds's emphasis on data structures and their relationships, and the Linux kernel's treatment of regressions. [Git mailing list](https://marc.info/?l=git&m=115401850825206), [Linux kernel regression guide](https://docs.kernel.org/process/handling-regressions.html).
- Google's criteria for code review and Martin Fowler's workflows and catalog for behavior-preserving refactoring. [Google engineering practices](https://google.github.io/eng-practices/review/reviewer/looking-for.html), [Workflows of Refactoring](https://martinfowler.com/articles/workflowsOfRefactoring/), [Refactoring catalog](https://refactoring.com/catalog/).
- Existing coding-agent skills that address unnecessary generated code and favor focused changes. [Cursor's Deslop skill](https://github.com/cursor/plugins/blob/main/cursor-team-kit/skills/deslop/SKILL.md), [Andrej Karpathy skills collection](https://github.com/multica-ai/andrej-karpathy-skills).
