<center> <h1>CACHE-HOG CODES</h1> </center>


Author : Arnab Ghosh 

Affiliation : Carnegie Mellon University

1. Brief overview
------------------

These cache hog programs have been designed to hog cache capacity. They do so by generating accesses to memory locations that are most likely to occupy specific cache locations. Hence, they tend to evict already present data in the cache.



Please cite the following work if you use these programs for experiments in your research:

Lavanya Subramanian, Vivek Seshadri, Arnab Ghosh, Samira Khan, and Onur Mutlu, "The Application Slowdown Model: Quantifying and Controlling the Impact of Inter-Application Interference at Shared Caches and Main Memory", Proceedings of the 48th International Symposium on Microarchitecture (MICRO), Waikiki, Hawaii, USA, December 2015.


2. Description of the basic idea
----------------

These cache hogs were written for and evaluated on an Intel Core-i5 processor with a 6MB shared cache. The following figure shows a rough schematic of this system with two cores..



      Core 1 ---> L1-1 ---> L2-1
                                \
                                |---> L3 ---> RAM
                                /
      Core 2 ---> L1-2 ---> L2-2


Each core has its own private L1 and L2 cache and the cores share the L3 cache, which in turn connects to the the DRAM main memory.

Our goal is to evict other applications’ cache blocks, when run alongside them. We do so by generating access patterns that evict   existing blocks residing in the L3 cache from the application we are trying to slowdown. 




cache_miss_sequential.c
-----------------------
This program generates a streaming access pattern. 


cache_miss_random.c
-------------------

This program randomly selects a position and accessesthe array element corresponding to that location .


cache_bank_parallelism.c
------------------------

This program  hogs memory bandwidth by trying to 
stress the different banks of the DRAM main memory and hence, delay other applications’ memory requests . 


cacheoccupy_stride_stream.c
---------------------------

This program hogs cache capacity by generating a working set that is two  to four times the size of the cache. Due to the streaming access pattern, all the sets are touched once each time this program loops through. 

3. Compiling and running these programs
-------------------------------------------

Use the -O0 flag to disable compiler optimization since it tries to figure out the access patterns beforehand and may interfere with the effectiveness of the hogs.

gcc -O0 cache_miss_sequential.c -o cache_miss_sequential

gcc -O0 cache_miss_random.c -o cache_miss_random

gcc -O0 cache_bank_parallelism.c -o cache_bank_parallelism

gcc -O0 cacheoccupy_stride_stream.c -o cacheoccupy_stride_stream

Empirically just running one of them doesn't suffice, try running more than one at a time to more effectively hog cache capacity and memory bandwidth.




