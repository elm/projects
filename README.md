# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list.
Many of these ideas are great for contributions and collaborators, so take a
look and see if anything catches your eye!


## Improve Elm Reactor Navigation Page

Right now, the navigation page of [Elm Reactor](http://elm-lang.org/blog/Introducing-Elm-Reactor.elm)
is not the most exciting or well-designed. Here is a mock what that navigation page *could* look like,
with a bunch of new and useful features:

![navigation mock](https://raw.githubusercontent.com/elm-lang/projects/master/elm-reactor-navigation/mock.png)

This can be broken up into much smaller and more manageable steps that are each very worthwhile:

  1. Rewrite in Elm. Right now it is done like [this](https://github.com/elm-lang/elm-reactor/blob/master/backend/Index.hs) which is a terrible shame.
  2. Improve style, perhaps taking inspiration from GitHub navigation.
  3. Improve layout and usability. Lots of people have trouble figuring out what the wrench icon does and miss out on cool features!
  4. Show README.md files with elm-markdown if they exist.
  5. Make it easy to find documentation for packages that are used *without* an internet connection.
  6. Figure out if new versions of packages exist and give notifications. Maybe have information about upgrade costs based on API diffs and number of uses of changed or removed values.

I think 5 and 6 actually are projects of their own, but they could certainly be exposed through this API.


## elm-format

Go has had lots of success with [gofmt](http://blog.golang.org/go-fmt-your-code) and it would be cool to have the same power in Elm. 

Once we have the basic version, it can be used to do other cool stuff. For example, when someone wants to upgrade from version 1.0.0 to 2.0.0 of some package, we could have a set of upgrade hints that would automatically do as much of the upgrade as possible. There is a [discussion about auto-upgrade on elm-dev](https://groups.google.com/d/topic/elm-dev/zmgGF-yljUA/discussion). If the code is managed by `elm-format` then we have a guarantee that it will come out with nice style! This is exactly how Go got [gofix](http://golang.org/cmd/fix/). 


## elm-test

A super simple way to run all of your tests, as described [here](https://groups.google.com/forum/#!topic/elm-dev/-oC1b4KuELA).


## Visualize Compilation

It would be cool if when you compile your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist. We could show all the dependencies of the project as a very cool progress bar:

![dependency graph progress](https://raw.githubusercontent.com/elm-lang/projects/master/compiler-progress-visualization/mock.gif)

Notice that we can keep compiling files even after one failed if they do not depend on each other. We could make it so a user would click on a red node to get a in-browser panel about that module and what the particular errors were. This way, doing a refactor does not require going back to the compiler tons of times, you just run it and fix everything that is red!

Maybe we could also add information about build statistics. Which modules are slow? Are there bottlenecks in the dependency graph that lead to less parallelization? I think this could be cool on really large projects when you want to do tricks to bring down compile times.


## Visualize Tests

Same idea as visualizing compilation, but instead we see progress on tests.

Tests are often hierarchical, so we can have things organized by top-level test suites. Those turn green if every sub-test is green. If any sub-test fail, we can navigate down the heirarchy until we get the particulars of that exact test.

Again, we can have speed statistics, so if some tests are super slow it is easy to discover and work on.


## Bad-Type-Error-Message Hall of Fame

It is known that type errors can come with quite confusing messages. This project is essentially an effort to create a big catalog of error messages such that we can:

1. **Discover patterns.** Are there certain things that always turn out bad? Can we focus on them? Is there an easy fix? As things stand, we have no idea about this.
2. **Evaluate fixes.** Even once we know a problem, it is quite hard to know if error messages approve across the board. Maybe we fixed the case we care about, but messed up a bunch of others!

I am imagining a tool we can run with different versions of the compiler and see all the different error messages spit out side-by-side. This would be a huge help to every future person looking to improve error messages!


## Better Type Error Messages

I saw a demo of how to improve type errors in OCaml that I found quite compelling. The talk was called "Improving Type Error Messages in OCaml" and had great results ([paper](http://ocaml.org/meetings/ocaml/2014/ocaml2014_9.pdf), [slides](http://ocaml.org/meetings/ocaml/2014/chargueraud-slides.pdf), [video](https://www.youtube.com/watch?v=V_ipQZeBueg&list=UUP9g4dLR7xt6KzCYntNqYcw)).

This was at ICFP 2014 in [the OCaml Workshop](http://ocaml.org/meetings/ocaml/2014/) which has a great focus on practical and useful topics. It is refreshing to see a room full of FP folks trying to solve real problems.


## In-browser REPL

This connects back to the Elm Reactor navigation progress a bit, but it can be done totally independently as well.

Some modules are best examined with the time-traveling debugger, others are best examined by poking around with certain functions. It would be extremely cool if we had an in-browser REPL so that we could do either. This would let us show `Element` and `Html` values really easily!

This is super free-form. Maybe inspiration can come from iPython.


## Preview Documentation

A common request from package authors is "How can I see the docs before I publish my package?" Like in [this issue](https://github.com/elm-lang/elm-lang.org/issues/169). The new package.elm-lang.org site is based on just a few .elm files, each one reading in a bunch of JSON spit out by `elm-doc`. It seems plausible to make some sort of Haskell module that can be used as a library and from the command line, so that package.elm-lang.org can use it *and* people can use it.

I think this project is something that many people would appreciate and it does not seem crazy.


## Make elm-doc better

There are [a number of issues open](https://github.com/elm-lang/elm-compiler/issues?q=is%3Aissue+is%3Aopen+elm-doc) about the rough edges of `elm-doc` that would all make for great improvements in the package publishing experience.

This is also a nice way to learn more about the compiler without taking on a massive and/or risky task.

## Organizing elm-package

The package website currently lists all libraries in alphabetical order, and all modules within a library in alphabetical order. We can do better.

A way to tag or categorize libraries would be helpful. Draw inspiration from [npm](https://www.npmjs.com/) and [rubyGems](https://rubygems.org/). The core library (and maybe other "official" libraries) can be shown prominently.

Within libraries, the maintainer should be able to specify the order of modules, and perhaps groups with headers. This will be most useful for the core libraries, to avoid the intimidating complexity at [swiftdoc](http://swiftdoc.org/).

A global search function would be nice, too.

## Dead code elimination

Elm can figure out which modules are unused and exclude them from the JS output, but it is possible to do this on the level of functions.

Here is an example where it would be a big improvement: if you use `Debug.crash` in your program it needs the `Debug` module, which needs the `Form` type which is in `Graphics.Collage` which needs `Graphics.Element`. And then you have all the `Element` and `Collage` functions in your output even thought they will never be executed.

Right now we create a dependency graph of modules. When we want module `Main` we go and grab its dependencies, their dependencies, etc. If a module is not needed, we will not run into it. One potential approach is to use the same general idea, but do it on the level of *values* instead of modules. Just try to get `Main.main` and its dependencies, their dependencies, etc. This would mean adding additional information to the `.elmi` or `.elmo` files generated during compilation.

There are other approaches, so it could also be cool to evaluate other strategies to see what works better. In any case, it seems that native modules will need to be included in their entirety because they cannot be broken down with their current design.

Thanks to @laszlopandy for this idea!


## Elm Application Scaffold

&ldquo;Ember.js has an awesome tool called `ember-cli` that assists in generating code & building assets. I'd love to eventually see Elm have everything `ember-cli` does, but for starters, it'd be nice to have a tool that scaffolds an application structure based on the existing community best practices, with tests ready to run. Of course, it can evolve with the practices. I've started this work using the Haskell library "Hi" over at http://github.com/joefiorini/hi-elm, but would love to see something like it in `elm-make`, maybe like `elm-make init`?&rdquo;

Thanks to @joefiorini for this idea!


## Elm installers

Elm is a cross-platform tool. One implication is that users of each platform have different expectations of how to install Elm. On OSX it might be `homebrew`, on Windows... etc.

Installers that would be nice to support:

- [Homebrew](http://brew.sh/) - Useful for developer-savvy OSX users.
- [Docker](https://registry.hub.docker.com) - Something official that would be maintained, the existing ones are stale. Useful in a context where Elm runs in some deployed/sandboxed setting.
- ~~OSX Installer~~ - Done
- ~~Windows Installer~~ - Done
- ...Add another if you know something that would benefit users


## Support for Google Closure Compiler advanced mode.

Google Closure Compiler's advanced mode can optimize code further by making assumptions about the JavaScript code. Unfortunately Elm's output breaks those assuptions and doesn't work with advanced mode.

Fixes needed:
 * Support obfuscation in port mashalling: [Fixed](https://github.com/elm-lang/elm-compiler/pull/877)
 * Protect public module names from being exported: [Needs work](https://github.com/elm-lang/elm-compiler/pull/877)
 * Protect 'Elm' from obfuscation: [Needs work](https://github.com/elm-lang/elm-make/pull/12)
 * Protect Elm's public API (ie. 'fullscreen', 'embed', 'worker') and ports API from obfuscation.
 * Don't use strings for field access in record updates (in [JavaScript.hs](https://github.com/elm-lang/elm-compiler/blob/f5236297c19913db12a66a3b09014e5ba2fcd8a4/src/Generate/JavaScript.hs#L92))
