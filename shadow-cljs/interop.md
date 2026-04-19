# npm, ESM, and JavaScript Interop

## npm Packages

Install JS packages with the project package manager:

```bash
npm install react react-dom
```

Require packages by string:

```clojure
(ns my.app
  (:require ["react" :as react]
            ["react-dom/client" :as rdom]))
```

Do not add npm packages to Maven dependencies. `package.json` and the lockfile should describe the JavaScript dependency graph.

## JS Resolution

Common `:js-options` uses:

```clojure
:js-options
{:js-package-dirs ["node_modules"]
 :resolve {"react" {:target :npm :require "preact/compat"}
           "react-dom" {:target :npm :require "preact/compat"}}
 :export-conditions ["browser" "module" "main"]}
```

Use `:resolve` for aliases, globals, file replacements, or packages with awkward entry points. Prefer explicit config over patching generated output.

## Globals and Externals

For a library loaded separately by a script tag:

```clojure
:js-options
{:resolve {"react" {:target :global :global "React"}}}
```

If advanced compilation renames JS properties incorrectly, first try:

```clojure
:compiler-options {:infer-externs :auto}
```

Add externs only for actual dynamic JS property access that inference cannot see.

## ESM Interop

With `:target :esm`, export selected CLJS vars:

```clojure
{:target :esm
 :output-dir "dist"
 :modules
 {:main {:exports {mount my.app/mount
                   unmount my.app/unmount}}}}
```

JS consumers can import the release output. Avoid publishing many independent CLJS packages that each bundle their own `cljs.core`; values from separate CLJS runtimes may not interoperate as expected.

Runtime ESM imports can be left to the host with the `esm:` prefix:

```clojure
(ns my.app
  (:require ["esm:../widget.js" :as widget]))
```

The path is resolved relative to generated output, not the source file.

## Browser Asset Paths

For browser builds:

```clojure
{:output-dir "public/js"
 :asset-path "/js"}
```

`output-dir` is a filesystem path. `asset-path` is the URL prefix used by the browser for generated chunks. Wrong `asset-path` is a common cause of release-only chunk loading failures.

## JS Tooling

When combining shadow-cljs with Vite, webpack, Metro, or another JS tool:

- Let shadow-cljs own CLJS compilation.
- Let the JS tool own app bundling, dev proxying, or native packaging.
- Use `:esm` or `:npm-module` when the JS tool should consume compiled CLJS.
- Keep generated output out of source directories unless the tool explicitly expects it.
