# Assignment 3 - Complete Documentation

**Student Name**: [Rahaf Mubarak Aldawsari]  
**Student ID**: [445052029]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `445052029_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 -May 3, 2026 (3:00 PM)
**What I implemented**: 

I started by analyzing the provided code and identifying shared resources such as counters and execution log.

**Challenges encountered**: 

Understanding where race conditions occur in multithreaded execution.

**How I solved it**: 

reviewed the lecture slides and textbook concepts about race conditions.

**Testing approach**: 
horan the program without synchronization to observe behavior.
**Time spent**: 
2 hours

Entry 2 - May 5, 20
---

### Entry 2 - May 3, 2026 (6:00 PM)
**What I implemented**: 

Added ReentrantLock to protect shared counters (contextSwitchCount, completedProcessCount, totalWaitingTime).
**Challenges encountered**: 
Ensuring correct placement of lock and unloc
**How I solved it**: 
Used try-finally blocks as recommended in OS concepts
**Testing approach**: 
Tested program multiple times to verify consistent results.
**Time spent**: 
1 hours
---

### Entry 3 -May 4, 2026 (7:00 PM)
**What I implemented**: 
Protected executionLog using the same lock to avoid ConcurrentModificationException.
**Challenges encountered**: 
Understanding why ArrayList is not thread-safe.
**How I solved it**: 
lied lock around add() operation
**Testing approach**: 
Ran program repeatedly and checked log size
**Time spent**: 
1.15 hours
---

### Entry 4 -May 5, 2026 (12:00 PM)
**What I implemented**: 
Added Semaphore to control CPU access.
**Challenges encountered**: 
Understanding how semaphore limits concurrent execution.
**How I solved it**: 
Used binary semaphore (1 permit) before process execution.
**Testing approach**: 
Verified that only one process executes at a time.
**Time spent**: 
2 hours
---

### Entry 5 - May 3, 2026 (5:00 PM)
**What I implemented**: 
Final testing, debugging, and validation of output
**Challenges encountered**: 
Ensuring no deadlocks occur.
**How I solved it**: 
Used try-finally to always release locks and semaphores.
**Testing approach**: 
Ran program 5+ times and compared results.
**Time spent**: 
1.5 hours
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?
  
**Your Answer**:
First, the variable contextSwitchCount is shared among threads. Multiple threads may increment it simultaneously, causing incorrect values due to lost updates.

Second, the executionLog (ArrayList) is accessed by multiple threads without synchronization. Since ArrayList is not thread-safe, concurrent modifications may lead to inconsistent data or runtime exceptions.

These issues occur because threads execute concurrently without mutual exclusion, leading to unpredictable behavior.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to ensure mutual exclusion, allowing only one thread to access a critical section at a time. It is suitable for protecting shared variables.

Semaphore, on the other hand, controls access to a limited number of resources. It allows a fixed number of threads to access a resource simultaneously.

In my code, I used ReentrantLock to protect shared counters and execution log, and a binary Semaphore to ensure only one process accesses the CPU at a time.
[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock is a situation where two or more threads are waiting indefinitely for each other to release resources.

One prevention technique is using try-finally blocks to ensure locks are always released.

Another technique is maintaining a consistent locking order.

In my implementation, I used try-finally blocks for both locks and semaphore release to ensure no thread
holds a resource forever.



---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
used a single lock (coarse-grained locking) to protect all shared counters.

This approach simplifies the implementation and avoids complexity in managing multiple locks.

However, it reduces concurrency because only one thread can access any counter at a time.

Fine-grained locking (separate locks) would improve concurrency but increases complexity and risk of deadlock.

Since the counters are simple and frequently accessed, coarse-grained locking is acceptable for this assignment
[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:  contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**:  They are shared among multiple threads and can be updated simultaneously.

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
Ensures atomic updates and prevents lost updates.
---

### Critical Section #2: Execution Log

**What resource**: executionLog (ArrayList)

**Why it needs protection**: 
ArrayList is not thread-safe and concurrent access may cause inconsistency.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
```
lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}
**Justification**: 
Prevents concurrent modification issue
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Control CPU acces
**Number of permits and why**: 
1 permit → only one process at a time
**Where implemented**: 
Inside run() method
**Code snippet**:
```java
// Paste your implementation here
``SharedResources.cpuSemaphore.acquire();

SharedResources.cpuSemaphore.release();`

**Effect on program behavior**: 
Ensures controlled execution and prevents simultaneous CPU usage.
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
⏱ Time Quantum:  4000ms                                                           ║
║  🔑 Student ID:    445052029                                                        ║
║  🔒 Sync Mode:     Mutex Locks & Semaphore
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
⏱ Time Quantum:  4000ms                                                           ║
║  🔑 Student ID:    445052025                                                        ║
║  🔒 Sync Mode:     Mutex Locks & Semaphore
**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 
different numbers of processes and time quantum values generated using the random seed (student ID).
**Results**: 

program successfully handled different scenarios without errors. Regardless of the number of processes or time quantum values, all processes completed execution and statistics remained accurate.
**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
Synchronization ensures that the system behaves correctly regardless of input variability. It makes the program robust and reliable under different execution conditions.
[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:
hrough this assignment, I learned that synchronization is essential in multithreaded systems to prevent race conditions and ensure data consistency. I understood how shared resources can lead to unpredictable behavior when accessed concurrently. Using ReentrantLock helped me implement mutual exclusion, ensuring that only one thread accesses critical sections at a time. I also learned how semaphores control access to limited resources such as CPU execution. Another important concept was the use of try-finally blocks to prevent deadlocks by guaranteeing resource release. Additionally, I realized the importance of thread-safe data structures and proper synchronization design. Overall, this assignment strengthened my understanding of concurrency control in operating systems.
Give TWO examples where synchronization is critical:

**Example 1**: 

Banking systems where multiple users access and update account balances simultaneously. Synchronization ensures that transactions are processed correctly without losing updates.
**Example 2**: 
operating system process scheduling, where multiple processes compete for CPU access. Synchronization ensures fair and controlled execution.
---

### How I would explain synchronization to others:
synchronization is like managing access to a shared resource such as a printer. If multiple people try to use it at the same time, their jobs may get mixed up. So, we use a system where only one person can use the printer at a time. In programming, synchronization ensures that only one thread accesses shared data at a time to avoid conflicts and errors.
[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**:  https://github.com/Rahaf1230/OS-Assignment3-Rahaf-Mubarak.git

**Number of commits**: 14

**Commit messages**: 
1. change my student id to 445052029
2.Add ReentrantLouck and import package 
3. add semaphore and import its package for synchronization
4. Secured the shared variable (contextSwitchCount) using a ReentrantLoc
5. protect the shared variable completedProcessCount using ReentrantLock
6. protecting shared variable totalWaitingTime using ReentrantLock
7. protecting executionLog
8. use Semaphore to control CPU access in process execution and in fina
9. Apply semaphore in runToCompletion method
10. answering Part 1: Development Log
11. answer Part 2: Technical Questions
12. annswer art 3: Synchronization Analysis

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. Synchronization is essential to prevent race conditions.
2.Locks ensure mutual exclusion in critical sections 
3. Semaphores control access to limited resources like CPU

**Most challenging aspect**: 

Understanding where race conditions occur and correctly applying synchronization without causing deadlocks.
**What I'm most proud of**: 

Successfully implementing synchronization mechanisms that ensure correct and consistent program execution across multiple runs.
---

**End of Documentation**
