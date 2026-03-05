# keel-skill

`keel-skill` packages the Keel workflow as:

- A reusable agent skill: [`skills/keel-workflow`](skills/keel-workflow)
- A Claude Code plugin: [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json)
- A Gemini CLI extension: [`gemini-extension.json`](gemini-extension.json)
- Shared harness context files: [`AGENTS.md`](AGENTS.md), [`CLAUDE.md`](CLAUDE.md), and [`GEMINI.md`](GEMINI.md)

The source workflow came from `~/workspace/rupurt/keel/AGENTS.md` and is now split into:

- A shared harness-level workflow file in [`AGENTS.md`](AGENTS.md)
- A concise reusable skill in [`skills/keel-workflow/SKILL.md`](skills/keel-workflow/SKILL.md)
- Namespaced plugin commands in [`commands/keel`](commands/keel)

## What You Get

- `keel-workflow` skill for planning, implementation, and research tasks in Keel-managed repositories
- `/keel:implement`, `/keel:plan`, and `/keel:research` commands for Claude Code and Gemini CLI
- Importable `AGENTS.md` instructions for repos that want shared harness context instead of a global install

## Install

### Codex

From the repo root, install the skill directly into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
ln -s "$(pwd)/skills/keel-workflow" ~/.codex/skills/keel-workflow
```

If you want workspace-scoped installation for tools that support the generic agent-skills alias, symlink the same folder into `.agents/skills/keel-workflow` inside the target repository instead.

### Claude Code

From the parent directory of this repo, create a small local marketplace that points at this plugin:

```bash
mkdir -p ../dev-marketplace/.claude-plugin
cat > ../dev-marketplace/.claude-plugin/marketplace.json <<'EOF'
{
  "name": "local-keel",
  "plugins": [
    {
      "name": "keel-skill",
      "source": "../keel-skill",
      "description": "Keel workflow plugin"
    }
  ]
}
EOF
```

Start Claude Code from the parent directory, add the marketplace, and install the plugin:

```text
/plugin marketplace add ./dev-marketplace
/plugin install keel-skill@local-keel
```

For published installs, point `/plugin install` at the marketplace entry that serves this repo.

### Gemini CLI

From the repo root, install the full extension:

```bash
gemini extensions link .
```

You can also install from a Git repository:

```bash
gemini extensions install https://github.com/your-org/keel-skill
```

If you only want the reusable skill instead of the full extension:

```bash
gemini skills install "$(pwd)/skills/keel-workflow"
```

## Usage

### Codex

The skill auto-triggers when the task is clearly about Keel workflow operations such as planning a voyage, implementing a story, or running bearing research. You can also mention the skill explicitly:

```text
Use the keel-workflow skill to plan the next voyage.
Use the keel-workflow skill to implement the next story with TDD.
Use the keel-workflow skill to research this ambiguous feature area.
```

### Claude Code

After installation, use the bundled namespaced commands:

```text
/keel:implement STORY-123
/keel:plan "Plan the auth hardening voyage"
/keel:research "Explore API versioning strategy"
```

The bundled `keel-workflow` skill is also available for automatic invocation when the task matches.

### Gemini CLI

After linking or installing the extension, use the same namespaced commands:

```text
/keel:implement STORY-123
/keel:plan "Plan the auth hardening voyage"
/keel:research "Explore API versioning strategy"
```

Gemini also loads [`GEMINI.md`](GEMINI.md) at session start and can invoke the bundled `keel-workflow` skill automatically.

## Repo Layout

```text
.
├── AGENTS.md
├── CLAUDE.md
├── GEMINI.md
├── .claude-plugin/plugin.json
├── gemini-extension.json
├── commands/keel/
└── skills/keel-workflow/
```

## Official References

- OpenAI Codex agent skills: https://developers.openai.com/codex/guides/skills
- Claude Code plugins: https://docs.claude.com/en/docs/claude-code/plugins
- Gemini CLI skills: https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/skills.md
- Gemini CLI extensions: https://github.com/google-gemini/gemini-cli/blob/main/docs/extensions/index.md
