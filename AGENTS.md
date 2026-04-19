# Agent instructions

This repository contains the **`shadow-cljs`** skill - a structured set of Markdown files that teach an AI coding agent how to work with shadow-cljs, the ClojureScript build tool.

**Any agent that understands this `AGENTS.md` convention should:**

1. Treat `shadow-cljs/SKILL.md` as the entry point - it contains a decision table pointing to the right reference file for the task.
2. Load reference files on demand based on that table:
   - `commands.md` - setup, watch, compile, release, check, server, REPL, and test commands
   - `configuration.md` - `shadow-cljs.edn`, dependencies, dev-http, build defaults, devtools
   - `targets.md` - browser, Node, tests, ESM, npm module, React Native, Chrome extension
   - `workflows.md` - browser apps, releases, code splitting, Node CLIs, tests, deps.edn, Polylith
   - `interop.md` - npm packages, ESM, globals, externs, JS tool integration
   - `troubleshooting.md` - failed loads, stale output, REPL/hot reload, advanced compilation, npm resolution
3. Clarify the target runtime and output artifact before changing `shadow-cljs.edn`.
4. Prefer project-local `npx shadow-cljs ...` or package scripts over globally installed shadow-cljs.
5. Keep `package.json` responsible for JavaScript dependencies and `shadow-cljs.edn`/`deps.edn` responsible for CLJS/JVM dependencies.

This file follows the [agents.md](https://agents.md/) convention and is honored by OpenAI Codex CLI, Cursor, Aider, Zed, Amp, Gemini CLI, Google Jules, Windsurf, Factory, RooCode, and many others.

For Claude Code, the richer native format is `.claude-plugin/` + `shadow-cljs/SKILL.md`.
