# dot-clojure

This is my personal `.clojure/deps.edn` file providing useful `clj` aliases drawn from a variety of projects. It is published to GitHub so I can keep all my computers sync'd up -- and to provide a range of examples that folks new to the Clojure CLI might find helpful.

**Several git dependencies here assume you have at least Clojure CLI 1.10.3.933!**

In addition, my `.clojure/tools/` folder is also here, containing the tools that I've installed globally, via the latest Clojure CLI (1.10.3.967) -- see [Tool installation and invocation](https://clojure.org/reference/deps_and_cli#tool_install) in the Deps and CLI Reference. As I add global tools, I am removing them as aliases.

The main alias I use here is `:dev/repl` which starts various combinations of REPL tooling. See [**The `:dev/repl` Alias**](#the-devrepl-alias) below for more details.

_Since it is my personal file, it may make assumptions about my own environment. For example, it uses `"RELEASE"` for several tools so I can always get the latest stable version of any dev/test tool I use. I make no effort at backward-compatibility and may add, delete, or change aliases as they benefit me personally. Caveat Programmer!_

**If you want a really well-documented, well-maintained alternative that actually tracks versions of tools, I would recommend you use the [Practicalli Clojure `deps.edn`](https://github.com/practicalli/clojure-deps-edn) project instead!**

With that caveat out of the way, here is some basic documentation about my tools and aliases (there are additional examples in the comments in the `deps.edn` file itself). _Note: I have recently cleaned this file up and removed a lot of aliases I no longer use!_

## Basic Tools

These are installed via `clojure -Ttools install ...` and usable via `clojure -T` with the tool name.

> Note the following renamings compared to previous `dot-clojure` versions: `new` => `clj-new`, `deps-new` => `new` (as I am focusing work on `deps-new` and want that to be my default `-Tnew` command).

* `new` -- the latest **work-in-progress** version of [deps-new](https://github.com/seancorfield/deps-new) to create new CLI/`deps.edn` projects: _[This uses a different (simpler!) templating system to `clj-new`, below, and therefore does not recognize Leiningen or Boot templates!]_
  * `clojure -Tnew app :name myname/myapp` -- creates a new `deps.edn`-based application project,
  * `clojure -Tnew lib :name myname/mylib` -- creates a new `deps.edn`-based library project,
  * `clojure -Tnew template :name myname/mytemplate` -- creates a new `deps.edn`-based template project,
  * `clojure -A:somealias -Tnew create :template some/thing :name myname/myapp` -- locates a template for `some/thing` on the classpath, based on `:somealias`, and uses it to create a new `deps.edn`-based project,
  * `clojure -A:deps -Tnew help/doc` -- for more information and other functions.
* `poly` -- a recent `shell` version of [Polylith's `poly` tool](https://github.com/polyfy/polylith) for working with Polylith projects:
  * `clojure -Tpoly shell` -- start an interactive Polylith shell,
  * `clojure -Tpoly info :loc true` -- display information about a Polylith workspace, including lines of code,
  * `clojure -Tpoly create c user` -- create a `user` component in a Polylith workspace,
  * `clojure -Tpoly test :dev true` -- run tests in the `dev` project context, in a Polylith workspace,
  * `clojure -A:deps -Tpoly help/doc` -- for more information and other functions.
* `nvd` -- based on a pull request to allow the [nvd-clojure](https://github.com/rm-hull/nvd-clojure) security checker to be installed as a tool:
  * `clojure -Tnvd check :classpath '"'$(clojure -Spath)'"'` will check all the JARs on that classpath for security vulnerabilities.
* `clj-new` -- the latest stable release of [clj-new](https://github.com/seancorfield/clj-new) to create new projects from (Leiningen and other) templates:
  * `clojure -Tclj-new app :name myname/myapp` -- creates a new `deps.edn`-based application project (but still uses `depstar` for uberjar building -- this will change soon to `tools.build`!),
  * `clojure -Tclj-new lib :name myname/mylib` -- creates a new `deps.edn`-based library project (but still uses `depstar` for JAR building -- this will change soon to `tools.build`!),
  * `clojure -Tclj-new template :name myname/mytemplate` -- creates a new `deps.edn`-based template project (but still uses `depstar` for JAR building -- this will change soon to `tools.build`!),
  * `clojure -Tclj-new create :template something :name myname/myapp` -- locates a template for `something` and uses it to create a new project (which might be `deps.edn`-based or `lein`-based, depending on the template),
  * `clojure -A:deps -Tclj-new help/doc` -- for more information and other functions.

More tools will be added to this section over time (as more tools add `:tools/usage` to their `deps.edn` files).

## Basic Aliases

Deploy jar files (if you don't have a `build.clj` file):
* `:deploy` -- pulls in and runs the latest stable release of Erik Assum's [deps-deploy](https://github.com/slipset/deps-deploy) and deploys the specified JAR file to Clojars, based on your `pom.xml` and the `CLOJARS_USERNAME` and `CLOJARS_PASSWORD` environment variables; `clojure -X:deploy :artifact '"MyProject.jar"'`

There are aliases to pull in various useful testing and debugging tools:
* `:test` -- adds both `test` and `src/test/clojure` to your classpath and pulls in the latest stable version of `test.check`
* `:runner` -- pulls in [Cognitect Labs' `test-runner`](https://github.com/cognitect-labs/test-runner) project and runs any tests it can find
* `:eastwood` -- pulls in the latest stable release of [Eastwood](https://github.com/jonase/eastwood) on your `src` and `test` folders; use with `:test` above
* `:check` -- pulls in [Athos' Check](https://github.com/athos/clj-check) project to compile all your namespaces to check for syntax errors and reflection warnings like `lein check`
* `:expect` -- pulls in the latest stable release of [expectations/clojure-test](https://github.com/clojure-expectations/clojure-test) -- the `clojure.test`-compatible version of Expectations
* `:bench` -- pulls in the latest stable release of [Criterium](https://github.com/hugoduncan/criterium/) for benchmarking your code
* `:outdated` -- pulls in and runs the latest stable release of [antq](https://github.com/antq/antq) and reports on outdated dependencies
* `:nvd` -- pulls in the latest stable release of [NVD for Clojure](https://github.com/rm-hull/nvd-clojure) for checking CVE vulnerabilities

There are aliases to pull in and start various REPL-related tools:
* `:dev/repl` -- depending on what is on your classpath, start Rebel Readline, with a Socket REPL (on "port 0" which will dynamically select an available port and print it out), but `SOCKET_REPL_PORT` env var overrides, saves port to `.socket-repl-port` file for next time);
  * usage:
    * `clj -M:portal:dev/repl` or
    * `clojure -M:rebel:dev/repl` or
    * `clojure -M:rebel:portal:dev/repl` or
  * Also works with Figwheel Main (now that I've started doing ClojureScript!):
    * `clojure -M:portal:fig:build:dev/repl` or
* `:classes` -- adds the `classes` folder to your classpath to pick up compiled code (e.g., see https://clojure.org/guides/dev_startup_time)
* `:socket` -- starts a Socket REPL on port 50505; can be combined with other aliases since this is just a JVM option
* `:rebel` -- starts a [Rebel Readline](https://github.com/bhauman/rebel-readline) REPL

* `:jedi-time` -- adds `datafy`/`nav` support for Java Time objects via [jedi-time](https://github.com/jimpil/jedi-time)
* `:portal` -- pulls in the latest stable release of the [Portal](https://github.com/djblue/portal) data visualization tool -- see the Portal web site for usage options
* `:reflect` -- adds Stuart Halloway's reflector utility (best used with Portal)

There are aliases to pull in specific versions of Clojure:
* `:master` -- Clojure 1.11.0-master-SNAPSHOT
* `:1.11` -- Clojure 1.11.0-alpha2 with `:as-alias`, `update-keys`, `update-vals` (Alpha 1 introduced the new named argument calling feature)
* `:1.10` -- Clojure 1.10.3
  * `:1.10.2` -- Clojure 1.10.2
  * `:1.10.1` -- Clojure 1.10.1
  * `:1.10.0` -- Clojure 1.10.0
* `:1.9` -- Clojure 1.9.0
* `:1.8` -- Clojure 1.8.0
* ... back to `:1.0` (note: `:1.5` is actually Clojure 1.5.1 to avoid a bug in Clojure 1.5.0, and `:1.2` is 1.2.1)

For the _EXPERIMENTAL_ `add-libs` function (`clojure.tools.deps.alpha.repl/add-libs`):
* `:add-libs` -- pulls in the `add-lib3` branch of [org.clojure/tools.deps.alpha](https://github.com/clojure/tools.deps.alpha); see the example `load-master` function in the comments in my `deps.edn`; this was previously called `:deps` but I realized that conflicted with the default `:deps` alias in the Clojure CLI install; be aware that `add-libs` is unsupported and likely to break or go away as `tools.deps.alpha` and Clojure both evolve. _[recently renamed from `:add-lib` to `:add-libs` to reflect the name change in the `add-lib3` branch!]_

## The `:dev/repl` Alias

The `:dev/repl` alias uses `load-file` to load the [`dev.clj` file](https://github.com/seancorfield/dot-clojure/blob/develop/dev.clj) from this repo. That does a number of things (see the `start-repl` docstring for more details):

* Starts a Socket REPL server (with the port selected via an environment variable, a JVM property, or a dot-file created on a previous run).
* Starts [Figwheel Main](https://github.com/bhauman/figwheel-main), if present on the classpath, else
* Starts [Rebel Readline](https://github.com/bhauman/rebel-readline), if present on the classpath.

### Portal Custom Commands

* `dev/->html` -- attempts to `slurp` the value (a filename or URL) and display it inline as HTML with a white background (so web pages are readable even in dark themes),
* `dev/->map` -- pours the value into a hash map (useful for dealing with Java `Properties` objects etc),
* `dev/->set` -- pours the value into a set,
* `dev/->vector` -- pours the value into a vector.

## Use with Figwheel

If you are doing ClojureScript development with Figwheel (`figwheel-main`) then you can do:

```
clojure -M:portal:fig:build:dev/repl
```

You'll get the regular Figwheel build REPL (for ClojureScript, which uses Rebel Readline) and a browser open on your application, plus a Socket REPL on an available port (or whatever your env says, for Clojure evaluation).

Connect to the Socket REPL, write your code as `.cljc` files, and you'll have the full power of your editor, Portal, and Figwheel! What you evaluate in your editor will be treated as Clojure code (and can be `tap>`'d into Portal, for example). What you evaluate at the REPL itself will be treated as ClojureScript code (and will affect your application instead).
