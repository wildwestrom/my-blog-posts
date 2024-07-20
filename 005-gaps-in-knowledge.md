---
TITLE: Gaps in my knowledge and how I might fill them in.
SUBTITLE: I've been learning software development for over 6 months now and I've been made aware of some areas where my knowledge could be better.
DATE: 2021-09-14
TAGS: programming
ID: gaps-in-knowledge
---

It seems every time I begin to cross the boundary from hack to
professional, I begin to realize just how little I know about what I\'ve
been learning (See: [Four stages of
Competence](https://en.wikipedia.org/wiki/Four_stages_of_competence)).

In this blog, I\'ll list all the areas I\'m still not familiar with in
the software world and what little I do know about them. Keep in mind,
this is coming from someone who\'s first main language is Clojure.

# How I usually learn

Maybe it\'s a good idea to take a second and reflect on how I learn
things. I usually like to look at best-practices from multiple sources
before I even start. The reason I do this is to avoid forming bad habits
from the beginning. Now I don\'t care if I become opinionated, as long
my opinions are based on reality and correct.

It\'s the same reason I\'ll learn proper pronunciation before I start to
practice speaking a language. I don\'t want my American English
phonological patterns to bleed into a language I\'m learning, especially
if I want to sound like a native.

# Things I definitely need to learn

## Test Driven Development

You may know that I\'m a big fan of Robert C. Martin a.k.a \"Uncle
Bob\". As an aside, I was first inspired to look into Clojure after his
talk _[The Last Programming Language](https://youtu.be/P2yr-3F6PQo)_. He
is an advocate for clean code and standards of professionalism for
software engineers. I would like to avoid a scenario where the
governments of the world start dictating what we can and cannot do as
programmers.

One such standard of ethics is what Uncle Bob cites as the Scribe\'s
Oath. Here is the third clause in this oath.

> I will produce, with each release, a quick, sure, and repeatable proof
> that every element of the code works as it should.

This is used to justify the absolute necessity test-driven development.
Uncle Bob would rather a world where no code is ever checked in without
tests.

Personally I believe in his vision, however I\'ve also had a lot of
difficulty implementing it. It\'s especially difficult to follow the
_\"red green refactor\"_ method to the letter. Despite all the benefits
I\'ve experienced when I\'ve had tests that prove the functionality of
my code with certainty, I still don\'t start with tests nearly as much
as I should.

- How does one write tests for a UI?
- How does one test HTTP responses?
- How to do generative testing, property-based testing?

## Error handling

I do almost zero error handling in my projects. No try/catch blocks, no
exceptions. It\'s as if I expect my software will never fail, and if I
know it might fail, I don\'t have any idea what the best way to fail is.
Could this be a manifestation of my own fear of failure? Maybe, but I
think that\'s a little out of scope.

I hope that soon I\'ll find the blog-post or the conference talk that
shows the way to properly handle errors. I still haven\'t entirely
wrapped my head around how to apply what the Joe Armstrong, creator of
Erlang, was saying in [this talk](https://youtu.be/TTM_b7EJg5E).

## Databases

I only took a cursory glance at Datomic and tried using it on my own
project. If I\'m honest, seemed really cumbersome, and this was supposed
to be a nicer database to work with, especially with Clojure.

So many moving pieces to keep track of, and I didn\'t even have to spin
up a Docker container to try it. It\'s probably a good idea to make sure
our data storage and querying is as robust and stable as possible, so
perhaps these moving pieces are inevitable consequences of the need to
work with data in a secure and scalable way.

Now I could try SQL. I recently learned just how ubiquitous SQL
databases are. I must admit, I was a little scared by the fact that
you\'re passing around strings to interact with the database and this
opens the door for nasty things like SQL injection attacks.

Either way, it seems like SQL is to databases like Git is to version
control. It is for the most part, the way to go.

## Logging

So far I\'ve found this to be totally unnecessary. I\'ve never needed to
log anything in my little projects. Of course, in a more complex system,
logging can be useful and even necessary. I\'d really like to gain a
better understanding of when we need to log and how.

## Server building

I\'ve done enough web-scraping to know what a GET and POST request are.
I\'ve used them to log in and out of sites programmatically, i.e.
without something like a headless browser.

It seems like Clojure is particularly well suited to the task of
building up servers by handling requests. I\'ve dipped my toe into
[Ring](https://github.com/ring-clojure/ring) with
[http-kit](https://http-kit.github.io/). I still have yet to really
understand what a well-built server looks like.

I also found out that there\'s a huge depth to HTTP and how the browser
handles it. It seems like I could really shoot myself in the foot if I
don\'t properly understand it.

## Security

This is a massive blind spot for me. Yes I\'m one of the few who can say
they\'ve successfully made a PGP key and keep it in a hardware token. I
also know how I can log in to ssh servers without remembering passwords.

Otherwise, I have no delved into this world at all. The idea of making
user authentication and not knowing what you\'re doing is dangerous and
irresponsible, at least in my opinion. Of course, this fear is also what
has kept me from even trying to implement user authentication myself.

Would learning penetration testing as well help increase my confidence?
Perhaps. Do a lot of security issues originate from people being stupid
with their data (e.g Sending passwords over email, using the same easily
guessable password on every site)? Yes, but I also need to do my part to
keep data as safe as possible.

Don\'t even get me started on anonymity, privacy, and security
techniques needed if your adversary is a 3-letter agency. If that\'s who
you\'re up against you\'re probably already fucked.

## Concurrency

Isn\'t it amazing that you can make `map` execute in parallel by adding
a \'p\'?. It only gets harder from here on.

This is yet another whole world unto itself. Not only that, but as soon
as concurrency is added to the mix, the complexity of all the topics I
mentioned before, and anything else I missed, increases exponentially.
Thankfully I\'m not flying so close to the metal that I need to manage
memory, but it\'s still a jungle, especially when working with
ClojureScript.

# Non essential things I want to learn more of

There are certain things that so far, I see as not entirely essential,
but are

## State

State is and will always be inevitable in any useful piece of software.
Even today there is still much debate around how to manage state, with
new libraries and techniques popping up for state management all the
time.

I\'m grateful that people realized that functional programming (i.e.
discipline placed upon assignment) can largely solve the problem of
managing state. It\'s also comforting to know that many intelligent
people thinking about this problem carefully, creating solutions are out
there to help wrangle any state into manageable pieces.

I think this is one of the few examples where I don\'t necessarily need
to learn a new tool, library, or paradigm, but rather just need to
practice keeping my code clean in general.

## Object oriented design patterns

There\'s a whole world of beliefs and debate about object oriented
programming and how to do it. Since I\'m using Clojure, I can largely
avoid this mess. I however believe, as do some others, that there are
some babes within [the bathwater that is
OOP](https://youtu.be/QM1iUe6IofM).

User p-himlik on the Clojurians Slack pointed me to [this
repository](https://github.com/plumatic/eng-practices/blob/master/clojure/20130926-data-representation.md)
written by [Jason Wolfe](https://github.com/w01fe). It gives an overview
of the various object-like macros within Clojure.

> In Clojure, there are a potentially daunting number of ways to
> represent a slice of data that would have been an Object in an
> OO-land. ... \[T\]he whole reason we care about data representation is
> because we want to make it easy to do the operations we want on our
> data -- thus, it makes no sense to think about data in the absence of
> functions. A complicating factor is that we sometimes want these
> functions to be polymorphic -- that is, work (differently) across a
> variety of different data types. -- Jason Wolfe

If I\'m being honest, I really have a lot of trouble wrapping my head
around OOP concepts. I have yet to understand how to use protocols,
records, or multi-methods in a useful way.

### Polymorphism

Polymorphism might still a useful concept even in functional
programming. I\'m still unconvinced that I really need protocols,
records, or multi-methods. I believe Rich Hickey is the one who first
described Clojure in particular as having \"À la Carte Polymorphism\".
This is one concept that I have not really wrapped my head around. Even
the [Wikipedia
article](https://en.wikipedia.org/wiki/Polymorphism_%28computer_science%29)
mentions 3 classes of polymorphism. In any case, Lambda Island has a
video on this titled _\"[À la Carte
Polymorphism](https://lambdaisland.com/episodes/a-la-carte-polymorphism-1)\"_
that I\'ll probably need to keep re-watching.

### Encapsulation

Stuart Sierra made a great video talking about this in his talk
_\"[Components: Just Enough Structure](https://youtu.be/13cmHf_kt-Q)\"_.
He argues that there are certain circumstances where certain features of
objects, can be useful in avoiding scattering global state all over a
large application. By encapsulating state locally within a component,
which is very much like an object, it\'s much easier to reason about
static configuration and state.

I may not recommend using
[component](https://github.com/stuartsierra/component), as even Mr.
Sierra himself admits that this requires whole-project buy-in from the
start. There are some problems with this approach, and so a library with
a similar idea called [mount](https://github.com/tolitius/mount) came
about to deal with some of [component\'s
downsides](https://github.com/tolitius/mount/blob/master/doc/differences-from-component.md).

## Specs and Types

It seems like type checking is largely unnecessary in Clojure. The idea
of being able to check for certain classes of bugs and errors at compile
time is often cited as a reason people advocate for strong typing.
Projects like [Typed
Clojure](https://github.com/typedclojure/typedclojure) and [Clojure
Spec](https://github.com/clojure/spec-alpha2) implement this
functionality optionally.

I haven\'t found much consensus on how useful things like
[spec](https://github.com/clojure/spec-alpha2),
[schema](https://github.com/plumatic/schema),
[core.typed](https://github.com/clojure/core.typed) or
[malli](https://github.com/metosin/malli) are in production. To me it
seems like more layers indirection, with the guarantees of safety and
bug-prevention still ultimately offloaded to the brain of the
programmer. Eventually I will form a more complete opinion on this.

# Conclusion

This whole blog turned out to be much longer than anticipated. I think
I\'ll keep most of this in my personal notes so I can add more
information as my understanding gets better.

So much to learn, so little time.
