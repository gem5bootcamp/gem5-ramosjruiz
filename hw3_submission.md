A) What were you able to get working and why do you think that you couldn't get the final objective?

I could interface the different ports to handle requests and responses between the CPU side and the Memory side with the Cache in between. It wasn't actually doing anything, I just got the ports correct to log something before forwarding. I could not adapt the solution with an actual function, for example adding the insepction buffer to handle queued requests nor adding actual caching to the Metadata Cache. I could get the address ranges of the Merkle Tree to stop the simulation from crashing, but that just means I got the values to not access illegal memory since I could not encrypt/decrypt the packets. I could not implement the suggested modification on the packets themselves to register a cache hit or miss (obviously mocked since I mentioned before that I could not cache the packets).

B) Describe the experiment that you would run if everything was working.

I would like to test if computations were being done correctly, that is to say, I could store and transform information with encryption/decryption without loss of information. A good experiment for this would be to run the matrix operation workloads with and without secure memory and observing discrepancies in the results.

On the other hand I would like to see the performance overhead of having simple memory access vs secure memory access vs cached secure memory access. I would expect the simple memory access to perform best, seconded by cached secure memory access and secure memory access to be the slowest of the three. However, I do not have an estimate of the performance difference between each group, so I would run 2-3 performance-intensive workloads to accentuate the gaps in performance.

Lastly, I would like to see how well the secure memory with cache handles multiple cores, since we would have implemented the inspection buffer to handle CPU side requests. For this experiment I would run the same workload with different ammount of cores on the processor and observe the results.

C) Run some applications (getting started suite works, or matrix multiply) using the baseline and discuss the impact that you believe the secure memory changes will have.

With the X86 OOO setup there were no blocked cycles in the cache, I believe that would have not been the case with multiple cores accessing the Secure Memory, although the Metadata Cache could have guaranteed no blockage. I'm positive the number of ticks simulated, together with the memory used and the operations would have kept constant, since there would be no overhead that might affect those variables However I do think that the simulated seconds would have shot up considering the first part of the simulation the cache would be completely empty and all packets would have to traverse the Merkle Tree.

The hit ratio of the cache could be lower, since the secure memory implementation would have a single level cache and the baseline was a two level cache. Less space means repeated evictions and thus, more encrypting and decrypting packets which would in turn affect simulated time.

I don't think stats would have been affected at core processor level, although if the scenario previously mentioned where multiple cores were computing with uncached packets, contention could occur and the ammount of instructions commited each cycle could go down significantly.
