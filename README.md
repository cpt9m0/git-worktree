# OpenCode Worktree Skill

An agent skill that teaches OpenCode to create or reuse a Git worktree and carry out a coding task inside it.

It checks existing worktrees and local changes, honors the requested branch and base, and makes file edits and commands target the correct checkout. It leaves the worktree available for review and only removes it when requested.

## Install

Requires Git and Node.js with npm/npx.

Install globally for OpenCode using the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add cpt9m0/opencode-worktree --skill opencode-worktree --agent opencode --global
```

For a project-only installation, run the same command from your project without `--global`:

```bash
npx skills add cpt9m0/opencode-worktree --skill opencode-worktree --agent opencode
```

You can inspect the available skill before installing:

```bash
npx skills add cpt9m0/opencode-worktree --list
```

The Skills CLI downloads the skill from GitHub; no separate npm package is required.

## Use

Restart OpenCode after installation, then ask:

> Use a worktree to implement the login feature.

Or specify a base and branch:

> Create a worktree from main on feature/login and implement the login feature there.

The description makes the skill available for automatic selection. Selection is model-driven, so explicitly ask to load `opencode-worktree` if needed. OpenCode versions supporting skill slash commands also expose `/opencode-worktree`.

The skill uses standard Git commands and does not depend on a particular desktop or web layout. Creating a worktree does not move an existing OpenCode session automatically: the agent must switch through a supported tool, target the new directory explicitly, or provide a relaunch command if its tools cannot access the checkout.

## Manual installation

Copy [SKILL.md](skills/opencode-worktree/SKILL.md) into one of OpenCode's skill discovery locations:

- Global: `~/.config/opencode/skills/opencode-worktree/SKILL.md`
- Project: `.opencode/skills/opencode-worktree/SKILL.md`

See the [OpenCode skill documentation](https://opencode.ai/docs/skills/) for discovery and permission settings.

## Contributing

Edit `skills/opencode-worktree/SKILL.md` and open a pull request. Keep instructions focused on worktree behavior and preserve existing changes and the user's requested scope. Include a realistic prompt and expected behavior when reporting a problem.

## License

[MIT](LICENSE). This is a community skill and is not affiliated with OpenCode.
