<hr>

**Before you get started,** watch [this video](https://youtu.be/DSjbTC-hvqQ?t=14m5s) that outlines how collaboration works in the Elm community.

<hr>
<br>

# Elm Projects

### For New Contributors

  - [Data structure evaluation website](#data-structure-evaluation-website)
  - [Profiling `virtual-dom`](#profiling-virtual-dom-with-v8-tools)
  - [WebGL](#webgl)

### For Advanced Community Members

  - [`elm-find`](#elm-find)
  - [REPL](#in-browser-repl)
  - [Type-directed autocomplete](#type-directed-autocomplete)
  - [Monomorphizing](#explore-monomorphizing-compilers)
  - [WebAssembly](#explore-webassembly)

Again, it is very important that you watch [this video](https://youtu.be/DSjbTC-hvqQ?t=14m5s) and read [these comments](roadmap.md) before diving into any of this stuff! Writing the code is definitely not the hard part on any of these!

* * *


<br>

## Data structure evaluation website

Over time, the community will develop various implementations of data structures with different performance characterists. In my experiences in other languages, picking “the right one” is very time consuming. So **the big goal here is to help people learn about these data structures and make the right choices for their application.**

Say we have a couple `Queue` and `PriorityQueue` implementations, all with different performance characteristics. It would be wonderful to have a website that:

  1. Points people to great learning resources on immutable queues. What is a queue? What is a priority queue? Is it a tree structure? How does that work? Etc.

  2. Lets people race the implementations for various scenarios. Ideally folks could specify the size and shape of their data on the website and race based on that. If person A has ~100 items with `Int` priorities and person B has ~10000 items with `String` priorities, I would expect them to make the different choices.

  3. Maybe show graphs of performance vs. collection size so people can quickly get a feel for how data structures would work for their expected usage.

In the end, I think this would be **an interactive website that approximates a college-level data structures course, but probably more fun!** All the learning would be directed towards practical problems that folks actually have at the moment, and it will save them a bunch of time!

> Some curration will be necessary to make this great. For example, there would have to be some process for including packages in these numbers. For example, if there is a package that is just has worse performance in all cases, it does not seem like a good use of time for people perusing this website. Ultimately, I think this project would need some coordination with core community members, so please talk about your work on [discourse](https://discourse.elm-lang.org/) to get feedback as early as possible!


<br>

## Profiling `virtual-dom` with V8 tools

The `elm-lang/virtual-dom` library is [quite fast](http://elm-lang.org/blog/blazing-fast-html-round-two). It may be possible to do better though!

So the goal here is to **get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives)**. This would allow us to:

  1. Be sure that code is getting optimized (but not deoptimized!)
  2. Know more about how much garbage is produced.

Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducible part of the library. This way any potential changes could be informed by performance implications.


<br>

## WebGL

There is already a great foundation for WebGL with [elm-community/webgl](http://package.elm-lang.org/packages/elm-community/webgl/latest), allowing scenes [like this](https://twitter.com/unsoundscapes/status/817493065405435905).

From there folks are working on projects for [terrain generation](https://twitter.com/czaplic/status/819324109674815489) and [loading 3D meshes](https://twitter.com/czaplic/status/820055313386586112).

I think we can make it really fun and easy to create 3D graphics with Elm, and it is a very deep topic. I recommend exploring topics like [picking](http://away3d.com/tutorials/Introduction_to_Mouse_Picking),  [entity-component systems](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system), [fancy shaders](https://www.shadertoy.com/), etc. by digging into a project of your own and (1) seeing what you need and (2) sharing your observations in blog posts on [/r/elm](https://www.reddit.com/r/elm/).


<br>


# More Advanced Projects

There are a bunch of more speculative projects that it would be fun to look into.

These are probably best for people who are quite experienced with Elm. Maybe who have been using it for a year or more. Long enough to have a good sense of the design aesthetic of core Elm projects.

<br>


## `elm-find`

Create a command line tool named `elm-find` that is all about quickly finding definitions and usages.

```
$ elm-find Fake.thing
[]
```

```
$ elm-find toFullName
[
    {
        "definition": {
            "isLocal": true,
            "path": "src/Student.elm",
            "module": "Student",
            "region": {
                "start": { "row": 22, "col": 0 }
                "end": { "row": 34, "col": 0 }
            }
        },
        "uses": []
    }
]
```

```
$ elm-find String.reverse
[
    {
        "definition": {
            "isLocal": false,
            "package": "elm/core",
            "module": "String"
        },
        "uses": [
            {
                "path": "src/Main.elm",
                "module": "Main",
                "region": {
                    "start": { "row": 22, "col": 14 }
                    "end": { "row": 22, "col": 20 }
                }
            }
        ]
    }
]
```

The implementation should focus on being as fast as possible. Think `grep`. The goal would be for commands to finish in under 500ms on 100k line projects, but I think it's possible to do a lot better.

This could be used to make a bunch of helpful tools:

- A command line tool to do bulk renames in large projects.
- Editor integrations for "Jump to Definition" and bulk renames.

I would recommend implementing this by starting with the existing compiler and stripping out anything that is not needed. This would give you a good foundation for crawling files efficiently, making nice command line arguments, and parsing very quickly.

<br>


## In-browser REPL

Right now our REPL only runs on the command line. It would be amazing to have an in-browser REPL so we could actually see `Html` values and have a better UI for poking around expressions and values.

[The most successful effort](http://elmrepl.cuberoot.in/) runs `elm-repl` on a server. This is quite handy, but I think we should take things like [iPython notebooks](http://nbviewer.jupyter.org/) as inspiration for the end goal.

I expect the early prototype would be a REPL for a simple lambda calculus (not connected to Elm at all) so you could test out the core UI ideas. From there, you would get in contact with community members who work on core tools and figure out what the path would be to bring it in. For example, it would be really cool if the time-travel debugger let you easily go into the REPL for any frame!

<br>


## Type-Directed Autocomplete

If we see the following code:

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

From there, perhaps we can rank suggestions based on which file or package they live in.

**Note:** Based on my experiences working on the Gmail team, I am very interested to see how well we can do with simple heuristics. Assuming quality is good, this approach has many benefits over machine learning. For example, you can easily special case thing, you can tweak heuristics without retraining, and the results may be more predictable for users. Point is, focus on the simplest plausible approach before diving into fancier techniques!

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

In all these cases, **the ideal result is documentation** that gets shared with [discourse](https://discourse.elm-lang.org/). Before making technical decisions and investments, certain big questions must be addressed. So it probably makes sense to do some prototyping, but the actual deliverable here is knowledge.
