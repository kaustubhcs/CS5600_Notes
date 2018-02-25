# CS 5600

EECE 5600: Computer Systems
Visit the course syllabus page
OSTEP: http://pages.cs.wisc.edu/~remzi/OSTEP/
```
Main () {
Int i=4;
Int f = =fact (i);
printf(“/d”, f) 
}

Int fact (int i) {

}
```

HW2 Use GDB and check call frames
When you run into segfault, check backtrace!
64 bit machine has 8 byte int storage! Check this!
Dont do monkey coding

Get rid of all function and debugger libraries.

When the function call returns


Process
Program
Arguments main(int argc, char* argv[])

System Calls
Environment


SYSCALL
In main function, when you call open function

Int fd = open();

Open is not a syscall

Libc makes a syscall using a wrapper to wrap open for user




ENVIRONMENT is basically a string which states {Only constraint here, is the environment variable cannot contain =}
“USER=root”
“PATH=/bin:/”
“HOME=/root”

Const char* h = getenv(“HOME”)

H will point to first character after =

QUIZ Ques: what does putenv() do?
What does exec system call do?

The environment is inherited across fork!

execve(“/bin/ls”,args,environ)

Can pass values from environ

Both args and environ are array of pointers

Libc has char * environ


Char *p = environ[0]

While (p != NULL) {
	printf(%s,p);
	p=p+1;
}



Another signature of Main

Int main (int argc, char *argv[], char *envp[]) {

}

What is signal mask?

If called, will block particular signals till released


ASYNC SIGNAL SAFE
These are functions that are safe to be called from signal handler because they don’t hang when interrupted from a function.


socket(), open() → These are async signal safe


IPC: Inter process communication:
I/O and File descriptos F/D
PIPES
SOCKETS
SHARED MEM

/proc/PID/fd


F = dup(4)

F = dup2(4,7)

fstat(fd, ...)


On a new process creation, you get 3 filedescriptors by default;

0: STDIN
1: STDOUT
2: STDERR

IO Redirection
Fd = open(“out.txt”);
dup2(fd,1);



FILE DESCRIPTORS are carried forward to new processes, inherited




Threads

Share
File descriptors
Everything

But they don’t share the same stack
They have their own stacks!
Though the thread can overwrite on its parents stack;



Spin Locks

#### TEST_AND_SET



```
TST(&flag) {

If (flag ==0) {
Flag = 1;
Return 0;
}

Return 1; 
}
```
#### COMPARE EXCHANGE

```
CMPXCHG (int *p,int old, int new) {
 If (*p == old)
*p = new;

Return old;
}
```
Suppose say you have 
```p = 0; old = 0; new = 1```

then 


READ FUTEX for quiz for next lecture!
Read paper!

Will learn about futex and semaphores and spin locks!


HW 3/4
HW4 Optimize HW3 code:- 

Perthread Arena
Per core arena
CPU AFFINITY!


LATE?

LOCKS


Threads


Virtual Memory

Program not compilable is a straight ZERO;
Correctness: 1 bug is ok, maybe some marks may be cut.
2 bugs is a straight ZERO in assignment.





Fork in threads
If you fork in multithreaded program, all other threads die, only the one who forked is alive

`pthread_atfork()`


mallinfo() // Use Standard struct given on mallinfo

atexit() // You may print info about your malloc allocations at exit.


Spinlock can be completely implemented in usersapce, just it will burn cpu cycles.

A --> B --> C --> D








### FOR LOCKING
```
sys_lock(lock) {
if (lock -> OWNER == 0){
    lock->owner = caller();
    return;
    }
}

yield(); --> Tells kernel to do some other work

enqueue(lock,tid)

```










### FOR UNLOCKING

```
sys_unlock () {
lock->owner = 0;
wakeup(tid);
}
```




More memory per lock as opposed to more mmoery per thread || THINK!




## FUTEX

Fast user level locks


```
lock() {
CMPXCHG(lock->OWNER,NULL,getpid());
return

syscall();
}
```


SN (Self note): // Read Compare and exchange!

Problems:

1. Deadlock
2. Unlock without locking


##  Unlock without locking

```
unlock() {
if (l -> owner != gettid()) {
return error;
}
l -> owner = 0;
}
```


Classic Deadlock

Thread 1: lock(A) lock(B)



Thread 2: lock(B) lock(A)

Lock Ranking:
Let lock have numbering: And you can acquire lock only for a lock that is in a particular sequence, as in all one after two etc.


## Read the FUTEX Paper for quiz next time


`FUTEX_WAIT (addr, old, new)`

`FUTEX_WAKE (addr, number)`

Quiz will e on implementation of `FUTEX_WAIT and FUTEX_WAKE`







```
int counter = 0;

pthread_lock(); // You should use CMPXCHG, because the code inside the lock is small so CMPXCHG will not wait for long, so not many CPU cycles will get wasted.

counter++;

pthread_unlock();

```

```
FETCH_AND_ADD (&counter, 1) {
while (1) {
int old = *counter
    if (cmpxchg(&counter,old,old+1)==old) {
        return;
        }
    }
}
```

Lookup GCC Atomics

`__sync`
`_fetch_and_add`
`_fetch_and_and`
`_fetch_and_or`


`-LOCK-FREE`
`-WAIT-FREE`




ABA Problem

Lock Free Conditions



Produce Consumer Queue

```
% Producer
enqueue ();
```


```
% Consumer
item = degueue();
```


```
enqueue() {
lock ();

// CODE

unlock();
}
```



```
dequeue() {
lock ();

// CODE

unlock();
}
```




Binary Semaphore



## READ: Condition Variales and FUTEXES for Quiz next lec!!!



# Virtual Memory




Page tables -> 2 types,


Page directories
MULTI LEVEL PAGE TABLES

TLB









