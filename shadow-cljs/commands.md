# shadow-cljs Commands

## Project Setup

```bash
npx create-cljs-project my-app
cd my-app
npm install
```

Existing projects usually need both a JS package and shadow-cljs:

```bash
npm install --save-dev shadow-cljs
```

For `deps.edn` or Leiningen projects, keep the shadow-cljs version consistent with the classpath. A failed load often means another dependency pulled incompatible ClojureScript or Closure Compiler versions.

## Common Commands

```bash
npx shadow-cljs watch app       # start dev server, compile, watch, hot reload
npx shadow-cljs compile app     # one dev compile
npx shadow-cljs release app     # optimized production build
npx shadow-cljs check app       # analyzer/compiler checks without emitting final release artifact
npx shadow-cljs server          # start the long-running server
npx shadow-cljs stop            # stop the background server
npx shadow-cljs info            # print environment and build info
npx shadow-cljs help            # list commands
```

Use the build id from `shadow-cljs.edn` without the leading colon:

```clojure
:builds {:frontend {...}}
```

```bash
npx shadow-cljs watch frontend
```

## REPLs

```bash
npx shadow-cljs node-repl
npx shadow-cljs browser-repl
npx shadow-cljs cljs-repl app
npx shadow-cljs clj-repl
```

For a build REPL, run `watch` first or let `cljs-repl` connect through the running server. Browser REPLs require a page that has loaded the compiled build and connected to devtools.

## Tests

```bash
npx shadow-cljs compile test-node
node out/test-node/script.js

npx shadow-cljs compile test-karma
npx karma start
```

With package scripts:

```json
{
  "scripts": {
    "dev": "shadow-cljs watch app",
    "release": "shadow-cljs release app",
    "test": "shadow-cljs compile test-node && node out/test-node/script.js"
  }
}
```

## Server UI and Ports

`watch`, REPL, and `server` use the shadow-cljs server. The default UI port is commonly `9630`; `:dev-http` ports are configured separately in `shadow-cljs.edn`.

If a port is occupied, change `:dev-http` or stop the old server:

```bash
npx shadow-cljs stop
```
