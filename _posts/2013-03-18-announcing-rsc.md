---
layout: post
title: ANNOUNCING RIAK SCALA CLIENT
icon: riak-scala-client
---
After three months of hacking, I'm proud to announce a new Riak Scala client library based
on Akka and Spray and simply called riak-scala-client. It aims to be easy to use,
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
    - Fetching exact matches
    - Fetching ranges
    - Storing with indexes
- Getting/setting bucket properties
- ping

Other features include:

- Completely non-blocking thanks to Scala 2.10 Futures, Akka, and Spray
- Transparent integration with Akka projects through an Akka extension
- An untyped RiakValue class for interacting with raw Riak values and their associated
  meta data (vclock, etag, content type, last modified time, indexes, etc.)
- A typed RiakMeta\[T\] class for interacting with deserialized values while retaining
  their associated meta data (vclock, etag, content type, last modified time, indexes, etc.)
- Customizable conflict resolution on all fetches (and stores when returnbody=true)
- Automatic (de)serialization of Scala (case) classes using type classes
    - builtin spray-json (de)serializers
- Automatic indexing of Scala (case) classes using type classes
- Auto-retry of fetches and stores (a standard feature of the underlying spray-client library)

The following Riak (http) API features are still missing and will follow soon:

- Link walking
- Map Reduce
- Listing all keys in a bucket
- Listing all buckets
- Conditional fetch/store semantics (i.e. If-None-Match and If-Match for ETags and
  If-Modified-Since and If-Unmodified-Since for LastModified)
- Node Status

Check it out at: http://riak.scalapenos.com/
