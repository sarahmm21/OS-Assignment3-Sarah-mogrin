# Assignment 3 - Complete Documentation

**Student Name**: [Sarah almogrin]  
**Student ID**: [445052163]  
**Date Submitted**: [ 7 may ]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 5, 2026 - 3:00 PM]
**What I implemented**: 
I added the ReentrantLock to protect shared variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime.
**Challenges encountered**: 
At first, I did not know where to place the lock and unlock statements correctly.
**How I solved it**: 
I used lock.lock() before modifying shared resources and lock.unlock() inside a finally block.
**Testing approach**: 
I ran the program several times and checked if the counters updated correctly.
**Time spent**: 
1 hour
---

### Entry 2 - [May 5, 2026 - 5:00 PM]
**What I implemented**: 
I implemented the Semaphore to control CPU access and allow only one process to run at a time.
**Challenges encountered**: 
the program sometimes froze because the semaphore was not released correctly.
**How I solved it**: 
I moved cpuSemaphore.release() into the finally block to ensure it always releases.
**Testing approach**: 
I tested the scheduler using multiple processes and checked if execution completed successfully.
**Time spent**: 
2 hour
---

### Entry 3 - [May 6, 2026 - 1:00 PM]
**What I implemented**: 
I synchronized the execution log and waiting time calculations.
**Challenges encountered**: 
accidentally used tryLock() instead of unlock(), which caused deadlock issues.
**How I solved it**: 
I replaced all incorrect tryLock() calls with unlock().
**Testing approach**: 
reran the program and verified that all processes completed without freezing.
**Time spent**: 
2 hour
---

### Entry 4 - [May 6, 2026 - 4:00 PM]
**What I implemented**: 
I fixed synchronization issues in the scheduler and completed the remaining critical sections.
**Challenges encountered**: 
Ensuring that all threads completed execution without conflicts.
**How I solved it**: 
I reviewed synchronization methods and ensured resources were accessed safely
**Testing approach**: 
i executed the program multiple times and checked that all processes completed correctly.
**Time spent**: 
1 hour
---

### Entry 5 - [7 may, 2:00pm]
**What I implemented**: 
I finalized testing, verified statistics, and prepared the final project version for GitHub submission.
**Challenges encountered**: 
Verifying the correctness of synchronization results and scheduler statistics.
**How I solved it**: 
I compared execution outputs across multiple runs and reviewed shared resource updates.
**Testing approach**: 
I tested different scheduling scenarios and confirmed stable execution.
**Time spent**: 
1.5 hour
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected? 
 The first shared resource is contextSwitchCount, which is updated by multiple threads during process execution. Another shared resource is executionLog, where all threads add execution messages.
- Why is concurrent access a problem?
  Concurrent access is a problem because multiple threads may try to modify the same resource at the same time. This can cause inconsistent or incorrect data because thread operations may overlap during execution.
- What incorrect behavior could occur?
Incorrect behavior may include wrong counter values, missing execution log entries, or corrupted shared data. Without synchronization, the scheduler statistics may become inaccurate and program behavior may become unpredictable.
**Your Answer**:

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock provides mutual exclusion and allows only one thread to access a critical section at a time. Semaphore controls access to a shared resource by limiting the number of threads that can use it simultaneously 
I used ReentrantLock to protect shared counters such as contextSwitchCount, completedProcessCount, totalWaitingTime, and the shared executionLog.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
The first prevention technique is using finally blocks to guarantee that locks and semaphores are always released. The second prevention technique is minimizing nested locks and keeping synchronization simple to reduce thread dependency.
I released all locks and semaphores inside finally blocks using unlock() and release(). This ensured that resources were always freed after execution and prevented threads from remaining blocked.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used one ReentrantLock for all three shared counters, which is considered coarse-grained locking. I chose this approach because it simplifies synchronization and makes the implementation easier to manage. Using a single lock reduces the possibility of synchronization mistakes and keeps the code more organized. However, the disadvantage is that only one thread can access the protected resources at a time, which reduces concurrency. Fine-grained locking uses separate locks for each shared resource, allowing multiple threads to work independently at the same time. Since the three counters are independent, fine-grained locking would provide better concurrency and improve performance. Despite that, I selected coarse-grained locking because it was simpler, safer, and sufficient for the requirements of this assignment.


---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, and totalWaitingTime
**Why they need protection**: 
These variables are shared between multiple threads and may be updated at the same time. Without synchronization, race conditions could occur and produce incorrect values.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here 
lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}
```

**Justification**: 
The lock ensures that only one thread updates the counters at a time, preventing inconsistent results.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog
**Why it needs protection**: 
Multiple threads write to the same ArrayList, and concurrent modification may corrupt the log or cause inconsistent entries.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here 
lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}
```

