---
layout: post
title: Riak Scala Client 0.8.1 Released
short-title: Riak Scala Client 0.8.1
tags: [scala, riak scala client]
icon: riak-scala-client
---
I have just released a minor update of __[riak-scala-client](http://riak.scalapenos.com)__
to bring it up to date with Spray, now that it has finally released the long-awaited
M8 milestone. It also contains some minor improvements and fixes.

This release comes in two flavors to satisfy both the stable crowd and the early
adopters.  Version __0.8.1.1__ is compatible with Akka 2.1.x (compiled against
2.1.4) and Spray 1.1-M8  while version __0.8.1.2__ is compatible with Akka 2.2-RC1
and Spray 1.2-M8. A release compatible with the final release of Akka 2.2 will
follow as soon as Spray releases a compatible version for it.

Changes include:

- Some internal code changes to bring the code up to date with the Spray M8 API
- Better handling of Riak tombstone values during conflict resolution (tombstoned
  values will now be ignored)
- The default value of the add-client-id-header configuration property has been
  changed to `no`. This better reflects common practise since virtually no one
  is running pre-1.0 versions of Riak anymore and all post-1.0 versions support
  the newer vnode ids. Of course you can still set it to `yes` in your
  application.conf if you want.
- The client will no longer erroneously add the `ETag` and `Last-Modified` headers
  in requests to Riak. These are response headers and have no place in a request.
- General code cleanup

A bigger release with more new features will follow soon. Work was on hold for a few
months while other things had a higher priority but priority has now been
restored and I'll be adding more of the missing Riak features, solving your
issues, and merging in your pull requests as soon as possible! My apologies for
the temporary slow down.

Happy Riakking!
