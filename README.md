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

Once we have the basic version, it can be used to do other cool stuff. For example, when someone wants to upgrade from version 1.0.0 to 2.0.0 of some package, we could have a set of upgrade hints that would automatically do as much of the upgrade as possible. If the code is managed by `elm-format` then we have a guarantee that it will come out with nice style!

## elm-test

A super simple way to run all of your tests, as described [here](https://groups.google.com/forum/#!topic/elm-dev/-oC1b4KuELA).

## Visualize Compilation

It would be cool if when you compile your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist. We could show all the dependencies of the project as a very cool progress bar:

![dependency graph progress](https://raw.githubusercontent.com/elm-lang/projects/master/compiler-progress-visualization/mock.gif)

Notice that we can keep compiling files even after one failed if they do not depend on each other. We could make it so a user would click on a red node to get a in-browser panel about that module and what the particular errors were. This way, doing a refactor does not require going back to the compiler tons of times, you just run it and fix everything that is red!

Maybe we could also add information about build statistics. Which modules are slow? Are there bottlenecks in the dependency graph that lead to less parallelization? I think this could be cool on really large projects when you want to do tricks to bring down compile times.


## Visualize Tests

Same idea as visualizing compilation, but instead we see progress on tests.

Tests are often heirarchical, so we can have things organized by top-level test suites. Those turn green if every sub-test is green. If any sub-test fail, we can navigate down the heirarchy until we get the particulars of that exact test.

Again, we can have speed statistics, so if some tests are super slow it is easy to discover and work on.

## In-browser REPL

This connects back to the Elm Reactor navigation progress a bit, but it can be done totally independently as well.

Some modules are best examined with the time-traveling debugger, others are best examined by poking around with certain functions. It would be extremely cool if we had an in-browser REPL so that we could do either. This would let us show `Element` and `Html` values really easily!

This is super free-form. Maybe inspiration can come from iPython.

## Dead code elimination

Elm can figure out which modules are unused and exclude them from the JS output, but it is possible to do this on the level of functions.

Here is an example where it would be a big improvement: if you use `Debug.crash` in your program it needs the `Debug` module, which needs the `Form` type which is in `Graphics.Collage` which needs `Graphics.Element`. And then you have all the `Element` and `Collage` functions in your output even thought they will never be executed.

Right now, we just look at which modules are needed. So the nodes in our dependency graph are modules. One way to do dead code elimination is to make this more fine grained. We can make the nodes in the dependency graph *values*. This means adding additional information to the `.elmi` or `.elmo` files so we know what is needed for each function and value. From there we can just pick the values we need!

There are other approaches, so it could also be cool to evaluate other strategies to see what works better. In any case, it seems that native modules will need to be included in their entirety because they cannot be broken down with their current design.
