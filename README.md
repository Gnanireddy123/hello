QUESTION :. Write a multi-threaded C program that gives readers priority over writers concerning a shared (global) variable. Essentially, if any readers are waiting, then they have priority over writer threads -- writers can only write when there are no readers. This program should adhere to the following constraints:
    Multiple readers/writers must be supported (5 of each is fine)
    Readers must read the shared variable X number of times
    Writers must write the shared variable X number of times
    Readers must print:
        The value read
        The number of readers present when value is read
    Writers must print:
        The written value
        The number of readers present were when value is written (should be 0)
    Before a reader/writer attempts to access the shared variable it should wait some random amount of time
        Note: This will help ensure that reads and writes do not occur all at once
    Use pthreads, mutexes, and condition variables to synchronize access to the shared variable


SOLUTION:  If one of the people tries editing the file, no other person should be reading or writing at the same time, otherwise changes will not be visible to him/her.
However if some person is reading the file, then others may read it at the same time.
Precisely in OS we call this situation as the readers-writers problem

Problem parameters:



 

One set of data is shared among a number of processes
Once a writer is ready, it performs its write. Only one writer may write at a time
If a process is writing, no other process can read it
If at least one reader is reading, no other process can write
Readers may not write and only read
Solution when Reader has the Priority over Writer

Here priority means, no reader should wait if the share is currently opened for reading.

Three variables are used: mutex, wrt, readcnt to implement solution

semaphore mutex, wrt; // semaphore mutex is used to ensure mutual exclusion when readcnt is updated i.e. when any reader enters or exit from the critical section and semaphore wrt is used by both readers and writers
int readcnt;  //    readcnt tells the number of processes performing read in the critical section, initially 0
Functions for sempahore :



 

– wait() : decrements the semaphore value.

– signal() : increments the semaphore value.

Writer process:

Writer requests the entry to critical section.
If allowed i.e. wait() gives a true value, it enters and performs the write. If not allowed, it keeps on waiting.
It exits the critical section.


Reader process:

Reader requests the entry to critical section.
If allowed:
it increments the count of number of readers inside the critical section. If this reader is the first reader entering, it locks the wrt semaphore to restrict the entry of writers if any reader is inside.
It then, signals mutex as any other reader is allowed to enter while others are already reading.
After performing reading, it exits the critical section. When exiting, it checks if no more reader is inside, it signals the semaphore “wrt” as now, writer can enter the critical section.
If not allowed, it keeps on waiting.
Thus, the semaphore ‘wrt‘ is queued on both readers and writers in a manner such that preference is given to readers if writers are also there. Thus, no reader is waiting simply because a writer has requested to enter the critical section.

Article contributed by Ekta Goel. Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above


