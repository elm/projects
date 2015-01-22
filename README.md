# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list.
Many of these ideas are great for contributions and collaborators, so take a
look and see if anything catches your eye!

## Improve Elm Reactor Navigation Page

  1. Rewrite in Elm. Right now it is done like [this](https://github.com/elm-lang/elm-reactor/blob/master/backend/Index.hs) which is a terrible shame.
  2. Improve style, perhaps taking inspiration from GitHub navigation.
  3. Improve layout and usability. Lots of people have trouble figuring out what the wrench icon does and miss out on cool features!
  4. Show README.md files with elm-markdown if they exist.
  5. Make it easy to find documentation for packages that are used *without* an internet connection.

Here is a mock of the craziest, most extreme version of this project:

![navigation mock](https://raw.githubusercontent.com/elm-lang/projects/master/elm-reactor-navigation/mock.png)

Obviously not all of this needs to be done at once! First step is probably just rewriting things in Elm.


## In-browser REPL

Some modules are best examined with the time-traveling debugger, others are best examined by poking around with certain functions. It would be extremely cool if we had an in-browser REPL so that we could do either.

This is super free-form. Maybe inspiration can come from iPython.

## elm-format

Go has had lots of success with [gofmt](http://blog.golang.org/go-fmt-your-code) and it would be cool to have the same power in Elm.

Once we have the basic version, it can be used to do other cool stuff. For example, when someone wants to upgrade from version 1.0.0 to 2.0.0 of some package, we could have a set of upgrade hints that would automatically do as much of the upgrade as possible. If the code is managed by `elm-format` then we have a guarantee that it will come out with nice style!

## elm-test

A super simple way to run all of your tests, as described [here](https://groups.google.com/forum/#!topic/elm-dev/-oC1b4KuELA).

## Visualize compilation and testing

It would be cool if when you compile your code or test your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist.

Mock coming soon!