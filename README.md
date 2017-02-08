# Elm Projects

This is a big list of Elm projects that are somewhere on a very long todo list. Many of these ideas are great for contributions and collaborators, so take a look and see if anything catches your eye!

But first, **watch [this video][citep] that outlines how collaboration works in the Elm community.**

[citep]: https://youtu.be/DSjbTC-hvqQ

Now for some projects!


<br>

## WebGL

[![Triangle](http://webgl.elm-community.org/examples/screenshots/triangle.jpg)](http://webgl.elm-community.org/examples/triangle.html)
[![Cube](http://webgl.elm-community.org/examples/screenshots/cube.jpg)](http://webgl.elm-community.org/examples/cube.html)
[![Crate](http://webgl.elm-community.org/examples/screenshots/crate.jpg)](http://webgl.elm-community.org/examples/crate.html)
[![Thwomp](http://webgl.elm-community.org/examples/screenshots/thwomp.jpg)](http://webgl.elm-community.org/examples/thwomp.html)
[![FirstPerson](http://webgl.elm-community.org/examples/screenshots/first-person.jpg)](http://webgl.elm-community.org/examples/first-person.html)

There is already a great foundation for WebGL with [elm-community/webgl](http://package.elm-lang.org/packages/elm-community/webgl/latest), allowing scenes [like this](https://twitter.com/unsoundscapes/status/817493065405435905).

From there folks are working on projects for [terrain generation](https://twitter.com/czaplic/status/819324109674815489) and [loading 3D meshes](https://twitter.com/czaplic/status/820055313386586112).

I think we can make it really fun and easy to create 3D graphics with Elm, and it is a very deep topic. I recommend exploring topics like [picking](http://away3d.com/tutorials/Introduction_to_Mouse_Picking),  [entity-component systems](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system), [crazy shaders](https://www.shadertoy.com/), etc. by digging into a project of your own and (1) seeing what you need and (2) sharing your observations in blog posts on [/r/elm](reddit.com/r/elm).


<br>

## Data Visualization

It seems obvious that Elm would be great for data visualization. Often folks think &ldquo;I want D3&rdquo; and then are unsure how to do it so they give up. That is a silly strategy. Instead start small!

Just start by showing pretty time plots or bar charts. It has a `view` function that takes some data. From there, share your work and get feedback! API design is hard, and there are many folks in the Elm community that can help you refine and improve your designs.

Good examples in this area include [elm-plot](https://terezka.github.io/elm-plot/)!


<br>

## Profiling `virtual-dom` with V8 tools

The `elm-lang/virtual-dom` library is [quite fast](http://elm-lang.org/blog/blazing-fast-html-round-two). It may be possible to do better though!

**Goal:** Get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives). This would allow us to:

  1. Be sure that code is getting optimized (but not deoptimized!)
  2. Know more about how much garbage is produced.
  
Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducable part of the library. This way any potential changes could be informed by performance implications.


<br>


## Improve Process Bot

There is [this robot](https://github.com/process-bot/) that goes around on Elm repos, trying to help issues and PRs go smoothly by letting people know about [the contribution-checklist](https://github.com/process-bot/contribution-checklist). There are two features that I think it would be great to have:

  1. Once a thread gets to 10 posts, process-bot would comment and tell people to close it down and (1) close it down and summarize the state of affairs in a new issue, (2) move the discussion somewhere else until it can be made more clear, or (3) keep talking in the closed thread until you decide to do 1 or 2.
  
  2. When a PR is opened, have process-bot check a list of people who have given contributors agreements. If they have given one, great! If not, give them a message telling them how to proceed.

The relevant code lives in [this repo](https://github.com/process-bot/contribution-checklist).

<br>


# More Academic Stuff

There are a bunch of more speculative projects that it would be fun to look into, particularly if you are a student learning about compilers or programming languages. Please let folks know on [elm-dev](https://groups.google.com/forum/#!forum/elm-dev) if you are looking into any of these as they touch on lots of core stuff.


<br>

## In-browser REPL

Right now our REPL only runs on the command line. It would be amazing to have an in-browser REPL so we could actually see `Html` values and have a better UI for poking around expressions and values.

[The most successful effort](http://elmrepl.cuberoot.in/) runs `elm-repl` on a server. This is quite handy, but I think we should take things like [iPython notebooks](http://nbviewer.jupyter.org/) as inspiration for the end goal.

I expect the early prototype would be a REPL for a simple lambda calculus (not connected to Elm at all) so you could test out the core UI ideas. From there, you would get in contact with community members who work on core tools and figure out what the path would be to bring it in. For example, it would be really cool if the time-travel debugger let you easily go into the REPL for any frame!


<br>

## Explore Monomorphizing Compilers

The [MLton](http://mlton.org/) project pioneered the “monomorphizing” compiler.

Normally functions like `map : (a -> b) -> List a -> List b` produce generic code that can work on *any* list, but if you happen to know that it is used as `map : (Int -> Int) -> List Int -> List Int` you could use a denser memory representation for the `List Int`, leading to quite serious performance improvements. Less indirection and less garbage generated!

This topic has a complex relationship to Elm because whole program optimization does not mix well with the kind of asset bundling you need to do in complex webapps. Point is, it is worth learning more about this topic and sharing your thoughts about what it'd take to do this in Elm with the [elm-dev](https://groups.google.com/d/forum/elm-dev) mailing list.
