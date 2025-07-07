# Dev:Locking - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Locking
TECHNICAL MANUAL Locking \- Memory locking 

## Synopsis

Prior to [Hassayampa (Version 3.0)]
    

Core: 
    
    
    lock(unsigned int *lock);
    unlock(unsigned int *lock);
    

Modules: 
    
    
    LOCK(unsigned int *lock);
    UNLOCK(unsigned int *lock);
    LOCK_OBJECT(object *obj);
    UNLOCK_OBJECT(object *obj);
    

[Hassayampa (Version 3.0)] and later
    

Core: 
    
    
    wlock(unsigned int *lock);
    rlock(unsigned int *lock);
    unlock(unsigned int *lock);
    

Modules: 
    
    
    READLOCK(unsigned int *lock);
    WRITELOCK(unsigned int *lock);
    UNLOCK(unsigned int *lock);
    READLOCK_OBJECT(object *obj);
    WRITELOCK_OBJECT(object *obj);
    UNLOCK_OBJECT(object *obj);
    LOCKED(object *obj,(command-list));
    

## Description

Memory locking is used to prevent non-atomic memory access operations from allowing read/write mishaps, such as illustrated by the following simple example. 

CPU 0 | CPU 1 | X | Remark   
---|---|---|---  
read X |  | 0 |   
| read X | 0 |   
write X+1 |  | 1 | CPU 0 adds 1 to its value of X   
| write X+1 | 1 | CPU 1 adds 1 to its value of X   
read X |  | 1 | value of X is not 2 as expected   
| read X | 1 | value of X is not 2 as expected   
  
Prior to [Hassayampa (Version 3.0)] the problem is addressed by restricting access to memory using a spin lock: 

CPU 0 | CPU 1 | X | Remark   
---|---|---|---  
|  | 0 | Initial state   
lock X |  | 0 | CPU 0 lock ok   
read X |  | 0 |   
| lock X | 0 | CPU 1 blocked   
write X+1 |  | 1 | CPU 0 adds 1 to its value of X   
unlock X |  | 1 | CPU 1 lock ok   
lock X |  | 1 | CPU 0 blocked   
| read X | 1 |   
| write X+1 | 2 | CPU 1 adds 1 to its value of X   
| unlock X | 2 | CPU 0 lock ok   
| lock X | 2 | CPU 1 blocked   
read X |  | 2 | value of X is 2 as expected   
unlock X |  | 2 | CPU 1 lock ok   
| read X | 2 | value of X is 2 as expected   
| unlock X | 2 |   
  
As of [Hassayampa (Version 3.0)] the problem is addressed by restricting access to memory using a R/W lock: 

CPU 0 | CPU 1 | X | Remark   
---|---|---|---  
|  | 0 | Initial state   
wlock X |  | 0 | CPU 0 lock ok   
read X |  | 0 |   
| wlock X | 0 | CPU 1 blocked   
write X+1 |  | 1 | CPU 0 adds 1 to its value of X   
unlock X |  | 1 | CPU 1 lock ok   
rlock X |  | 1 | CPU 0 blocked   
| read X | 1 |   
| write X+1 | 2 | CPU 1 adds 1 to its value of X   
| unlock X | 2 | CPU 0 lock ok   
| rlock X | 2 | CPU 1 lock ok   
read X |  | 2 | value of X is 2 as expected   
| read X | 2 | value of X is 2 as expected   
unlock X |  | 2 |   
| unlock X | 2 |   
  
The advantage of R/W locking is that when only reads are being performed, they are not blocked. Blocking only occurs when a write is being performed. In addition, as of [Hassayampa (Version 3.0)] the `gl_get()` and `gl_set()` routines automatically implement the appropriate locking mechanism for the type of run being performed. In the case of single threaded simulation, no locking is performed. For multithreaded simulations, r/w locking is used for all memory access between objects. 

Note
    As of [Hassayampa (Version 3.0)] `lock()` is implemented as `wlock()`.

## Examples

The following examples illustrate good coding practice when using locks. 

Coherence locks
    Be sure to operate on data that needs to remain coherent using a single lock instead of multiple locks. For example, you should use
    
    
    READLOCK(x_lock);
    complex t[] = {x[0], x[1] x[2]};
    UNLOCK(x_lock);
    

    rather than using three separately locked data copy operations.

Calculation locks
    Avoid lengthy calculations while using locks. For example, you should use
    
    
    READLOCK(x_lock);
    complex t = A*x;
    UNLOCK(x_key);
    WRITELOCK(y_lock);
    y = t;
    UNLOCK(y_lock);
    

    rather than embedding the calculation inside the safe code region.

Nested locks
    Although you should avoid nested lock because of possible race conditions, if you must use a nested lock try to put the write lock outside the read lock. For example, you should use
    
    
    WRITELOCK(y_lock);
    READLOCK(x_lock);
    x = A*x + B*y;
    UNLOCK(x_lock);
    UNLOCK(y_lock);
    

    rather than taking the read lock out first because write locks can take much longer to obtain than read locks.

## See also

  * [Multithreading]

