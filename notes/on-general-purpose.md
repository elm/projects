# On “General-Purpose” Languages

Many people ask me “will Elm become a general-purpose language?” or “do you think Elm is a general-purpose language?” What are they talking about?


## What is a general-purpose language?

Well, the definition [from wikipedia](https://en.wikipedia.org/wiki/General-purpose_programming_language) says:

> a general-purpose programming language is a programming language designed to be used for writing software in a wide variety of application domains

So is this term useful for any real question? “Can I use Python for a wide variety of things?” Sure. But it is very crucial to know *which* things to make any technical decision.

It would be a mistake to say, “Oh, Python can be used for a wide variety of things. Great! I will use it for this embedded system.”


## Examples of Niches

[The list on wikipedia](https://en.wikipedia.org/wiki/General-purpose_programming_language) has quite a few entries. Let’s examine a few.

| Language | Strong For        | Weak For         |
|----------|-------------------|------------------|
| C        | operating systems | web programming  |
| Java     | servers           | emdedded systems |
| Python   | scripting         | iPhone apps      |

Why is it that these “general-purpose” languages have such clear niches?

Language design choices like “how does memory management work?” inherently select certain target domains. For embedded systems, you want manual memory management! For a quick script, you do not!

Ecosystems also form around certain domains. Python may seem like an odd choice for scientific computing from a performance perspective, but it has a great ecosystem for that domain. Without the ecosystem, it would be silly to do this, and you can observe that this is true in Ruby and Perl and PHP.


## Relevance to Elm

When a young language says “X is a general-purpose language” I hear “I am not sure what X is for but I am making it anyway”. I think languages like Elm, Elixir, and Julia have had success because they focused on a particular domain and attempted to understand the *people* who work in those domains.

So why spend a bunch of time early in the life of a language to transition from “great in one domain” to “weak in three domains”? Many languages have tried this, and you probably have not heard of them because it is not a good opening strategy. Historically, languages branch out after establishing their niche. Clojure began for servers and branched out to web apps. Python began for scripting and branched out to scientific computing, education, etc.

I prefer to have a coherent strategy about expanding to other domains. Why expand to another domain? What strengths could Elm bring there? How much work would it be? Should work on web apps slow down for this? What is best for the long-term health of the project? Etc. I also think it is important to clearly inform people what the language is decent at *right now*. That way they do not waste their time on paths we already know that Python or Erlang or C can do better *right now*. I ask that you respect people’s time as well if you do experiments with Elm to explore other domains.
