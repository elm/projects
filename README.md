# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list.
Many of these ideas are great for contributions and collaborators, so take a
look and see if anything catches your eye!


## Make elm-lang.org responsive

Right now it is not. It would be pretty great if it was! You can check out the code [here](https://github.com/elm-lang/elm-lang.org/).


## Visualize Compilation

It would be cool if when you compile your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist. We could show all the dependencies of the project as a very cool progress bar:

![dependency graph progress](https://raw.githubusercontent.com/elm-lang/projects/master/compiler-progress-visualization/mock.gif)

Notice that we can keep compiling files even after one failed if they do not depend on each other. We could make it so a user would click on a red node to get a in-browser panel about that module and what the particular errors were. This way, doing a refactor does not require going back to the compiler tons of times, you just run it and fix everything that is red!

Maybe we could also add information about build statistics. Which modules are slow? Are there bottlenecks in the dependency graph that lead to less parallelization? I think this could be cool on really large projects when you want to do tricks to bring down compile times.

If you work on this, assume `elm-make` is producing a dependency graph and spits out info about each module as compilation happens, and then work on the UI. That information about compilation is in the pipeline for `elm-make` so do not block on it!


## Visualize Tests

Same idea as visualizing compilation, but instead we see progress on tests.

Tests are often heirarchical, so we can have things organized by top-level test suites. Those turn green if every sub-test is green. If any sub-test fail, we can navigate down the heirarchy until we get the particulars of that exact test.

Again, we can have speed statistics, so if some tests are super slow it is easy to discover and work on.


## In-browser REPL

This connects back to the Elm Reactor navigation progress a bit, but it can be done totally independently as well.

Some modules are best examined with the time-traveling debugger, others are best examined by poking around with certain functions. It would be extremely cool if we had an in-browser REPL so that we could do either. This would let us show `Element` and `Html` values really easily!

This is super free-form. Maybe inspiration can come from iPython.


## Improve Elm Reactor Navigation Page

Thanks to @JustusAdam and this repo, [Elm Reactor](http://elm-lang.org/blog/time-travel-made-easy)'s navigation page has undergone a really nice visual overhaul!

![New Navigation Page](https://cloud.githubusercontent.com/assets/1658058/9261514/405ec898-41c3-11e5-9bc5-22eefb082743.png)

There are still some cool things we can do though!

  1. Make possible to read documentation for packages *without* an internet connection.
  2. Figure out if new versions of packages exist and give notifications. Maybe have information about upgrade costs based on API diffs and number of uses of changed or removed values.


## Elm installers

Elm is a cross-platform tool. One implication is that users of each platform have different expectations of how to install Elm.

Installers that would be nice to support:

- Arch
- NixOS
- [Docker](https://registry.hub.docker.com) - Something official that would be maintained, the existing ones are stale. Useful in a context where Elm runs in some deployed/sandboxed setting.
- ...Add another if you know something that would benefit users


## Elm Application Scaffold

&ldquo;Ember.js has an awesome tool called `ember-cli` that assists in generating code & building assets. I'd love to eventually see Elm have everything `ember-cli` does, but for starters, it'd be nice to have a tool that scaffolds an application structure based on the existing community best practices, with tests ready to run. Of course, it can evolve with the practices. I've started this work using the Haskell library "Hi" over at http://github.com/joefiorini/hi-elm, but would love to see something like it in `elm-make`, maybe like `elm-make init`?&rdquo;

Thanks to @joefiorini for this idea!


## Support for Google Closure Compiler advanced mode.

Google Closure Compiler's advanced mode can optimize code further by making assumptions about the JavaScript code. Unfortunately Elm's output breaks those assuptions and doesn't work with advanced mode.

Fixes needed:
 * Support obfuscation in port mashalling: [Fixed](https://github.com/elm-lang/elm-compiler/pull/877)
 * Protect public module names from being exported: [Needs work](https://github.com/elm-lang/elm-compiler/pull/877)
 * Protect 'Elm' from obfuscation: [Needs work](https://github.com/elm-lang/elm-make/pull/12)
 * Protect Elm's public API (ie. 'fullscreen', 'embed', 'worker') and ports API from obfuscation.
 * Don't use strings for field access in record updates (in [JavaScript.hs](https://github.com/elm-lang/elm-compiler/blob/f5236297c19913db12a66a3b09014e5ba2fcd8a4/src/Generate/JavaScript.hs#L92))
