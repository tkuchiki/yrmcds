yrmcds
======

yrmcds is a memory object caching system with master/slave replication
and [server-side locking](docs/locking.md).

Currently, yrmcds supports two protocols: the first is an enhanced
[memcached][], and another is a protocol to implement
[distributed resource counters](docs/counter.md).

Since the memcached protocol is perfectly compatible with the
[original implementation][memcached], yrmcds can be used as a drop-in
replacement for memcached.  Thanks to its virtual-IP based replication
system, existing applications can obtain high-available
memcached-compatible service without any modifications.

A companion client library [libyrmcds][] and a [PHP extension][php-yrmcds]
are also available.

yrmcds was developed originally for [kintone.com][kintone].

License
-------

yrmcds is licensed under [the BSD 2-clause license][bsd2].

The source code contains a [SipHash][] implementation borrowed from
[csiphash][] which is licensed under [the MIT license][mit].

Features
--------

* Complete memcached text and binary protocols.
* [Server-side locking](docs/locking.md) extension to memcached protocol.
* [Distributed resource counter](docs/counter.md) protocol.
* [Optional memory security](docs/usage.md#secure_erase) to store
  confidential information like SSL session data.
* Large objects can be stored in temporary files, not in memory.
* Virtual-IP based master-slave replication.
    * Automatic fail-over
    * Automatic recovery of redundancy.
* Global LRU eviction / no slab distribution problem.
    * Unlike memcached, yrmcds is not involved with slabs problems.
      ([1][slab1], [2][slab2])

A [companion client library][libyrmcds] and a [PHP extension][php-yrmcds]
are also available.

See also [usage guide](docs/usage.md), [future plans](docs/future.md),
[differences from memcached](docs/diffs.md), [design notes](docs/design.md)
and some [benchmark results](docs/bench.md).

Prerequisites
-------------

* C++11 compiler (gcc 4.8.1+ or clang 3.3+).
* [TCMalloc][tcmalloc] from Google.
* GNU make.

Build
-----

1. Prepare TCMalloc.  
    On Ubuntu, run `apt-get install libgoogle-perftools-dev`.
2. Run `make`.

You can build yrmcds without TCMalloc by editing Makefile.

Install
-------

On Ubuntu, `sudo make install` installs yrmcds under `/usr/local`.  
An upstart script and a logrotate configuration file are installed too.

About the name
--------------

The name yrmcds was taken from "Ymmt's Replicating MemCacheD for Sessions".  
As it reads, yrmcds was developed mainly for session storage.

The correct pronunciation sounds like: "Yo-Ru-Mac-Do" (夜マクド in Japanese).


[memcached]: http://memcached.org/
[bsd2]: http://opensource.org/licenses/BSD-2-Clause
[SipHash]: https://131002.net/siphash/
[csiphash]: https://github.com/majek/csiphash
[mit]: http://opensource.org/licenses/MIT
[libyrmcds]: http://cybozu.github.io/libyrmcds/
[php-yrmcds]: http://cybozu.github.io/php-yrmcds/
[slab1]: http://nosql.mypopescu.com/post/13506116892/memcached-internals-memory-allocation-eviction
[slab2]: https://groups.google.com/forum/#!topic/memcached/DuJNy5gbQ0o
[kintone]: https://www.kintone.com/
[tcmalloc]: http://goog-perftools.sourceforge.net/doc/tcmalloc.html
