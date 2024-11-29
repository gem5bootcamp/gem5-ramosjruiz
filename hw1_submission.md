# Performance Evaluation Report

---

## **Question 1**

**For Algorithm 1, does increasing the number of threads improve or hurt performance? Use data to back up your answer.**

### **Naive Algorithm**  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.287470           |  
| 2          | 0.354853           |  
| 4          | 0.325001           |  
| 8          | 0.320909           |  

**Answer:**  
Algorithm 1 performed best with 1 thread and worst with 2 threads. Increasing the thread count generally hurt performance.

---

## **Question 2**

**(a) For Algorithm 6, does increasing the number of threads improve or hurt performance? Use data to back up your answer.**  
**(b) What is the speedup when using 2, 4, 8, and 16 threads?**

### **All Algorithm**  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.262452           |  
| 2          | 0.165475           |  
| 4          | 0.156006           |  
| 8          | 0.269185           |  

**(a) Answer:**  
Algorithm 6 performed best with 4 threads and worst with 8 threads. Performance improved initially but decreased beyond 4 threads.

**(b) Speedup:**  
Taking 1 core as a base:  
- **2 cores:** 1.59  
- **4 cores:** 1.68  
- **8 cores:** 0.97  

---

## **Question 3**

(a) Using the data for all 6 algorithms, what is the most important optimization: chunking the array, using different result addresses, or padding between result addresses?  
(b) Speculate how the hardware implementation causes this result.

**Refer to `/data/cache-coherence/performance-gem5-mesi`:**  

**(a) Answer:**  
The best performance was seen in algorithms 5 and 6, indicating that **padding between result addresses** is the most important optimization.

**(b) Answer:**  
Poor cache usage seems to be the main bottleneck, so optimizations reducing cache conflicts significantly improve performance.

---

## **Question 4**

**(a) What is the speedup of Algorithm 1 and Algorithm 6 on 16 cores as estimated by gem5?**  
**(b) How does this compare to real system performance?**

### **Naive Algorithm**  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.838              |  
| 2          | 0.779              |  
| 4          | 0.818              |  
| 8          | 0.881              |  
| 16         | 0.917              |  

### **All Algorithm**  
| Core Count | Average Time (ms) |  
|------------|--------------------|  
| 1          | 0.839              |  
| 2          | 0.438              |  
| 4          | 0.236              |  
| 8          | 0.142              |  
| 16         | 0.111              |  

**(a) Speedup:**  
- **Algorithm 1 (Naive):** 0.914  
- **Algorithm 6 (All):** 7.56  

**(b) Comparison:**  
gem5 estimated speedup (0.914) is similar to the real system (0.897) for Algorithm 1. For Algorithm 6, gem5’s estimated speedup (7.56) is significantly higher than the real system (0.975).

---

## **Question 5**

**Which optimization has the biggest impact on the hit ratio? Show data and discuss algorithms compared.**

### **Cache Hit Rates:**  
| Algorithm       | L1 Cache Hit Rate (%) | L2 Cache Hit Rate (%) |  
|-----------------|-----------------------|-----------------------|  
| All             | 99.45                 | 71.70                 |  
| Block           | 96.64                 | 89.65                 |  
| Chunk           | 96.29                 | 71.38                 |  
| Chunk-race      | 96.72                 | 32.36                 |  
| Naive           | 93.90                 | 90.01                 |  
| Race            | 93.91                 | 73.40                 |  

**Answer:**  
While the base hit rate for the naive approach was high (93%), combining optimizations boosted it to 96%. The **"All"** algorithm, combining all strategies, showed the highest improvement.

---

## **Question 6**

**Which optimization has the biggest impact on read sharing? Show data and discuss algorithms compared.**

### **Read Sharing:**  
| Algorithm  | Total Read Share |  
|------------|------------------|  
| All        | 626              |  
| Block      | 2404             |  
| Chunk      | 599              |  
| Chunk-race | 610              |  
| Naive      | 2380             |  
| Race       | 2385             |  

**Answer:**  
**Chunking** had the most significant impact, reducing read sharing drastically compared to naive and race algorithms.

---

## **Question 7**

**Which optimization has the biggest impact on write sharing? Show data and discuss algorithms compared.**

### **Write Sharing:**  
| Algorithm     | Total Write Share |  
|---------------|-------------------|  
| All           | 230               |  
| Block         | 192               |  
| Chunk         | 32863             |  
| Chunk-race    | 32776             |  
| Naive         | 32906             |  
| Race          | 32814             |  

