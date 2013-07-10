---
layout: post
title: Announcing Riak Scala Client
short-title: Riak Scala Client
icon: riak-scala-client
---
After three months of hacking, I'm proud to announce a new Riak Scala client library based
on Akka and Spray and simply called __riak-scala-client__. It aims to be easy to use,
non-blocking, and fast, in that order.

[http://riak.scalapenos.com/](http://riak.scalapenos.com/)

This is the first public release but I've been testing it on a large project since January
and it was time to let other people kick the tires and tell me what I did wrong (or right).

The client is purely based on the Riak http API and currently it supports the following Riak
features:

- Fetch
- Store
- Delete
- Secondary Indexes (2i)
- Getting/setting bucket properties
- ping

Other features include:

- Completely non-blocking thanks to Scala 2.10 Futures, Akka, and Spray
- Transparent integration with Akka projects through an Akka extension
- An untyped `RiakValue` class for interacting with raw Riak values and their associated
  meta data (vclock, etag, content type, last modified time, indexes, etc.)
- A typed `RiakMeta\[T\]` class for interacting with deserialized values while retaining
  their associated meta data (vclock, etag, content type, last modified time, indexes, etc.)
- Customizable conflict resolution on all fetches (and stores when `returnbody=true`)
- Automatic (de)serialization of Scala (case) classes using type classes
- Builtin default spray-json (de)serializers
- Customizable indexing of Scala (case) classes using type classes
- Auto-retry of fetches and stores (a standard feature of the underlying spray-client library)

Missing features such as link walking and Map-Reduce will be implemented
as soon as possible.

Check it out at: [http://riak.scalapenos.com/](http://riak.scalapenos.com/)
