# Roadmap FAQ

People want to know what is going to happen with Elm. Hopefully this document helps!

  - [What is the timeline?](#what-is-the-timeline)
  - [How do I make a single-page app?](#how-do-i-make-a-single-page-app)
  - [Where is the localStorage package?](#where-is-the-localstorage-package)
  - [Can I use Elm on servers?](#can-i-use-elm-on-servers)
  - [When will Elm compile to X?](#when-will-elm-compile-to-x)

If you are looking for ways to contribute, please check out [the suggestions](README.md) in this repo.

<br>


## What is the timeline?

**There is no timeline with exact dates, but we know what is on the horizon.**

A lot of people think that (1) languages are just features that need implementing, (2) these features should be placed on a calendar, and (3) we should throw people at the features until they are done early. This is not how Elm has developed!

First, when it comes to language design, I think doing things *right* is far more valuable than doing things *right now*. It is easy to think short term and rush features, but languages live for decades, and there are no take-backs on mistakes.

Second, looking back, playfulness and inspiration have been crucial parts of Elm’s development. For example, the friendly error messages began with a project to provide JSON output to code editors. There was no huge demand for JSON output, but after spending a bit of time on the project, it became clear that the error messages could be dramatically improved. No one knew that was possible! That means no one was asking for it either! So by any reasonable prioritization scheme, this was a “pointless” project, but it turned into an extremely valuable part of Elm.

Point is, I think scheduling progress is kind of a silly idea. “That conceptual breakthrough is scheduled for September!” Listening and experimenting are a part of the process. It has been extremely valuable to Elm so far, and I expect that to continue to be true.

<br>


## How do I make a single-page app?

**You can get 90% of the way there now, and 0.19 will smooth this out significantly.**

The major focus of 0.19 will be creating “single-page apps” in Elm. The features that fall under that umbrella include:

  - Server-side rendering &mdash; sending HTML with the initial response
  - Tree shaking &mdash; trimming out unused code (usually called dead code elimination or DCE)
  - Code splitting &mdash; cutting up code into smaller chunks for better caching
  - Lazy loading &mdash; only sending the code chunks needed for a particular page

Once it is possible to create “real” single-page apps thanks to these features, we will also have friendly documentation about how to do it.

> **Note:** It is still too early to say exactly which features will be in the release and how everything will work. Perhaps not all of these features *should* be in the first iteration. Maybe if one is cut, the others can be realesed earlier. That kind of thing. Point is, this list is more about what the major concerns are.

Now some people evaluating Elm may *need* need these features for a particular project. Perhaps every millisecond of page load has significant impact on revenue? I recommend measuring things. If your Elm bundle is 30kb gzipped, you may actually be competitive with other options. But imagining that it cannot work for you, it is a totally reasonable choice to hold off on using Elm for now. We will get there, and I hope you circle back after 0.19 and evaluate Elm again. That’s what [Scott is doing](https://twitter.com/scottcorgan/status/857586663261949954)!

Most cases are not nearly so extreme. If you are one of the 99% of developers with different tradeoffs, I recommend treating each “page” as a separate Elm module, and then having a “routing” module that uses [elm-lang/navigation](https://github.com/elm-lang/navigation), [evancz/url-parser](https://github.com/evancz/url-parser), `Cmd.map`, and `Sub.map` to swap between pages. This will set you up well for 0.19 which will provide a much nicer alternative to that “routing” layer.

<br>


## Where is the localStorage package?

**Expanding “web platform” support is a high priority, just behind the SPA stuff.**

First, many people think expanding “web platform” support is easy. “Just copy the JS API into Elm as tasks!” The whole point of Elm is to rethink common problems and try to do better. For example, Elm existed for more than *two years* before we added HTML support. Rather than having a fragmented ecosystem of competing HTML libraries of varying quality, we have one library that is great. For many Elm users, the fact that there are clear defaults that work well is a huge draw.

Second, the general policy is to prioritize things that *cannot be done* over things that *could be done better*. Obviously it would be great if the whole “web platform” was available in Elm today, but anything that is missing can be done with [ports](https://guide.elm-lang.org/interop/javascript.html). And again, after the SPA stuff is done, expanding the “web platform” seems like the highest priority.

> **Note:** When using ports for something complex, it is often helpful to create a client/server relationship between Elm and JS. Have one port out that gives `Json.Encode.Value` representing some request and have one port in that gives `Json.Decode.Value` that gives responses. So rather than trying to wrap each JS function independently, you draw a clear abstraction boundary between Elm and JS.

Third, a great deal of work has actually gone into `localStorage` and IndexedDB already, but the results were not good enough to be released. If a library like this is released, it will need to be supported forever because we cannot just switch to a better API when we figure it out. Programmers may store important information on thousands of computers out in the world, and losing that data could hurt their business. So until we can offer a really solid approach here (one that is cross-browser, will grow with your application, and can upgrade browser data with new releases) we prefer to be clear that we do not offer this yet.

Maybe these prioritization choices do not appeal to your sensibility or today’s tradeoffs do not seem to work for your scenario. That is fine! It is totally reasonable to circle back to Elm after “web platform” support has expanded and see if it works better for you then.

> **Note:** Many people wonder “why not have the community expand the web platform?” I have tried to address this question [here](https://groups.google.com/d/msg/elm-dev/1JW6wknkDIo/H9ZnS71BCAAJ) and [here](https://groups.google.com/d/msg/elm-dev/bAHD_8PbgKE/X-z67wTdCAAJ). There are other languages out there that make different choices on this question, and I recommend checking them out. They may suit you better!

<br>


## Can I use Elm on servers?

**No, and if we started today, this would still be a multi-year project.**

There is a great deal of excitement about Elm on servers. I think this is partially due to a couple common misconceptions about running the same language on the front-end and back-end:

  * Folks want to share types. No more JSON! That *sounds* amazing, but what happens when a user keeps a tab open for a while? If you share types directly, changing types will break their code. Creating a serialization format that can evolve with your product is a very hard problem, [as elaborated upon here](notes/on-sharing-types.md), and simply “having types” does not solve it.

  * People want server-side rendering of HTML. Running a server all in Elm does not really give you this. I mean, it can, but it is way more complicated than having a tool that can take Elm modules and spit out HTML. The simpler version can work with any existing server, which is why that is the approach being explored for 0.19!

  * Folks like to think languages are “general-purpose” and if the language is good, it should be good at *anything*. People say C++ is a general-purpose language, but it is not so nice for web apps. People say Python is a general purpose language, but it is not so good for operating systems. And you cannot just “make Python good at operating systems” by compiling it to C. More notes on “general-purpose” as a questionable concept [here](notes/on-general-purpose.md).

I think the main thing you get from “Elm on servers” is the ability to share some libraries and to have common tooling. That is still pretty neat, and perhaps worthwhile. That said, **I think it is a better strategy to make Elm an extremely strong choice for web apps before expanding to other domains.** There is still a lot of work to do for web apps!

And before you try to do it yourself...

<br>


## When will Elm compile to X?

**Compiling through X probably does not accomplish what you hope.**

Many folks tell me “Elm should compile to X” where X is a thing they like. Here are people suggesting [Go](https://twitter.com/zvozin/status/847860742787223553), [Lua](https://groups.google.com/d/msg/elm-dev/Mi9j3nVD5NE/11akZGmNAgAJ) and [Erlang](https://groups.google.com/d/msg/elm-dev/Mi9j3nVD5NE/Pf1GXS2QAgAJ). But why not go through OCaml or C# or Java or or Scala or F# or Haskell or ES6 or C++ or Rust or node.js?

**The hard part of supporting a domain is not the compiler. It is making a good ecosystem.** Python is nice for scientific computing because of things like NumPy and SciPy, not because of whatever backend. Elm is nice for front-end because of the ecosystem, like the HTML library and The Elm Architecture, not the particulars of code generation. **Just putting a typed functional language in a domain does not mean it will be fun and productive in practice!**

So say we choose to go through Erlang to make Elm a server language too. Great. Now the question is “how do you write a typed server with no side-effects that is (1) true to the spirit of Elm and (2) feels nicer than other server languages?” That's the hard question, and the answer may be that it cannot be done. So writing a compiler backend is like 5% of what it takes to make a project like this worthwhile. And ultimately, the ideal form of a project like this has its own bespoke backend, going to assembly directly.

If you want to explore this more, *please* watch [Code is the Easy Part](https://youtu.be/DSjbTC-hvqQ) first. It gets into how to collaborate in the Elm community, and I would recommend any of [these projects](README.md) over starting down this path as a first project within Elm.

