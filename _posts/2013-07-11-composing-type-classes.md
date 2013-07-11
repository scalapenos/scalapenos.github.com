---
layout: post
title: Composing Type Classes
short-title: Composing Type Classes
tags: [scala, type class, composing, functional programming, syndicate-ok]
icon: scala
---
When you are writing a library or any kind of API in Scala, type classes
are a great tool to allow for customizable behavior while keeping coupling down
to a minimum. But what happens when one method requires several type classes
and its signature is starting to look a bit too scary?

Consider the following API method from my **[riak-scala-client][rsc]**
library:

{% highlight scala %}
def store(key: String, value: RiakValue): Future[Unit] = ...
{% endhighlight %}

The store method takes a key and an instance of `RiakValue` and stores it in Riak,
great. But this puts the burden of creating instances of `RiakValue` completely on
the end user (i.e. some poor application developer). So to to make things a
little easier to use we can provide a second version of the store method that takes
any value of type `T` as long as that value can somehow be turned into a `RiakValue`:

{% highlight scala %}
trait RiakSerializer[T] {
  def serialize(t: T): (String, ContentType)
}

def store[T: RiakSerializer](key: String, value: T): Future[Unit] = ...
{% endhighlight %}

Of course a type class without a decent default value will still force that
poor application developer to always do all the work himself, so we add a
default implementation and we make sure that the compiler will see it at
the lowest posible priority so that it won't conflict with user provided
custom implementations:

{% highlight scala %}
object RiakSerializer extends LowPriorityDefaultRiakSerializerImplicits

trait LowPriorityDefaultRiakSerializerImplicits {
  implicit def stringSerializer = new RiakSerializer[String] {
    def serialize(s: String): (String, ContentType) = (s, ContentTypes.`text/plain`)
  }
}
{% endhighlight %}

Great! Now the user can chose to accept the default behavior or he can customize
it by providing his own implementation of the type class. A nice example of
the power of type classes.

Now for the tricky part. What happens if, in order to create a proper instance
of a `RiakValue`, we need to perform a number of separate actions and we want to
user to be able to customize one or more of those actions using type classes
while using default behavior for the others?

In my **[riak-scala-client][rsc]** library, the real method signature for `store`
used to look something like this:

{% highlight scala %}
def store[T: RiakSerializer
           : RiakIndexer
           : RiakLinker
           : RiakMetaInfoGenerator](key: String, value: T): Future[Unit] = ...
{% endhighlight %}

So the value needs to be serialized but it can optionally be enriched with some
secondary indexes, links to other values, and meta attributes.

I can imagine this method signature looks pretty damn scary to most end users.
So how can I make this look a bit friendlier to the avarage end user while still
allowing power users the same flexibility of customization?

Ideally I'd like the method signature to be as simple as the first version.
Something like this:

{% highlight scala %}
def store[T: RiakMarshaller](key: String, value: T): Future[Unit] = ...
{% endhighlight %}

What I ended up with is something that I'm calling a *composed type class*, or
maybe even a *higher order type class*. I'm sure the purely functional people
will have a much better name for it but I couldn't find any obvious references
to it online.

The idea is to provide a type class that itself is constrained by the other four
type classes and which delegates its implementation to the in scope
implementations of those typeclasses. Don't worry, it sounds more complicated
than it is!

It looks like this:

{% highlight scala %}
sealed abstract class RiakMarshaller[T: RiakSerializer
                                      : RiakIndexer
                                      : RiakLinker
                                      : RiakMetaInfoGenerator] {
  def serialize(t: T): (String, ContentType)
  def index(t: T): Set[RiakIndex]
  def link(t: T): Set[RiakLink]
  def metaInfo(t: T): Set[RiakMetaInfo]
}

object RiakMarshaller {
  implicit def default[T: RiakSerializer
                        : RiakIndexer
                        : RiakLinker
                        : RiakMetaInfoGenerator] = new RiakMarshaller[T] {
    def serialize(t: T): (String, ContentType) = implicitly[RiakSerializer[T]].serialize(t)
    def index(t: T): Set[RiakIndex] = implicitly[RiakIndexer[T]].index(t)
    def link(t: T): Set[RiakLink] = implicitly[RiakLinker[T]].link(t)
    def metaInfo(t: T): Set[RiakMetaInfo] = implicitly[RiakMetaInfoGenerator[T]].metaInfo(t)
  }
}
{% endhighlight %}

The `RiakMarshaller` type class serves as a kind of alias for the other four and
just delegates the real work to them. It is implemented as a sealed class
because there is no need for extension since people can completely customize its
behavior by proving their own implementations of the individual *delegate* type
classes.

Now the method signature of `store` looks nice and simple again while we retain
the flexibility of the scary looking version.

Is this too complicated? I don't think so. Type classes in general do take a
little bit of getting used to for most developers but once you are comfortable
with them, the above seems like an obvious extra step and, dare I say it, a
simplification.

What do you think? [Let me know on Twitter](https://twitter.com/intent/tweet?screen_name=agemooij).

[rsc]: http://riak.scalapenos.com/ (Riak Scala Client Library)
