1. Using the wrong complier
```bash
We didn't notice that g++ is used instead of gcc. Fixing the issue on mac took time before the code could be run error free.
```

2. redefinition of 'semun' in zgt_semaphore.c
```bash
`union semun` is defined by most platforms in their header files .(sys/sem.h)
This was causing compilation error on mac since the header file sys/sem.h already includes it.
Removing the reference to header files caused major issues so there were 2 options:

    1. Check for the paltform and then use the semun
        #if defined (__FreeBSD__)
        #else

    2. Use a different semun instance

We opted for option 2 to avoid conflicts with other platforms and renamed semun to semun1 thus creating
an alternate instance.
```

3. undefined reference to pthread_create and pthread_join (in linux distribution: Ubuntu 16.04)
```bash
This error was caused because -lpthread was used in the make file to make the executables. In linux distribution 16.04 (14.04 and above) -lpthread was replaced by -pthread to make it work.
An alternative was to move all libraries behind the objects needing them but we went with first option.
```

4. Segmentation fault (code dumped)
```bash
We encounterd this couple of times through the testing of our code. Major reason which caused this was we were trying to reference a memory that did not existed (dead nodes).

We used valgrind tool to look into the error and get the information for the cause.
```

5. Encountered problems while releasing the semaphores when a transaction ends and waking up other transactions waiting for that object. After meeting the TA and understanding the concept we were able to reslove this.

6. We faced problem implementing the set_lock method as it invloves a lot of conditions to be taken care of. We used VS Code C++ debugging tool to get the value at each step and check when the object can be acquired by other transaction.