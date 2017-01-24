# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list.
Many of these ideas are great for contributions and collaborators, so take a
look and see if anything catches your eye!

**But first**, watch [this video][citep] that outlines how collaboration works in the Elm community:

[![Code is the Easy Part](https://img.youtube.com/vi/DSjbTC-hvqQ/0.jpg)][citep]

[citep]: https://youtu.be/DSjbTC-hvqQ

Now for some projects!

<br>

## Profiling `virtual-dom` with V8 tools

The `elm-lang/virtual-dom` library is [quite fast](http://elm-lang.org/blog/blazing-fast-html-round-two). It may be possible to do better though!

**Goal:** Get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives). This would allow us to:

  1. Be sure that code is getting optimized (but not deoptimized!)
  2. Know more about how much garbage is produced.
  
Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducable part of the library. This way any potential changes could be informed by performance implications.

<br>


## Make Libraries

There are certain categories where Elm can be really great, and there may be opportunities for contribution:

  - **Data Visualization** &mdash; It seems obvious that Elm would be great for data visualization. Often folks think &ldquo;I want D3&rdquo; and then are unsure how to do it so they give up. That is a silly strategy. Instead start small. Make a library that just shows pretty time plots or bar charts. It has a `view` function that takes some data. From there, share your work and get feedback! (e.g. [elm-plot](https://terezka.github.io/elm-plot/))
  
  - **WebGL** &mdash; There is already a nice foundation for WebGL with [elm-community/webgl](https://github.com/elm-community/elm-webgl/tree/3.0.3), but there is a ton of room for projects like [Zinggi/elm-obj-loader](https://github.com/Zinggi/elm-obj-loader) for creating terrain or objects or whatever you need. I'd recommend just using WebGL a bunch as a starting point!

The best kind of library is something *you* happen to need in your daily life though. That is the best inpiration and it means you have decent understanding of the problem and a concrete use case to work towards.

> **WARNING: API design is very hard to do well.** I would make sure you have a decent amount of experience building Elm projects before making libraries that abstract that work! So after that, make sure you:
>
>  - Read [guide.elm-lang.org](https://guide.elm-lang.org/) in full.
>  - Read the [API design guidelines](http://package.elm-lang.org/help/design-guidelines).
>  - Reread [this section](https://guide.elm-lang.org/reuse/) of the guide.
>  - Follow the links and watch the videos [here](https://guide.elm-lang.org/reuse/more.html).
>
> Meanwhile, **always be looking for feedback from folks with more experience.** I have done API design sessions with a bunch of folks now. This happened with `elm-webgl` and `elm-style-animation` and `elm-css` and `elm-test` and tons of others. It takes quite a few years and a good gut to get great at making these APIs, and the best way to improve is to learn from people who have done it before.

<br>

## In-browser REPL

Some modules are best examined with the time-traveling debugger, others are best examined by poking around with certain functions. It would be extremely cool if we had an in-browser REPL so that we could do either. This would let us show `Element` and `Html` values really easily!

[The most successful effort](http://elmrepl.cuberoot.in/) runs `elm-repl` on a server, which is quite handy, but you could do a bunch of cool stuff if you could get the REPL going in Elm.

This is super free-form. Maybe inspiration can come from iPython.

<br>


## Improve Process Bot

There is [this robot](https://github.com/process-bot/) that goes around on Elm repos, trying to help issues and PRs go smoothly by letting people know about [the contribution-checklist](https://github.com/process-bot/contribution-checklist). There are two features that I think it would be great to have:

  1. Once a thread gets to 10 posts, process-bot would comment and tell people to close it down and (1) close it down and summarize the state of affairs in a new issue, (2) move the discussion somewhere else until it can be made more clear, or (3) keep talking in the closed thread until you decide to do 1 or 2.
  
  2. When a PR is opened, have process-bot check a list of people who have given contributors agreements. If they have given one, great! If not, give them a message telling them how to proceed.

The relevant code lives in [this repo](https://github.com/process-bot/contribution-checklist).


## Visualize Compilation

It would be cool if when you compile your code, you get a nice dashboard that shows you what is going on and helps you fix any problems that may exist. We could show all the dependencies of the project as a very cool progress bar:

![dependency graph progress](https://raw.githubusercontent.com/elm-lang/projects/master/compiler-progress-visualization/mock.gif)

Notice that we can keep compiling files even after one failed if they do not depend on each other. We could make it so a user would click on a red node to get a in-browser panel about that module and what the particular errors were. This way, doing a refactor does not require going back to the compiler tons of times, you just run it and fix everything that is red!

Maybe we could also add information about build statistics. Which modules are slow? Are there bottlenecks in the dependency graph that lead to less parallelization? I think this could be cool on really large projects when you want to do tricks to bring down compile times.

If you work on this, **start with the UI**. Do not worry about integration at first. I would expect the steps to be:

  - Make a library for visualizing dependency graphs. Probably with SVG.
  - Use dummy dependency graphs and script dummy events to see things flow through.

If this works, you will definitely have a cool library. From there, we can figure out how to get `elm-make` producing the necessary information.
