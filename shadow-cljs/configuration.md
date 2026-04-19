# shadow-cljs Configuration

## Minimal Browser Project

```clojure
;; shadow-cljs.edn
{:source-paths ["src/main" "src/test"]

 :dependencies
 [[reagent "1.2.0"]]

 :dev-http {8080 "public"}

 :builds
 {:app
  {:target :browser
   :output-dir "public/js"
   :asset-path "/js"
   :modules {:main {:init-fn my.app/init}}}}}
```

```clojure
;; src/main/my/app.cljs
(ns my.app)

(defn init []
  (js/console.log "loaded"))
```

```html
<!-- public/index.html -->
<div id="root"></div>
<script src="/js/main.js"></script>
```

## Top-Level Keys

| Key | Use |
|-----|-----|
| `:source-paths` | CLJS source directories, usually `src/main` and `src/test` |
| `:dependencies` | Maven dependencies in Leiningen vector form |
| `:deps` | Use `deps.edn`; optionally `{:aliases [:dev :test]}` |
| `:lein` | Use Leiningen project/profile, e.g. `{:profile "+cljs"}` |
| `:dev-http` | Static dev servers started by watch/server |
| `:build-defaults` | Shared config merged into every build |
| `:builds` | Named build configs |
| `:npm-deps` | npm dependency install behavior; commonly leave default or set `{:install false}` in managed JS workspaces |
| `:jvm-opts` | JVM options such as memory limits |

Do not define the same dependency in several places unless the project intentionally uses both `deps.edn` aliases and `shadow-cljs.edn` dependencies. Prefer one source of truth.

## Build Config Keys

| Key | Use |
|-----|-----|
| `:target` | Runtime/artifact type |
| `:output-dir` | Directory for generated JS chunks |
| `:output-to` | Single output file for single-file targets |
| `:asset-path` | Browser path used to load generated chunks |
| `:modules` | Browser/ESM module graph; supports `:init-fn`, `:entries`, `:depends-on` |
| `:main` | Main namespace for script-style targets |
| `:entries` | Test or library entry namespaces |
| `:devtools` | Hot reload hooks, preloads, watch dirs, console support |
| `:compiler-options` | ClojureScript and Closure options |
| `:js-options` | npm/JS resolution and bundling options |
| `:build-hooks` | Hook functions run during the build |
| `:dev` / `:release` | Mode-specific config overrides |

## Devtools Hooks

```clojure
:devtools
{:before-load my.app/stop
 :after-load my.app/start
 :preloads [devtools.preload]}
```

Use hot reload hooks for idempotent lifecycle code. Avoid putting irreversible startup work directly in top-level forms.

## Build Defaults

```clojure
:build-defaults
{:compiler-options {:infer-externs :auto}
 :devtools {:reload-strategy :full}}
```

Keep defaults boring. Put target-specific output, modules, and JS interop settings inside each build.
