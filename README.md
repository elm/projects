# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list.
Many of these ideas are great for contributions and collaborators, so take a
look and see if anything catches your eye!


## Make elm-lang.org responsive

Right now it is not. It would be pretty great if it was! You can check out the code [here](https://github.com/elm-lang/elm-lang.org/).


## Compare Elm's virtual-dom to other stuff

I would like to update [this blog post](http://elm-lang.org/blog/blazing-fast-html) now that everyone has had a chance to speed their stuff up. The [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) implementation (rewritten from scratch for Elm 0.17) is quite fast. The [ui-perf](https://github.com/evancz/ui-perf) project shows that it compares favorably to React, but it would be good to have more reference points.

**Goal:** Add Ember, Angular 1, and Angular 2 to the [ui-perf](https://github.com/evancz/ui-perf) project. Make sure they conform to the same [methodology](https://github.com/evancz/ui-perf#methodology) as the other projects. No `requestAnimationFrame`, no skipping frames! I have been doing two versions for each project: one with no optimizations enabled, and one with &ldquo;reasonable&rdquo; optimizations. Here reasonable means &ldquo;follow standard optimization practices&rdquo; without specializing for any particular benchmark.


## Benchmark virtual-dom

The implementation of [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) should be quite fast. It may be possible to do better though!

**Goal:** Get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives). This would allow us to:

    1. Be sure that code is getting optimized (but not deoptimized!)
    2. Know more about how much garbage is produced.
  
Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducable part of the library. This way any potential changes could be informed by performance implications.


## Visualize Compilation

It would be cool if when you compile your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist. We could show all the dependencies of the project as a very cool progress bar:

![dependency graph progress](https://raw.githubusercontent.com/elm-lang/projects/master/compiler-progress-visualization/mock.gif)

Notice that we can keep compiling files even after one failed if they do not depend on each other. We could make it so a user would click on a red node to get a in-browser panel about that module and what the particular errors were. This way, doing a refactor does not require going back to the compiler tons of times, you just run it and fix everything that is red!

Maybe we could also add information about build statistics. Which modules are slow? Are there bottlenecks in the dependency graph that lead to less parallelization? I think this could be cool on really large projects when you want to do tricks to bring down compile times.

If you work on this, **start with the UI**. Do not worry about integration at first. I would expect the steps to be:

  - Make a library for visualizing dependency graphs. Probably with SVG.
  - Use dummy dependency graphs and script dummy events to see things flow through.

If this works, you will definitely have a cool library. From there, we can figure out how to get `elm-make` producing the necessary information.


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

  1. Figure out if new versions of packages exist and give notifications. Maybe have information about upgrade costs based on API diffs and number of uses of changed or removed values.
  2. Make possible to read documentation for packages *without* an internet connection.

