# Git Worktree Skill

An agent-neutral skill for creating or reusing a Git worktree and carrying out a coding task inside it. Use it with agents that support `SKILL.md` instructions and can access Git and the target checkout, including OpenCode, Claude Code, Codex, and Cursor.

It checks existing worktrees and local changes, honors the requested branch and base, and makes file edits and commands target the correct checkout. It leaves the worktree available for review and only removes it when requested.

## Install

Requires Git and Node.js with npm/npx.

Install using the [Skills CLI](https://github.com/vercel-labs/skills), selecting your agents interactively:

```bash
npx skills add cpt9m0/git-worktree
```

Install globally for all agents supported by the installer:

```bash
npx skills add cpt9m0/git-worktree --skill git-worktree --agent '*' --global
```

Or target specific agents:

```bash
npx skills add cpt9m0/git-worktree --skill git-worktree --agent opencode claude-code codex --global
```

For a project-only installation, omit `--global` and run from your project. Keep the wildcard quoted so your shell does not expand it.

Inspect the skill before installing:

```bash
npx skills add cpt9m0/git-worktree --list
```

The installer downloads the skill from GitHub; no separate npm package is required.

## Use

Restart or reload your agent's skill discovery after installation, then ask:

> Use a worktree to implement the login feature.

Or specify a base and branch:

> Create a worktree from main on feature/login and implement the login feature there.

The description makes the skill available for automatic selection in agents that support it. Selection and explicit invocation syntax vary by agent; explicitly ask to load `git-worktree` if needed.

The skill uses standard Git commands and does not depend on an agent-specific UI, tool, launcher, or configuration. Creating a worktree does not automatically move the agent's active workspace. The agent must switch through a supported tool, target the new directory explicitly, or explain how to reopen its workspace if its tools cannot access the checkout.

## Manual installation

Copy [SKILL.md](skills/git-worktree/SKILL.md) into your agent's documented skill discovery location, in a folder named `git-worktree`. Agents supporting the shared layout can use `.agents/skills/git-worktree/SKILL.md` in a project.

## Migrating from opencode-worktree

The repository and skill were previously named `opencode-worktree`. Install `git-worktree` using the commands above. Remove the old installed skill from the same scope to avoid duplicate discovery:

```bash
# For the original global OpenCode installation managed by Skills CLI
npx skills remove opencode-worktree --agent opencode --global
```

If you installed the old skill by copying it manually, remove that old `opencode-worktree` folder from your agent's skill directory after installing the replacement.

## Contributing

Edit `skills/git-worktree/SKILL.md` and open a pull request. Keep instructions focused on worktree behavior and preserve existing changes and the user's requested scope. Include your agent, a realistic prompt, and expected behavior when reporting a problem.

## License

[MIT](LICENSE). This is a community skill and is not affiliated with any coding-agent vendor.
