# Technical Interview: System Analysis Questions

## Part 1

**1. What is the PID for the Java process using up the most RAM?**

PID: 21651

This process is using 6.5% of the available system RAM

**2. What is the PID for the Java process consuming the most CPU?**

PID: 22460

This process is using 475% of the available CPU (Multi-core system)

**3. Is this system in distress?**

The system appears to be in distress, because the process 22460 is monopolizing CPU execution time.  The load average is steadily increasing and currently sits at 3.85 which is getting high, even for a multi-core system.

## Part 2

**In the below screenshot, what is the PID of the java process with the largest heap?**

It cannot be determined which PID has the largest heap, as memory usage is not displayed for each process.

## Part 3

**1. In general, what are we looking at here?**

We are looking at a performance monitoring application (New Relic APM) which monitors how memory is being used by various JVMs.

**2. Explain the middle three graphs, Par Eden Space, Par Survivor, and CMS Old Gen heap usage.**

Par Eden Space: This memory pool is used to initially allocate java objects on the heap.

Par Survivor: Objects which survive the initial garbage collection in the Eden Space reside here

CMS Old Gen Heap: This memory pool is used to store long surviving objects.  

**3. Is the system in distress?**

The system is not in distress, because the used heap space does not approach the total committed heap space in the Old Gen Heap.

