title: Rust takes the crown in this programming language benchmark.
published_date: "2016-10-15 23:52:00 +0100"
layout: default.liquid
data:
  short_title: rust_benchmark_win
---
*Updated on 16 Oct 2016 (added benchmark output)*

I recently started preaching here and there about how cool, fast and light [Rust][1] is.  
Then shortly after I posted this nice [comparison][2] between [Rust][1] and Java by [llogiq@][2],  
my ex-colleague [famzah@][5] asked me if I can contribute a Rust implementation for a benchmark suite  
he's maintaining : [C++ vs. Python vs. PHP vs. Java vs. Others performance benchmark (2016 Q3)][4].  
Some might argue that both the C++ and Rust implementations in these benchmarks are not the most
efficient or idiomatic implementations for the respective language, but the idea was to implement  
 the exact (or close as possible) algorithm as in the original Python version so that we can get a rough  
idea of how they perform doing the same thing (in theory).
I can't say I was surprised to see that in my tests the Rust version outperformed the C++ one.  
Also [famzah@][5]'s initial report was that also shows that the Rust version consumes much less memory  
at around 36MB, while the C++ one consumed 55MB.  

Here is the raw benchmark output only of the C++ (-O2 optimized) and Rust versions:  
```
# Run time limited to 90 wall-clock seconds
#
# C++ (optimized with -O2)
# ... compilation
# ... run 1
real_TIME:89.26sec user_CPU:73.96sec sys_CPU:7.00sec max_RSS:50596kb swaps:0 ctx_sw:3768+2 nlines:805 run_try:1  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# ... run 2
real_TIME:89.99sec user_CPU:74.49sec sys_CPU:7.08sec max_RSS:50400kb swaps:0 ctx_sw:3396+1 nlines:813 run_try:2  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# ... run 3
real_TIME:89.99sec user_CPU:74.50sec sys_CPU:7.03sec max_RSS:50168kb swaps:0 ctx_sw:2938+1 nlines:807 run_try:3  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# ... run 4
real_TIME:90.00sec user_CPU:74.42sec sys_CPU:7.18sec max_RSS:51984kb swaps:0 ctx_sw:2673+1 nlines:806 run_try:4  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# ... run 5
real_TIME:89.95sec user_CPU:75.32sec sys_CPU:6.10sec max_RSS:52112kb swaps:0 ctx_sw:2053+1 nlines:808 run_try:5  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# ... run 6
real_TIME:90.06sec user_CPU:75.20sec sys_CPU:6.62sec max_RSS:51240kb swaps:0 ctx_sw:3206+1 nlines:806 run_try:6  header:'C++ (optimized with -O2)' version:'g++
 (GCC) 4.8.5 20150623 (Red Hat 4.8.5-4) ' src_file:primes.cpp
# Rust
# ... compilation
# ... run 1
real_TIME:90.07sec user_CPU:76.08sec sys_CPU:5.54sec max_RSS:38376kb swaps:0 ctx_sw:4479+1 nlines:867 run_try:1  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
# ... run 2
real_TIME:90.00sec user_CPU:76.80sec sys_CPU:4.45sec max_RSS:38380kb swaps:0 ctx_sw:4232+1 nlines:871 run_try:2  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
# ... run 3
real_TIME:90.09sec user_CPU:77.09sec sys_CPU:4.52sec max_RSS:38380kb swaps:0 ctx_sw:4181+1 nlines:902 run_try:3  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
# ... run 4
real_TIME:90.05sec user_CPU:77.17sec sys_CPU:4.33sec max_RSS:38384kb swaps:0 ctx_sw:4192+1 nlines:895 run_try:4  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
# ... run 5
real_TIME:90.12sec user_CPU:77.15sec sys_CPU:4.52sec max_RSS:39312kb swaps:0 ctx_sw:3902+1 nlines:904 run_try:5  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
# ... run 6
real_TIME:90.06sec user_CPU:77.28sec sys_CPU:4.35sec max_RSS:39932kb swaps:0 ctx_sw:3477+1 nlines:900 run_try:6  header:'Rust' version:'rustc 1.12.0 (3191fbae9
 2016-09-23) ' src_file:primes.rs
```

Some analysis:  
```
 ndenev> ~ > langs-performance(branch:master) > awk '/primes.rs/ { print $7 }' test.txt | cut -d : -f 2 > rust.txt
 ndenev> ~ > langs-performance(branch:master) > awk '/primes.cpp/ { print $7 }' test.txt | cut -d : -f 2 > cpp.txt
 ndenev> ~ > langs-performance(branch:master) > ministat -s -w 60 cpp.txt rust.txt
x cpp.txt
+ rust.txt
+------------------------------------------------------------+
|xxx  x                              +  +            +  + ++ |
||MA|                                                        |
|                                        |________A_____M___||
+------------------------------------------------------------+
    N           Min           Max        Median           Avg        Stddev
x   6           805           813           807         807.5     2.8809721
+   6           867           904           900     889.83333     16.461065
Difference at 95.0% confidence
        82.3333 +/- 15.2002
        10.1961% +/- 1.88238%
        (Student's t, pooled s = 11.8167)
 ndenev> ~ > langs-performance(branch:master) >
```


Yup, Rust is 10% faster than C++ and uses less memory!  
With these results I don't know what would be the reason for me to pick C or C++ again :D  
You can find the code to the [famzah@][5]'s benchmark *[here][6]*, and my pull request with the Rust version *[here][7]*.

[1]: http://www.rust-lang.org
[2]: https://llogiq.github.io/2016/02/28/java-rust.html
[3]: https://llogiq.github.io
[4]: https://blog.famzah.net/2016/09/10/cpp-vs-python-vs-php-vs-java-vs-others-performance-benchmark-2016-q3/
[5]: https://blog.famzah.net
[6]: https://github.com/famzah/langs-performance
[7]: https://github.com/famzah/langs-performance/pull/5/files

