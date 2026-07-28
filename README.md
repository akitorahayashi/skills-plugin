# skills-plugin

A minimal template for one Agent Skills plugin. The repository root is the
plugin root for Claude Code and Antigravity CLI, and is the plugin body that a
Codex marketplace distributes. One shared `skills/` directory supplies every
client; each manifest carries only client-specific identity.

## Structure

```text
skills-plugin/
├── skills/
│   └── example-skill/
│       └── SKILL.md                      # one directory per skill; name comes from SKILL.md frontmatter
├── .claude-plugin/
│   └── plugin.json                       # Claude Code manifest
├── .codex-plugin/
│   └── plugin.json                       # Codex manifest
└── plugin.json                            # Antigravity CLI manifest
```

`skills/` is shared across all three clients. Each manifest carries only that
client's identity; the skill body is never duplicated. Component files
(`skills/`, and later `hooks/`, `agents/`, `commands/`, `.mcp.json`) live at the
repository root. Only `plugin.json` belongs inside `.claude-plugin/` and
`.codex-plugin/`.

## What each manifest requires

- Claude Code — skills under `skills/` are auto-discovered, so
  `.claude-plugin/plugin.json` needs no `skills` field. Metadata like `author`,
  `homepage`, `repository`, `license`, and `keywords` is optional.
- Codex — `.codex-plugin/plugin.json` declares `"skills": "./skills/"`.
- Antigravity CLI — `plugin.json` is a closed schema: only `name` (required,
  `^[a-zA-Z0-9-_]+$`) and `description` are valid. Skills are discovered from
  `skills/`; do not add other fields.

## Customize

1. Rename the repository to your plugin name.
2. Rename `skills/example-skill/` to your skill's name and rewrite its
   `SKILL.md`. The `name` frontmatter sets the invocation name; the
   `description` frontmatter is the sentence the agent reads to decide when to
   use the skill, so make it a specific trigger.
3. Replace `example-plugin` with your plugin name (kebab-case) in all three
   manifests and replace `your-name` in the Claude Code and Codex manifests.
4. Add more skills as sibling directories under `skills/`. Group related skills
   in one plugin rather than splitting one plugin per skill.
5. Validate before distributing:

   ```bash
   claude plugin validate .
   ```

## Install

The repository root is the path passed to a single-plugin installation flow. Use
a marketplace only when one catalog must expose several independently
installable plugins; `skills-marketplace` is the separate template for that
case.

### Claude Code

Claude Code loads the repository directly for a development session:

```bash
claude --plugin-dir .
```

### Codex

Codex CLI installs only from a configured marketplace snapshot. This template
does not create its own marketplace, so distribute it through a separate
`skills-marketplace` repository when Codex installation is required.

### Antigravity CLI

Antigravity stages the plugin from the path or repository URL that it receives.
The repository root holds both `plugin.json` and `skills/`.

```bash
agy plugin install git@github.com:your-org/your-plugin.git
```
