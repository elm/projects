# On Sharing Types

Imagine that we could use the same types in the front-end and backend-code. The crucial question becomes **How do we keep these types synchronized between server and clients?** Whenever any serialized type changes, we would need to force all users to refresh to the latest code.


## Challenges

Forced refreshes raise some difficult technical questions:

  - What happens while the servers are transitioning? If one request is routed to a new server, but another to an old one, what happens? Does the user thrash back and forth? How can that be avoided? Maybe versions only can go “up” so there is a history of all releases? And if the request goes to an old endpoint it just fails?

  - What happens when someone is writing something when this transition occurs? Does the refresh just happen and the work is lost? Do we save it somehow? The types do not match in the new code, so what format do we use?

Perhaps it is possible to resolve these challenges. That could be really cool. My point is only that “having types on both sides” alone does not magically resolves the challenges of coordinating client and server code.

You may be thinking that these problems seem hard, but how often does it really matter?!


## Frequency

Say you are releasing new code once a day. With a naive implementation, that means everyone gets forced refreshes every day as well.

Let’s try to be more clever though. Say we have a hash that represents all types that may be serialized between client and server. **But how do I know which types those are?** Every server endpoint would need to be statically known by the compiler. `/user/profile` returns a `Profile`, `/comment/INT` returns a `Comment`, etc. What design permits that?

Let’s try to be less clever. Say we have a hash that represents all types used in the whole program. That could reduce the number of forced refreshes, but with extreme caveats. Adding a new `Msg` or new field in your `Model` would force a refresh.

Again, the point here is that “just run Elm on node.js” or “just run Elm on BEAM VM” does not address the important and difficult questions that could make “sharing types” valuable in practice.
