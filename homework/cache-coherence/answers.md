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

Question 5

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the hit ratio?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Question 6

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the read sharing?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Question 7

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the write sharing?

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

Question 8

Let's get back to the question we're trying to answer. From question 3 above, "What is it about the hardware that causes this optimization to be most important?"

So: (a) Out of the three characteristics we have looked at, the L1 hit ratio, the read sharing, or the write sharing, which is most important for determining performance? Use the average memory latency (and overall performance) to address this question.

Finally, you should have an idea of what optimizations have the biggest impact on the hit ratio, the read sharing performance, and the write sharing performance.

So: (b) Using data from the gem5 simulations, now answer what hardware characteristic causes the most important optimization to be the most important.

Question 9

Run using a xbar_latency of 1 cycle and 25 cycles (in addition to the 10 cycles that you have already run).

As you increase the cache-to-cache latency, how does it affect the importance of the different optimizations?

You don't have to run all algorithms. You can probably get away with just running algorithm 1 and algorithm 6.
