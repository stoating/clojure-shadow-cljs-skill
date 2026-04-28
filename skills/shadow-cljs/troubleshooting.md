# shadow-cljs Troubleshooting

## Failed to Load

The upstream CLI reports failed loads as commonly caused by dependency conflicts. In `deps.edn` or `project.clj` projects, verify the versions of:

- `org.clojure/clojure`
- `org.clojure/clojurescript`
- `com.google.javascript/closure-compiler-unshaded`
- `thheller/shadow-cljs`

Use one source of truth for these when possible. If another dependency forces an incompatible ClojureScript or Closure Compiler version, exclude or pin it deliberately.

## Build Id Not Found

If `npx shadow-cljs watch app` says no build exists, verify:

```clojure
:builds {:app {...}}
```

The CLI argument is `app`, not `:app`.

## Browser Page Does Not Connect

Check:

- The page loads the generated script, e.g. `/js/main.js`.
- `:dev-http` serves the directory containing the HTML.
- `:asset-path` matches the browser URL path for JS chunks.
- The build is running in `watch` mode, not just `compile`, when expecting hot reload and REPL support.

## Hot Reload Is Unreliable

Move startup side effects behind functions:

```clojure
(defonce root (atom nil))

(defn render [] ...)
(defn init [] (render))
(defn ^:dev/after-load reload [] (render))
```

Avoid top-level DOM mutation, socket creation, timers, or app starts that run every reload without cleanup.

## npm Package Cannot Be Resolved

Check that the package is installed in `node_modules`, the package manager lockfile is current, and the required string matches the package export. For package export-map issues, inspect package `exports` and configure `:js-options` only as narrowly as needed.

## Advanced Compilation Breaks JS Interop

Symptoms include `undefined is not a function`, missing fields, or renamed object properties in release only.

Fix order:

1. Prefer direct property access forms that extern inference can see.
2. Set `:compiler-options {:infer-externs :auto}`.
3. Add explicit externs for dynamic or reflective property access.
4. Avoid advanced compilation for builds meant to be transformed by another bundler unless that is intentional.

## Stale or Strange Output

Stop the server and rebuild:

```bash
npx shadow-cljs stop
npx shadow-cljs watch app
```

If config or dependency state is corrupt, remove generated build output and `.shadow-cljs` after confirming no user files are stored there.

## Source Path Problems

Namespace `my.app.core` must live at `my/app/core.cljs` under one configured source path. Use unique top-level namespaces to avoid collisions with generic names like `app.core`.
