# shadow-cljs Workflows

## Add a Browser Frontend

1. Add `src/main` to `:source-paths`.
2. Create an app namespace with an idempotent `init`.
3. Add a `:browser` build with `:modules`.
4. Add `:dev-http` for static files.
5. Load the generated module from HTML.
6. Run `npx shadow-cljs watch app`.

```clojure
{:dev-http {8080 "public"}
 :builds
 {:app
  {:target :browser
   :output-dir "public/js"
   :asset-path "/js"
   :modules {:main {:init-fn my.app/init}}
   :devtools {:after-load my.app/render}}}}
```

`render` should tolerate being called repeatedly during hot reload.

## Production Release

```bash
npx shadow-cljs release app
```

Check that HTML references the release output path and that any assets loaded dynamically use paths valid after deployment. For browser modules, `:asset-path` must match the public URL for generated chunks.

## Add Code Splitting

```clojure
:modules
{:main {:init-fn my.app/init}
 :settings {:entries [my.app.settings]
            :depends-on #{:main}}}
```

Load the secondary module through the runtime/module loader or a normal script import depending on target and app architecture. Shared code should live in dependencies of the base module or in modules depended on by later chunks.

## Add a Node CLI

```clojure
:builds
{:cli
 {:target :node-script
  :main my.cli/main
  :output-to "out/cli.js"}}
```

```clojure
(ns my.cli)

(defn main [& args]
  (println "args:" args))
```

```bash
npx shadow-cljs release cli
node out/cli.js
```

## Add CLJS Tests

Node:

```clojure
:builds
{:test
 {:target :node-test
  :output-to "out/test.js"}}
```

Browser/Karma:

```clojure
:builds
{:karma
 {:target :karma
  :output-to "out/karma.js"}}
```

Keep test namespaces under `src/test` or a test alias included by shadow-cljs. Use browser tests when code touches DOM, browser globals, canvas, or layout APIs.

## Integrate with `deps.edn`

```clojure
;; shadow-cljs.edn
{:deps {:aliases [:dev :test]}
 :builds {:app {:target :browser
                :modules {:main {:init-fn my.app/init}}}}}
```

Keep CLJS-specific output and target config in `shadow-cljs.edn`; keep shared JVM/CLJ dependencies and dev/test aliases in `deps.edn`.

## Integrate with Polylith

Use shadow-cljs for build artifacts and Polylith for workspace structure. Include the development project or project aliases that expose the required CLJS bricks:

```clojure
{:deps {:aliases [:dev :+default]}
 :builds {:frontend {:target :browser
                     :modules {:main {:init-fn my.frontend.app/init}}}}}
```

Avoid reaching across Polylith component boundaries in CLJS namespaces; require public interface namespaces.
