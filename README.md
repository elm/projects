<hr>

**Before you get started,** watch [this video](https://youtu.be/DSjbTC-hvqQ?t=14m5s) that outlines how collaboration works in the Elm community.

<hr>
<br>

# Elm Projects

### For New Contributors

  - [Markdown Parser](#markdown-parser)
  - [Data structure evaluation website](#data-structure-evaluation-website)
  - [Data Visualization](#data-visualization)
  - [Package Search](#package-search)
  - [WebGL](#webgl)
  - [Profiling `virtual-dom`](#profiling-virtual-dom-with-v8-tools)
  - [Process Bot](#improve-process-bot)

### For Advanced Community Members

  - [Type-directed autocomplete](#type-directed-autocomplete)
  - [REPL](#in-browser-repl)
  - [Monomorphizing](#explore-monomorphizing-compilers)
  - [WebAssembly](#explore-webassembly)

Again, it is very important that you watch [this video](https://youtu.be/DSjbTC-hvqQ?t=14m5s) and read [these comments](roadmap.md) before diving into any of this stuff! Writing the code is definitely not the hard part on any of these!

* * *


<br>

## Markdown Parser

It would be great to have a markdown parser written entirely in Elm.

This is a hard project, so I recommend looking into [jgm/cheapskate](https://github.com/jgm/cheapskate) to see a very thoughtful and efficient implementation. The author is super smart and particularly knowledgable on this topic!

I would start by exploring with [`elm-tools/parser`](https://github.com/elm-tools/parser), but perhaps something more heavy duty will be necessary in the end.


<br>

## Data structure evaluation website

Over time, the community will develop various implementations of data structures with different performance characterists. In my experiences in other languages, picking “the right one” is very time consuming. So **the big goal here is to help people learn about these data structures and make the right choices for their application.**

Say we have a couple `Queue` and `PriorityQueue` implementations, all with different performance characteristics. It would be wonderful to have a website that:

  1. Points people to great learning resources on immutable queues. What is a queue? What is a priority queue? Is it a tree structure? How does that work? Etc.

  2. Lets people race the implementations for various scenarios. Ideally folks could specify the size and shape of their data on the website and race based on that. If person A has ~100 items with `Int` priorities and person B has ~10000 items with `String` priorities, I would expect them to make the different choices.

  3. Maybe show graphs of performance vs. collection size so people can quickly get a feel for how data structures would work for their expected usage.

In the end, I think this would be **an interactive website that approximates a college-level data structures course, but probably more fun!** All the learning would be directed towards practical problems that folks actually have at the moment, and it will save them a bunch of time!

> Some curration will be necessary to make this great. For example, there would have to be some process for including packages in these numbers. For example, if there is a package that is just has worse performance in all cases, it does not seem like a good use of time for people perusing this website. Ultimately, I think this project would need some coordination with core community members, so please talk about your work on the [elm-dev mailing list](https://groups.google.com/forum/#!forum/elm-dev) to get feedback as early as possible!


<br>

## Data Visualization

Elm is great for data visualization in theory, and projects like [elm-plot](https://terezka.github.io/elm-plot/) are beginning to make it great in practice!

Start by asking, “what kind of visualization do I need?” See if it exists as an Elm library. If not, create a `view` function that uses [`elm-lang/svg`](https://github.com/elm-lang/svg) to display what you need.

From there, share your work and get feedback! API design is hard, and there are many folks in the Elm community that can help you refine and improve your designs.

**Note:** Resources like [The Visual Display of Quantitative Information](https://www.edwardtufte.com/tufte/books_vdqi) by Edward Tufte are great for learning more about the *art* of visual communication. It's not just about an API!



<br>

## Package Search

The search feature of [package.elm-lang.org](http://package.elm-lang.org/) is quite rudimentary. Community members have already created “type search” [like this](http://klaftertief.github.io/elm-search/) which is really cool, but I think we would benefit from a more traditional search feature as well.

Here are some ideas:

  1. Explore full-text search. Does searching through everything in the docs give better results than just searching the name and summary?
  2. Include secondary information in rankings. How complete are the docs? How many examples? How many infix operators? This would allow transparent mechanisms for incentivizing better package design and documentation.
  3. Many searches indicate a conceptual mismatch between JS and Elm. When someone searches for “components” the best outcome is that they find some documentation, not a package that claims that this is a thing. So it might be good to detect certain search terms and give structured information above the search results. For example, a link to the relevant parts of [guide.elm-lang.org](https://guide.elm-lang.org/) or some other relevant documentation.

Ideally this service can live on its own server. It would take in JSON and give out JSON. So if the search server goes down, it does not take down the package website. This makes it a great project because it minimizes technical coordination in the early phases.

I think it makes the most sense to focus on 1 and 2, and to not get hung up on the particular details of rankings. For any official use those details will need to be carefully tweaked, so perhaps the best system is one that makes it easy to analyze docs in various ways.


<br>

## WebGL

There is already a great foundation for WebGL with [elm-community/webgl](http://package.elm-lang.org/packages/elm-community/webgl/latest), allowing scenes [like this](https://twitter.com/unsoundscapes/status/817493065405435905).

From there folks are working on projects for [terrain generation](https://twitter.com/czaplic/status/819324109674815489) and [loading 3D meshes](https://twitter.com/czaplic/status/820055313386586112).

I think we can make it really fun and easy to create 3D graphics with Elm, and it is a very deep topic. I recommend exploring topics like [picking](http://away3d.com/tutorials/Introduction_to_Mouse_Picking),  [entity-component systems](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system), [crazy shaders](https://www.shadertoy.com/), etc. by digging into a project of your own and (1) seeing what you need and (2) sharing your observations in blog posts on [/r/elm](https://www.reddit.com/r/elm/).


<br>

## Profiling `virtual-dom` with V8 tools

The `elm-lang/virtual-dom` library is [quite fast](http://elm-lang.org/blog/blazing-fast-html-round-two). It may be possible to do better though!

So the goal here is to **get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives)**. This would allow us to:

  1. Be sure that code is getting optimized (but not deoptimized!)
  2. Know more about how much garbage is produced.

Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducible part of the library. This way any potential changes could be informed by performance implications.


<br>


## Improve Process Bot

There is [this robot](https://github.com/process-bot/) that goes around on Elm repos, trying to help issues and PRs go smoothly by letting people know about [the contribution-checklist](https://github.com/process-bot/contribution-checklist). There are two features that I think it would be great to have:

  1. Once a thread gets to 10 posts, process-bot would comment and tell people to close it down and (1) close it down and summarize the state of affairs in a new issue, (2) move the discussion somewhere else until it can be made more clear, or (3) keep talking in the closed thread until you decide to do 1 or 2.

  2. When a PR is opened, have process-bot check a list of people who have given contributors agreements. If they have given one, great! If not, give them a message telling them how to proceed.

The relevant code lives in [this repo](https://github.com/process-bot/contribution-checklist).


<br>

# More Academic Stuff

There are a bunch of more speculative projects that it would be fun to look into, particularly if you are a student learning about compilers or programming languages. Please let folks know on [elm-dev](https://groups.google.com/forum/#!forum/elm-dev) if you are looking into any of these as they touch on lots of core stuff and coordination is inevitable if things go well.


<br>

## Type-Directed Autocomplete

The [elmjutsu](https://atom.io/packages/elmjutsu) plugin for Atom does some very nice autocompletion based on type information. I think we can push this farther though. For example, if we see the following code:

```elm
longestName : List User -> Int
longestName users =
  users
    |> List.map .name
    |> ...
```

In the `...` we know we want to get from `List String` to `Int` so we can suggest functions like `List.length`.

The best way to approach this problem is to try to implement the following function:

```elm
suggest : Dict String Type -> Type -> List Expr
suggest knownValues targetType =
  ...

type Type = Var String | Lambda Type Type | ...
-- simple representation of types

type Expr = Call String (List String)
-- in the simplest version, you suggest very minimal expressions
-- things like `List.foldl (+) 0`
```

By doing this, you decouple your work from other stuff. That means you can focus on *making suggestions*. If it goes well, integrating this with other things can be a separate project.

From there, perhaps we can rank suggestions based on which file or package they live in. Based on my experiences on Gmail, I am very interested to see how well we can do with simple heuristics. Assuming quality is good, this approach has many benefits over machine learning. For example, you can easily special case thing, you can tweak heuristics without retraining, and the results may be more predictable for users. Point is, focus on the simplest plausible approach before diving into fancier techniques!


<br>

## In-browser REPL

Right now our REPL only runs on the command line. It would be amazing to have an in-browser REPL so we could actually see `Html` values and have a better UI for poking around expressions and values.

[The most successful effort](http://elmrepl.cuberoot.in/) runs `elm-repl` on a server. This is quite handy, but I think we should take things like [iPython notebooks](http://nbviewer.jupyter.org/) as inspiration for the end goal.

I expect the early prototype would be a REPL for a simple lambda calculus (not connected to Elm at all) so you could test out the core UI ideas. From there, you would get in contact with community members who work on core tools and figure out what the path would be to bring it in. For example, it would be really cool if the time-travel debugger let you easily go into the REPL for any frame!


<br>

## Explore Monomorphizing Compilers

The [MLton](http://mlton.org/) project pioneered the “monomorphizing” compiler.

Normally functions like `map : (a -> b) -> List a -> List b` produce generic code that can work on *any* list, but if you happen to know that it is used as `map : (Int -> Int) -> List Int -> List Int` you could use a denser memory representation for the `List Int`, leading to quite serious performance improvements. Less indirection and less garbage generated!

This topic has a complex relationship to Elm because whole program optimization does not mix well with the kind of asset bundling you need to do in complex webapps.

**The ideal result is documentation.** What papers are relevant? What techniques are needed? Are there any Elm specific details or issues? This kind of thing can be shared as a blog post or mailing list post on [elm-dev](https://groups.google.com/d/forum/elm-dev).


<br>

## Explore WebAssembly

[WebAssembly](http://webassembly.org/) will be maturing over the next few years. Without a garbage collector, it is not viable for languages like Elm. In the meantime, there are a few questions it would be good to answer:

  - What are the facilities for representing UTF-8 strings? If you make it all from bit arrays and trees, we should do a “literature review” of how strings are represented in languages like JS, Java, Go, etc.
  - How does WebAssembly code interact with the DOM? What does that mean for Elm’s virtual-dom library?
  - How does WebAssembly interact with WebGL? What does that mean for Elm’s webgl library?

In all these cases, **the ideal result is documentation** that gets shared with [elm-dev](https://groups.google.com/d/forum/elm-dev). Before making technical decisions and investments, certain big questions must be addressed. So it probably makes sense to do some prototyping, but the actual deliverable here is knowledge.
