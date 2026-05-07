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

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