**Answer:**  
**Padding between addresses** had the largest impact, as write sharing dropped from around 32,000 in chunk, naive and race methods to 200 in the "Block" and "All" algorithms.

---

## **Question 8**

**(a) Which hardware characteristic (L1 hit ratio, read sharing, or write sharing) is most important for performance? Use average memory latency to justify.**  
**(b) What hardware characteristic causes this optimization to be most important?**

### **Question 8**

**(a) Out of the three characteristics we have looked at—L1 hit ratio, read sharing, or write sharing—which is most important for determining performance? Use average memory latency and overall performance to address this question.**  

**(b) Using data from the gem5 simulations, what hardware characteristic causes the most important optimization to be the most important?**

**Answer 8(a):**  

The **write sharing** characteristic has the most significant impact on performance. This is evident from the drastic reduction in execution time when padding between result addresses is applied, as seen in the "All" and "Block" algorithms. While L1 hit ratios were already high and improved only marginally, reducing write sharing significantly reduced average memory latency. 

#### Supporting Data:
- **Write Sharing (Total Write Share):**  
  - **Chunk/Chunk-race:** ~32,000 (high contention)  
  - **All/Block:** ~230 (low contention, better performance)  

- **Performance Comparison:**  
  - **Naive (16 cores):** 0.917 ms (high write sharing, poor scaling)  
  - **All (16 cores):** 0.111 ms (low write sharing, excellent scaling)  

The drastic difference in execution time correlates with reduced write sharing, indicating it as the bottleneck.

---

### **Answer 8(b):**  

The main hardware characteristic driving this optimization is the **cache coherence protocol**. When multiple cores write to nearby memory addresses, frequent cache invalidations and updates increase latency due to coherence traffic. Padding reduces false sharing by ensuring each core operates on separate cache lines, minimizing coherence overhead. 

#### Why is this important?  
- **False sharing** causes frequent invalidations, reducing effective cache utilization.  
- **Padding** aligns data to prevent multiple cores from accessing the same cache line, improving overall efficiency.

This hardware limitation makes minimizing write sharing crucial for multi-threaded performance.

---

## **Question 9**

**How does increasing cache-to-cache latency (1, 10, 25 cycles) affect the importance of optimizations?**

**Answer:**  
### **Simulated Time (simSeconds)**  

#### Algorithm 6
##### **1-Cycle Latency**  
| Core Count | Simulated Time (Milliseconds) |  
|------------|-----------------------|  
| 1          | 0.833                 |  
| 2          | 0.427                 |  
| 4          | 0.226                 |  
| 8          | 0.134                 |  
| 16         | 0.102                 |  

##### **25-Cycle Latency**  
| Core Count | Simulated Time (Milliseconds) |  
|------------|--------------------------|  
| 1          | 0.847                 |  
| 2          | 0.456                 |  
| 4          | 0.251                 |  
| 8          | 0.156                 |  
| 16         | 0.127                 |


### **Simulated Time (simSeconds)**  
#### **1-Cycle Latency**  
| Core Count | Simulated Time (Milliseconds) |  
|------------|--------------------------|  
| 1          | 0.833                    |  
| 2          | 0.573                    |  
| 4          | 0.347                    |  
| 8          | 0.338                    |  
| 16         | 0.349                    |
  
#### **25-Cycle Latency**  
| Core Count | Simulated Time (Milliseconds) |  
|------------|--------------------------|  
| 1          | 0.847                    |  
| 2          | 0.101                    |  
| 4          | 0.958                    |  
| 8          | 0.988                    |  
| 16         | 0.176                    |  

Increasing cache-to-cache latency tends to hurt performance more in the Naive Algorithm than in Algorithm 6.
At 1-Cycle Latency, the time decreases as core count increases for both algorithms, but Algorithm 6 shows more consistent performance improvement across cores, while Naive sees a small drop-off.
At 25-Cycle Latency, the performance drop in Naive is more pronounced, with significant jumps in simulated time for 2, 4, and 8 cores, and 16 cores exhibiting the highest time increase compared to Algorithm 6.

Impact on Optimizations:

Algorithm 6 benefits significantly from optimizations such as chunking and padding, especially under higher latencies (e.g., 25 cycles), where performance gains from optimizing cache behavior become more evident.
For Naive, optimizations that improve cache usage are crucial for minimizing latency impacts, particularly as latency increases, since the basic approach has fewer optimizations to rely on.