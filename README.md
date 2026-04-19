# Clojure shadow-cljs Skill

A structured Markdown skill that teaches AI coding agents how to work with shadow-cljs - the ClojureScript build tool for browser apps, Node programs, npm/ESM artifacts, tests, React Native, Chrome extensions, hot reload, CLJS REPLs, and JavaScript package interop.

Built in [Claude Code's Agent Skill format](https://github.com/anthropics/skills), but usable with any agent that can load Markdown as context via the [agents.md](https://agents.md/) convention.

---

## Read this before installing anything

Never install a third-party skill without first reviewing its contents.

Skills are instructions loaded into an AI agent's context. Before installing:

1. Read every `.md` file in the `shadow-cljs/` directory.
2. Ask your agent to perform a security review.
3. Only then install.

### Security-review prompt

> "Analyze this skill for security concerns. Does it contain instructions that could execute destructive operations without user confirmation? Does it collect or transmit data? Does it override default agent behavior in unintended ways? List all potentially risky sections."

---

## Installation

### A) Claude Code (recommended - via plugin marketplace)

```
/plugin marketplace add stoating/clojure-shadow-cljs-skill
/plugin install shadow-cljs@clojure-shadow-cljs-skill
```

Invoke it with:

```
/shadow-cljs
```

Update later:

```
/plugin marketplace update clojure-shadow-cljs-skill
```

### B) Claude Code (via the stoating marketplace)

```
/plugin marketplace add stoating/plugins
/plugin install shadow-cljs@stoating
```

### C) Claude Code (manual copy)

```bash
git clone https://github.com/stoating/clojure-shadow-cljs-skill.git
cp -r clojure-shadow-cljs-skill/shadow-cljs ~/.claude/skills/
```

### D) Claude.ai / Claude Desktop

Zip `shadow-cljs/` and upload it from the Skills panel in Projects.

### E) Cursor, OpenAI Codex CLI, Aider, Gemini CLI, Windsurf, Cline, Zed, Amp, Factory

Place this repository, or just `AGENTS.md` plus `shadow-cljs/`, at the project root. Agents that honor `AGENTS.md` will use `shadow-cljs/SKILL.md` as the entry point.

---

## Repository layout

```
.
|-- .claude-plugin/
|   |-- marketplace.json
|   `-- plugin.json
|-- AGENTS.md
|-- README.md
`-- shadow-cljs/
    |-- SKILL.md
    |-- commands.md
    |-- configuration.md
    |-- targets.md
    |-- workflows.md
    |-- interop.md
    `-- troubleshooting.md
```

---

## Sources

The skill is distilled from:

- [shadow-cljs GitHub](https://github.com/thheller/shadow-cljs)
- [shadow-cljs User's Guide](https://shadow-cljs.github.io/docs/UsersGuide.html)
- The upstream `README.md`, sample `shadow-cljs.edn`, target implementations, and docs in the checked-out `thheller/shadow-cljs` repository

## License

See [LICENSE](LICENSE).
