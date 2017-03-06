# Elm Projects for GSoC

Before you get started, read [this blog post about Elm + GSoC](http://elm-lang.org/blog/google-summer-of-code-2017) and watch [this video](https://youtu.be/DSjbTC-hvqQ?t=14m5s) that outlines how collaboration works in the Elm community.

From there, here are some projects, ordered by difficulty, that would make good GSoC projects:

  - [Package Search](#package-search)
  - [Profiling `virtual-dom`](#profiling-virtual-dom-with-v8-tools)
  - [WebAssembly](#explore-webassembly)
  - [Type-directed autocomplete](#type-directed-autocomplete)
  - [Monomorphizing](#explore-monomorphizing-compilers)
  - [Other](#other)

* * *


<br>

## Package Search

The search feature of [package.elm-lang.org](http://package.elm-lang.org/) is quite rudamentary. Community members have already created “type search” [like this](http://klaftertief.github.io/elm-search/) which is really cool, but I think we would benefit from a more traditional search feature as well.

Here are some ideas:

  1. Explore full-text search. Does searching through everything in the docs give better results than just searching the name and summary?
  2. Include secondary information in rankings. How complete are the docs? How many examples? How many infix operators? This would allow transparent mechanisms for incentivizing better package design and documentation.
  3. Many searches indicate a conceptual mismatch between JS and Elm. When someone searches for “components” the best outcome is that they find some documentation, not a package that claims that this is a thing. So it might be good to detect certain search terms and give structured information above the search results. For example, a link to the relevant parts of [guide.elm-lang.org](https://guide.elm-lang.org/) or some other relevant documentation.
  
Ideally this service can live on its own server. It would take in JSON and give out JSON. So if the search server goes down, it does not take down the package website. This makes it a great project because it minimizes technical coordination in the early phases.

I think it makes the most sense to focus on 1 and 2, and to not get hung up on the particular details of rankings. For any official use those details will need to be carefully tweaked, so perhaps the best system is one that makes it easy to analyze docs in various ways.


<br>

## Profiling `virtual-dom` with V8 tools

The `elm-lang/virtual-dom` library is [quite fast](http://elm-lang.org/blog/blazing-fast-html-round-two). It may be possible to do better though!

So the goal here is to **get [elm-lang/virtual-dom](https://github.com/elm-lang/virtual-dom/) setup with [v8-natives](https://www.npmjs.com/package/v8-natives)**. This would allow us to:

  1. Be sure that code is getting optimized (but not deoptimized!)
  2. Know more about how much garbage is produced.

Perhaps certain functions need to be broken into smaller chunks? Perhaps values can be made more monomorphic? A great result would be writeups of what you observe when testing things by hand. If that is useful, it would be best to make these tests a reproducable part of the library. This way any potential changes could be informed by performance implications.


<br>

## Explore WebAssembly

[WebAssembly](http://webassembly.org/) will be maturing over the next few years. Without a garbage collector, it is not viable for languages like Elm. In the meantime, there are a few questions it would be good to answer:

  - What are the facilities for representing UTF-8 strings? If you make it all from bit arrays and trees, we should do a “literature review” of how strings are represented in languages like JS, Java, Go, etc.
  - How does WebAssembly code interact with the DOM? What does that mean for Elm’s virtual-dom library?
  - How does WebAssembly interact with WebGL? What does that mean for Elm’s webgl library?

In all these cases, **the ideal result is documentation** that gets shared with [elm-dev](https://groups.google.com/d/forum/elm-dev). Before making technical decisions and investments, certain big questions must be addressed. So it probably makes sense to do some prototyping, but the actual deliverable here is knowledge.


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

In the `...` we know we want to get from `List String` to `Int` so we can suggest functions like `List.length`. Perhaps we can rank suggestions based on which file or package they live in. Based on my experiences on Gmail, I am very interested to see how well we can do with simple heuristics. Assuming quality is good, this approach has many benefits over machine learning. For example, you can easily special case thing, you can tweak heuristics without retraining, and the results may be more predictable for users. Point is, focus on the simplist plausible approach before diving into fancier techniques!


<br>

## Explore Monomorphizing Compilers

The [MLton](http://mlton.org/) project pioneered the “monomorphizing” compiler.

Normally functions like `map : (a -> b) -> List a -> List b` produce generic code that can work on *any* list, but if you happen to know that it is used as `map : (Int -> Int) -> List Int -> List Int` you could use a denser memory representation for the `List Int`, leading to quite serious performance improvements. Less indirection and less garbage generated!

This topic has a complex relationship to Elm because whole program optimization does not mix well with the kind of asset bundling you need to do in complex webapps.

**The ideal result is documentation.** What papers are relevant? What techniques are needed? Are there any Elm specific details or issues? This kind of thing can be shared as a blog post or mailing list post on [elm-dev](https://groups.google.com/d/forum/elm-dev).


<br>

## Other

Maybe you have a vision outside these options? Talk it through in [the #gsoc channel on slack](https://elmlang.slack.com/messages/gsoc/) to see if it sounds good.

You can also check out more ideas on [the wiki about projects and mentors](https://github.com/elm-lang/projects/wiki).