**Justification**: Synchronization protects the shared list and ensures execution messages are added safely.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control CPU access and ensure that only one process executes at a time.
**Number of permits and why**: 
1 permit was used because the simulation represents a single CPU resource.
**Where implemented**: 
Inside the run() and runToCompletion() methods.
**Code snippet**:
```java
// Paste your implementation here
SharedResources.cpuSemaphore.acquire();

try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 
The semaphore prevents multiple processes from using the CPU simultaneously and improves synchronization between threads.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results
I tested the scheduler by running the program multiple times to verify that synchronization produced stable and consistent results during execution.
**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**: 
(Show that running multiple times produces consistent, correct results) 
Total Context Switches: 30
Total Completed Processes: 16
Total Waiting Time: 930219ms
Average Waiting Time: 58138ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         2            7668         54513       
P2         4            5486         58181       
P3         3            2185         8076        
P4         4            5899         59673       
P5         4            8339         81119       
P6         4            9927         81467       
P7         5            2628         22314       
P8         2            5368         69598       
P9         4            3676         28969       
P10        3            3517         32652       
P11        4            2265         36179       
P12        1            8447         83397       
P13        5            5447         74983       
P14        3            7040         76439       
P15        2            7676         79487       
P16        4            5941         83172 

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.) 
Synchronization is necessary because multiple threads access shared resources concurrently. Without synchronization, race conditions could occur when threads update shared counters or write to the execution log simultaneously. This may lead to incorrect context switch values, inaccurate waiting times, or inconsistent execution logs. Protecting shared resources using locks ensures thread-safe execution and correct scheduler statistics.

**Conclusion**: 
The consistency test confirmed that synchronization mechanisms successfully protected shared resources and maintained stable scheduler behavior during concurrent execution.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException
tested whether concurrent access to the shared execution log could cause ConcurrentModificationException or inconsistent data.
**Testing procedure**: 
I executed the scheduler with multiple processes running concurrently while continuously updating the shared executionLog. I also repeated execution several times to observe program stability during concurrent modifications.
**Results**: 
The program executed successfully without throwing any exceptions. All execution log entries were stored correctly, and no corrupted or missing data appeared.
**What this proves**: 
This proves that the ReentrantLock correctly synchronized access to the shared executionLog. The shared ArrayList was safely protected from concurrent modification issues.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
I verified that the final scheduler statistics and process execution results were correct after synchronization.
**Expected values**: 
All processes should complete successfully.
completedProcessCount should equal the total number of processes.
Context switch values should increase correctly during execution.
Waiting times should remain positive and realistic.
The execution log should contain valid process events.
**Actual values**: 
The scheduler produced correct statistics after execution. All processes completed successfully, context switches were counted properly, and waiting times were calculated correctly. The execution log displayed valid scheduling activity for every process.
**Analysis**: 
The correctness test confirmed that synchronization protected all shared resources successfully. Shared counters remained accurate even when accessed by multiple threads concurrently. The scheduler behavior matched the expected Round Robin execution logic.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
tested the scheduler using different process counts and different time quantum values generated by the student ID seed.
**Purpose**: 
The purpose of this test was to verify scheduler stability and synchronization reliability under different execution conditions.
**Results**: 
The scheduler continued to execute correctly in all scenarios. Processes were scheduled properly, synchronization remained stable, and no deadlocks or race conditions occurred. Larger process counts increased execution time but did not affect correctness.
**What I learned**: 
I learned that synchronization mechanisms such as locks and semaphores improve program reliability under concurrent execution. I also learned that different scheduling scenarios can affect performance, but proper synchronization ensures stable and correct behavior regardless of workload size
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights] 
I learned that synchronization is very important in multithreaded programs because multiple threads may access shared resources at the same time. Without synchronization, race conditions can occur and produce incorrect or inconsistent results. I learned how ReentrantLock protects critical sections by allowing only one thread to access shared data at a time. I also learned how semaphores control access to limited resources such as the CPU. Another important lesson was the importance of releasing locks and semaphores correctly to avoid deadlocks and blocked threads. Testing concurrent programs helped me understand that synchronization issues may not always appear immediately and often require repeated execution to detect. Overall, this assignment improved my understanding of thread safety, process scheduling, and synchronization techniques in Java

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems use synchronization to protect account balances when multiple users perform transactions at the same time.

**Example 2**: Operating systems use synchronization when multiple processes compete for CPU access and shared system resources.

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]
Synchronization is a way to organize threads so they can safely share resources without interfering with each other. It works similarly to traffic control at an intersection, where rules prevent cars from crashing into each other. Locks allow only one thread to access shared data at a time, while semaphores control how many threads can use a resource simultaneously. Without synchronization, programs may produce incorrect results because multiple threads execute concurrently. Synchronization helps maintain data consistency, program stability, and safe execution in multithreaded applications.
---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/sarahmm21/OS-Assignment3-Sarah-mogrin.git

**Number of commits**: 15

**Commit messages**: 
1- update student ID to
2- Add ReentrantLock for synchronization
3- add Semaphore and import
4- protect shared contextSwitchCount using lock and release
5- protect shared completedProcessCount using lock and unlock
6- protect shared totalWaitingTime using lock and unlock to prevent deadlock
7- protect shared executionLog using lock and unlock
8- use Semaphore to control CPU access in process execution
9- apply semaphore in runToCompletion method
10- unlock update
11- answer part 2
12- answer part 3
13- answer part 4
14- part 5

---

## Summary

**Total time spent on assignment**: 3 days 

**Key takeaways**: 
1- Synchronization is important to protect shared resources in multithreaded programs.
2- ReentrantLock and Semaphore help prevent race conditions and improve thread safety.
3- Proper synchronization improves program stability and ensures correct scheduler execution.

**Most challenging aspect**: 
Understanding how to correctly synchronize shared resources and manage thread execution safely.
**What I'm most proud of**: 
Successfully implementing synchronization mechanisms and making the scheduler execute correctly without race conditions or deadlocks
---

**End of Documentation**
