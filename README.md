fastcache
=========

C implementation of Python 3 lru_cache for Python 2.6, 2.7, 3.2, 3.3, 3.4 

Install
-------

Via pip: 

    pip install fastcache

Manually : 

    git clone https://github.com/pbrady/fastcache.git
    cd fastcache
    python setup.py install

Test
----

`py.test --pyargs fastcache`

Travis CI status :  [![alt text][2]][1]

[2]: https://travis-ci.org/pbrady/fastcache.svg?branch=master (Travis build status)
[1]: http://travis-ci.org/pbrady/fastcache

Tests include the official suite of tests from Python standard library for functools.lru_cache

Use
---

    >>> from fastcache import clru_cache
    >>> @clru_cache(maxsize=256, typed=True)
    >>> def f(a, b):
    ...     """Test function."""
    ...     return (a, ) + (b, )
    >>> f(1,2)
    (1, 2)
    >>> f.cache_info()
    CacheInfo(hits=0, misses=1, maxsize=256, currsize=1)
    >>> f(1,2)
    (1, 2)
    >>> f.cache_info()
    CacheInfo(hits=1, misses=1, maxsize=256, currsize=1)
    >>> f.cache_clear()
    >>> f.cache_info()
    CacheInfo(hits=0, misses=0, maxsize=256, currsize=0)

Speed
-----

The speed up vs `lru_cache` provided by `functools` in 3.3 or 3.4 is 10x-30x depending on the function signature and whether one is comparing with 3.3 or 3.4.  A sample run of the benchmarking suite for 3.3 is 

	>>> import sys
	>>> sys.version_info
	sys.version_info(major=3, minor=3, micro=5, releaselevel='final', serial=0)
	>>> from fastcache import benchmark
	>>> benchmark.run()
	Test Suite 1 : 

	Primarily tests cost of function call, hashing and cache hits.
	Benchmark script based on
		http://bugs.python.org/file28400/lru_cache_bench.py

	function call                 speed up
	untyped(i)                       11.31, typed(i)                         31.20
	untyped("spam", i)               16.71, typed("spam", i)                 27.50
	untyped("spam", "spam", i)       14.24, typed("spam", "spam", i)         22.62
	untyped(a=i)                     13.25, typed(a=i)                       23.92
	untyped(a="spam", b=i)           10.51, typed(a="spam", b=i)             18.58
	untyped(a="spam", b="spam", c=i)  9.34, typed(a="spam", b="spam", c=i)   16.40

				 min   mean    max
	untyped    9.337 12.559 16.706
	typed     16.398 23.368 31.197


	Test Suite 2 :

	Tests millions of misses and millions of hits to quantify
	cache behavior when cache is full.

	function call                 speed up
	untyped(i, j, a="spammy")         8.94, typed(i, j, a="spammy")          14.09

A sample run of the benchmarking suite for 3.4 is

	>>> import sys
	>>> sys.version_info 
	sys.version_info(major=3, minor=4, micro=1, releaselevel='final', serial=0)
	>>> from fastcache import benchmark
	>>> benchmark.run()
	Test Suite 1 : 

	Primarily tests cost of function call, hashing and cache hits.
	Benchmark script based on
		http://bugs.python.org/file28400/lru_cache_bench.py

	function call                 speed up
	untyped(i)                        9.74, typed(i)                         23.31
	untyped("spam", i)               15.21, typed("spam", i)                 20.82
	untyped("spam", "spam", i)       13.35, typed("spam", "spam", i)         17.43
	untyped(a=i)                     12.27, typed(a=i)                       19.04
	untyped(a="spam", b=i)            9.81, typed(a="spam", b=i)             14.25
	untyped(a="spam", b="spam", c=i)  7.77, typed(a="spam", b="spam", c=i)   11.61

				 min   mean    max
	untyped    7.770 11.359 15.210
	typed     11.608 17.743 23.311


	Test Suite 2 :

	Tests millions of misses and millions of hits to quantify
	cache behavior when cache is full.

	function call                 speed up
	untyped(i, j, a="spammy")         8.27, typed(i, j, a="spammy")          11.18




