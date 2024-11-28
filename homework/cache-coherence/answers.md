Question 1

For algorithm 1, does increasing the number of threads improve performance or hurt performance? Use data to back up your answer.

Answer 1

### Naive  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.287470           |  
| 2          | 0.354853           |  
| 4          | 0.325001           |  
| 8          | 0.320909           |

Algorithm 1 had its best local performance on 1 thread and its worst performance on 2 threads. Adding threads hurt its performance.

Question 2

(a) For algorithm 6, does increasing the number of threads improve performance or hurt performance? Use data to back up your answer.

(b) What is the speedup when you use 2, 4, 8, and 16 threads (only answer with up to the number of cores on your system).

Answer 2

### All  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.262452           |  
| 2          | 0.165475           |  
| 4          | 0.156006           |  
| 8          | 0.269185           |
 
(a) Algorithm 6 had its best local performance on 4 threads and its worst performance on 8 threads. Adding threads improved performance up to a point, upon which adding threads began to hurt it.

(b) Taking 1 core as base, the speedup is: 1.59, 1.68 and 0.97 for 2, 4 and 8 cores respectively

Question 3

Refer to /data/local

(a) Using the data for all 6 algorithms, what is the most important optimization, chunking the array, using different result addresses, or putting padding between the result addresses?

(b) Speculate how the hardware implementation is causing this result. What is it about the hardware that causes this optimization to be most important?

Answer 3

(a) Since the best performance increase was seen in algorithms 5 and 6 we can conclude the most important optimization is putting padding between the result addresses.

(b) Poor use of the cache appears to be the bottleneck for most of the algorithms

Question 4

(a) What is the speedup of algorithm 1 and speedup of algorithm 6 on 16 cores as estimated by gem5?

(b) How does this compare to what you saw on the real system?

Answer 4

# Naive
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.838           |  
| 2          | 0.779           |  
| 4          | 0.818           |  
| 8          | 0.881           |  
| 16         | 0.917           | 

# All
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.839           |  
| 2          | 0.438           |  
| 4          | 0.236           |  
| 8          | 0.142           |  
| 16         | 0.111           |
 
(a) Speedup of Algorithm 1 (Naive) on 16 cores: 0.914. Speedup of Algorithm 6 (All) on 16 cores: 7.56

(b) The speedup from gem5 is 0.914, while the real system shows a speedup of 0.897. This suggests that gem5's estimated performance is quite close to the real system's performance. For Algorithm 6, the speedup in gem5 is much higher (7.56), while the real system shows a speedup of only 0.975.

Question 5

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the hit ratio?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Answer 5

Refer to data/gem5-mesi

| Algorithm    | L1 Hit Ratio % | L2 Hit Ratio % | 
|--------------|--------------------|------------|
| 1- Naive     | 0.839           |  |
| 2- Chunk     | 0.438           |  |
| 3- Race      | 0.236           |  |
| 4- Chunk Race| 0.142           |  |
| 5- Block     | 50           | 50             |
| 6- All       | 99.32           | 69.5 |

Algorithm	L1 Hit Ratio	L2 Hit Ratio
All	99.32%	69.5%
Block	50%	50%
Chunk	93.6%	69.4%
Chunk-Race	94.1%	56.5%
Naive	87.8%	90.3%
Race	88.3%	62.0%

The chunking strategy does not seem to have a significant impact, since algorithm 2 has a similar performance to algorithm 1 and algorithm 4 which combines algorithm 3 wiht chunking, performs similar to algorithm 3. The padding optimization (algorithm 5) has the biggest impact on the hit ratio, as evidenced by the significant drop in execution time at higher core counts (0.145ms at 16 cores). The 6th algorithm (a combination of all optimizations) shows just a slightly better performance compared to just padding (best performance 0.111ms).

Question 6

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the read sharing?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Answer 6


Question 7

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the write sharing?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Answer 7


Question 8

Let's get back to the question we're trying to answer. From question 3 above, "What is it about the hardware that causes this optimization to be most important?"

So: (a) Out of the three characteristics we have looked at, the L1 hit ratio, the read sharing, or the write sharing, which is most important for determining performance? Use the average memory latency (and overall performance) to address this question.

Finally, you should have an idea of what optimizations have the biggest impact on the hit ratio, the read sharing performance, and the write sharing performance.

So: (b) Using data from the gem5 simulations, now answer what hardware characteristic causes the most important optimization to be the most important.

Answer 8


Question 9

Run using a xbar_latency of 1 cycle and 25 cycles (in addition to the 10 cycles that you have already run).

As you increase the cache-to-cache latency, how does it affect the importance of the different optimizations?

You don't have to run all algorithms. You can probably get away with just running algorithm 1 and algorithm 6.

Answer 9


