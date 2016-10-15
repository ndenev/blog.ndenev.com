extends: default.liquid 
title: Rust takes the crown in this programming language benchmark.
short_title: rust_benchmark_win
date: 15 October 2016 23:52:00 +0100
---

I recently started preaching here and there about how cool, fast and light [Rust][1] is.
Then shortly after I posted this nice [comparison][2] between [Rust][1] and Java by [llogiq@][2],
my ex-colleague [famzah@][5] asked me if I can contribute a Rust implementation for a benchmark suite
 he's maintaining : [C++ vs. Python vs. PHP vs. Java vs. Others performance benchmark (2016 Q3)][4]
Agreed, both C++ and Rust implementations in these benchmarks are not the most efficient and/or idiomatic
implementations for the respective language, but the idea was to use the exact (or close as possible) algorithm
as in the original Python version so that we can get an idea of how they perform doing the same thing (in theory).
I can't say I was surprised to see that in my tests the Rust version outperformed the C++ one.
[famzah@][5]'s initial report was that also shows that the Rust version consumes much less memory
at around 36MB, while the C++ one consumed 55MB.

Given this, I don't know what would be the reason for me to pick C or C++ again :D
You can find the code to the [famzah@][5]'s benchmark *[here][6]*, and my pull request with the Rust version *[here][7]*.

[1]: http://www.rust-lang.org
[2]: https://llogiq.github.io/2016/02/28/java-rust.html
[3]: https://llogiq.github.io
[4]: https://blog.famzah.net/2016/09/10/cpp-vs-python-vs-php-vs-java-vs-others-performance-benchmark-2016-q3/
[5]: https://blog.famzah.net
[6]: https://github.com/famzah/langs-performance
[7]: https://github.com/famzah/langs-performance/pull/5/files
