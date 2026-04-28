# shadow-cljs Targets

## Browser

```clojure
{:target :browser
 :output-dir "public/js"
 :asset-path "/js"
 :modules {:main {:init-fn my.app/init}}}
```

Use for SPAs and browser apps. `:modules` controls generated chunk names. A module with `:init-fn` runs when that script is loaded.

Code splitting:

```clojure
:modules
{:main {:init-fn my.app/init}
 :admin {:entries [my.app.admin]
         :depends-on #{:main}}}
```

## Node Script

```clojure
{:target :node-script
 :main my.cli/main
 :output-to "out/cli.js"}
```

Use for executable Node programs. The namespace must provide the configured function.

## Node Library

```clojure
{:target :node-library
 :output-to "dist/index.js"
 :exports {:hello my.lib/hello}}
```

Use when Node/CommonJS consumers require exported functions.

## Node Tests

```clojure
{:target :node-test
 :output-to "out/node-tests.js"}
```

Compile then run with Node. Put test namespaces on `:source-paths` or test aliases.

## Karma Tests

```clojure
{:target :karma
 :output-to "out/karma-tests.js"}
```

Use when tests need browser APIs. Keep the Karma config responsible for browser launchers and the generated script path.

## ESM

```clojure
{:target :esm
 :output-dir "dist/esm"
 :modules
 {:main {:exports {init my.app/init}}}}
```

Use when another ESM-aware tool or runtime imports compiled CLJS. Development output may use `globalThis` for REPL and hot reload behavior; optimized output is the artifact to judge for consumers.

## ESM Files

```clojure
{:target :esm-files
 :output-dir "dist/esm"
 :entries [my.lib.core my.lib.extra]}
```

Use when each entry namespace should remain addressable as ESM files.

## npm Module

```clojure
{:target :npm-module
 :output-dir "dist/npm"
 :entries [my.lib.core]}
```

Use for CommonJS-style module output. For new interop with modern JS tools, consider `:esm` first.

## React Native and Expo

```clojure
{:target :react-native
 :init-fn my.mobile/init}
```

React Native builds depend on Metro and app-specific runtime setup. Keep the JavaScript package manager and native project config in charge of native dependencies.

## Chrome Extension

```clojure
{:target :chrome-extension
 :extension-dir "public/ext"
 :manifest-file "public/ext/chrome-manifest.edn"
 :outputs
 {:background {:output-type :chrome/background
               :entries [my.ext.background]}
  :content {:output-type :chrome/content-script
            :entries [my.ext.content]}}}
```

Use extension output types that match where the code runs. Browser APIs and content script isolation affect interop and testing.
