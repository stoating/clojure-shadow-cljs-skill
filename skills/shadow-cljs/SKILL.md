---
name: shadow-cljs
description: Use when working with shadow-cljs, the ClojureScript build tool. Activate when the user asks about shadow-cljs.edn, :builds, :target :browser/:node-script/:node-test/:karma/:esm/:npm-module/:react-native/:chrome-extension, watch/compile/release/check/server commands, dev-http, hot reload, CLJS REPLs, npm dependencies, JS interop, code splitting, externs, build hooks, or troubleshooting shadow-cljs warnings and dependency conflicts.
version: 1.0.0
---

# shadow-cljs

shadow-cljs compiles ClojureScript with pragmatic defaults, npm integration, hot reload, CLJS REPL support, and build targets for browsers, Node.js, tests, ESM, npm packages, React Native, Chrome extensions, and more.

## Quick Decision: What do you need?

| Task | Go to |
|------|-------|
| Install, scaffold, run watch/release/check/server/REPL commands | [commands.md](commands.md) |
| Configure `shadow-cljs.edn`, dependencies, dev-http, build defaults | [configuration.md](configuration.md) |
| Choose or edit a build target (`:browser`, `:node-script`, `:esm`, tests, etc.) | [targets.md](targets.md) |
| Add a frontend, Node script, tests, release build, or code splitting | [workflows.md](workflows.md) |
| Work with npm packages, ESM output, externs, globals, or JS build tools | [interop.md](interop.md) |
| Fix failed loads, stale builds, REPL/hot reload issues, warnings, or dependency conflicts | [troubleshooting.md](troubleshooting.md) |

## Core Mental Model

The project has two dependency worlds:

- `package.json` manages JavaScript packages installed into `node_modules`.
- `shadow-cljs.edn` manages CLJS source paths, Maven dependencies, dev servers, and build definitions.

Each entry under `:builds` is a named build. Commands operate on those build ids: `npx shadow-cljs watch app`, `npx shadow-cljs release app`, `npx shadow-cljs compile app`, `npx shadow-cljs cljs-repl app`.

Prefer project-local execution with `npx shadow-cljs ...` or a package script so the checked-in `shadow-cljs` version is used. Before changing config, identify the target runtime and the desired artifact: browser bundle, Node script, test runner output, ESM files, npm module, React Native bundle, or extension package.
