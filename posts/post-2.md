extends: default.liquid

title: Symmetrical RSS hashing using Toeplitz hash.
date: 18 September 2016
---

Modern OSes and NICs use hashing based on src/dst port and protocol tuples to identify
"flows" so that they can be put on different CPU or NIC queue.
Doing so improves cache locality and this performance and is what Microsoft (and others),
are calling [RSS][1] a.k.a receive side scaling/steering.

While playing with some Rust [code][3] to parse [Suricata][5] EVE Json log events I was thinking about
calculating and keeping per flow list of events (**looks like Suricata flow id fields are not very unique, however this is [changing][4]).
Then I found this nice [article][2] explaining the RSS Toeplitz hash function and a modification from the default
that is being used now to allow for symmetric hashing: [Symmetric RSS][2]
Symmetrical hashing means that ideally both directions of the traffic of certain flow would end up on the same NIC queue and thus the same CPU, so we can get better performance due to the better cache locality and potentially less locking required.

Here is also some DragonFlyBSD and FreeBSD code implementing Toeplitz:
 * [http://bxr.su/DragonFly/sys/net/toeplitz.c][6]
 * [https://github.com/freebsd/freebsd/blob/master/sys/net/toeplitz.c][7]

P.S.: I wonder why I see DragonFly code implement some sort of caching, while
FreeBSD code does not.

[1]: https://msdn.microsoft.com/windows/hardware/drivers/network/rss-hashing-functions
[2]: http://www.ran-lifshitz.com/2014/08/28/symmetric-rss-receive-side-scaling/
[3]: https://github.com/ndenev/eve-rust
[4]: https://redmine.openinfosecfoundation.org/issues/1870
[5]: https://suricata-ids.org
[6]: http://bxr.su/DragonFly/sys/net/toeplitz.c
[7]: https://github.com/freebsd/freebsd/blob/master/sys/net/toeplitz.c
