---
layout: site
header: Projects
projects-active: active
---

### Standard Library

Like much of the Ruby standard library, these libraries are sparsely documented. Your help would be appreciated!

* [`thread`](http://rdoc.info/stdlib/thread), providing Ruby threading primitives such as Queue
  * [Queue](http://rdoc.info/stdlib/thread/Queue), a blocking queue implementation for interthread communication
* [`thwait`](http://rdoc.info/stdlib/thwait), to create thread groups and wait for one or more to complete
* [`monitor`](http://rdoc.info/stdlib/monitor), an alternative approach to locking
* [`gserver`](http://rdoc.info/stdlib/gserver), a generic server implementation which serves requests in threads

### Threading Support

* [`ruby-thread`](https://github.com/meh/ruby-thread), various extensions to the Ruby threading libraries, including pools, futures, promises, channels, and pipelines

### Actors

Actors attempt to abstract away difficulties of parallel programming by concealing state and only passing messages.

* [Celluloid](https://github.com/celluloid/celluloid), an actor framework using threads and fibers
  * [DCell](https://github.com/celluloid/dcell), a distributed object framework on top of Celluloid
  * [Reel](https://github.com/celluloid/reel), a Celluloid-based web server

### Event-Driven I/O 

* [EventMachine](https://github.com/eventmachine/eventmachine), a low-level event-driven I/O subsystem
* [em-synchrony](https://github.com/igrigorik/em-synchrony), a set of tools to do event-based I/O without callbacks
* [goliath](https://github.com/postrank-labs/goliath), a lightweight event-based web framework
* [cramp](http://cramp.in/), another lightweight event-based web framework

### Thread-Safe Data

* [Hamster](http://github.com/harukizaemon/hamster), "efficient, immutable, thread-safe collection classes for Ruby"
* [ruby-atomic](https://github.com/headius/ruby-atomic), atomic references for JRuby and Rubinius
