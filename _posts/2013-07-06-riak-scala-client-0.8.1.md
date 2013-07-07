---
layout: post
title: RIAK SCALA CLIENT 0.8.1
icon: riak-scala-client
---

__riak-scala-client__ is a Scala client library for Riak based on <a href="http://akka.io" target="_blank">Akka</a>
and <a href="http://spray.io" target="_blank">Spray</a>. It aims to be easy to use, non-blocking, and fast, in that order.

This project was started in December 2012 out of frustration about the (then) lack of non-blocking
Scala (or Java) client libraries for <a href="http://basho.com/riak/">Riak</a>.

The first public version (0.8.0) was released in March 2013 after several early beta
releases were tested internally to get feedback from external users on a commercial project.
The latest version is {{ site.version }}.


__riak-scala-client__ has been tested on single machine Riak installations and small(ish) clusters
of up to 7 nodes. There might be some minor API changes on the way towards 1.0 but things should be
pretty stable overall. So please go ahead and try it out or even battle test it in production as long
as you are aware that the paint is still drying.


### Getting Started

__riak-scala-client__ is available from the Maven Central and Sontatype Releases repositories.
The current version is __{{ site.version }}__ and currently it is only available for Scala 2.10.x.

You can add __riak-scala-client__ to your SBT project by including the following dependency:

{% highlight scala %}
libraryDependencies += "com.scalapenos" %% "riak-scala-client" % {{ site.version }}
{% endhighlight %}

Or if you prefer Maven, you can add __riak-scala-client__ to your Maven project by including the following dependency:

{% highlight xml %}
<dependency>
<groupId>com.scalapenos</groupId>
<artifactId>riak-scala-client_2.10</artifactId>
<version>{{ site.version }}</version>
</dependency>
{% endhighlight %}

Next, head on over to the documentation section, which describes
everything else you need to get started.


### Coming Soon

Other things that are on the roadmap for 1.0:


- Using Play Iteratees for all API methods that return multiple values
- Implement streaming http response handling for all API methods that return multiple values
- Implement some micro-benchmarks to compare performance with other current Scala/Java clients

If you think anything is missing from the above lists, please go ahead and let us know by
creating an issue or a pull request!


### Feedback Welcome!

Feedback is always good so please go ahead and fork the project, file issues (or complaints),
create pull requests, star the project on GitHub, or loudly proclaim your (dis)approval on
a mailing list of your choice.


### License

This project is licensed under the Apache 2.0 open source license.
