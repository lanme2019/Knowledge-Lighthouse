# ReentrantLock

- 默认使用非公平锁 可重入


```java
public class ReentrantLock implements Lock, java.io.Serializable {
           public ReentrantLock() {
               sync = new NonfairSync();
           }
           static final class NonfairSync extends Sync {
                    private static final long serialVersionUID = 7316153563782823691L;
            
                    /**
                     * Performs lock.  Try immediate barge, backing up to normal
                     * acquire on failure.
                     */
                    final void lock() {
                        if (compareAndSetState(0, 1))
                            setExclusiveOwnerThread(Thread.currentThread());
                        else
                            acquire(1);
                    }
            
                    protected final boolean tryAcquire(int acquires) {
                        return nonfairTryAcquire(acquires);
                    }
                }

    
            final boolean nonfairTryAcquire(int acquires) {
            final Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
}

```


# ReentrantReadWriteLock

  
关键点
- 默认非公平锁 可重入（支持锁降级 写锁中可获取读锁）
- 读锁与写锁分离
- 读锁实现了AQS的共享锁，写锁实现了AQS独占锁
- 写锁只有等所有读锁释放完毕才可以获取
- state的高16位代表读锁的状态值，低16位代表写锁的值
- 

```java
public class ReentrantReadWriteLock
        implements ReadWriteLock, java.io.Serializable {
    public ReentrantReadWriteLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
        readerLock = new ReadLock(this);
        writerLock = new WriteLock(this);
    }
    
            protected final int tryAcquireShared(int unused) {
                Thread current = Thread.currentThread();
                int c = getState();
                if (exclusiveCount(c) != 0 &&
                    getExclusiveOwnerThread() != current)
                    return -1;
                int r = sharedCount(c);
                if (!readerShouldBlock() &&
                    r < MAX_COUNT &&
                    compareAndSetState(c, c + SHARED_UNIT)) {
                    if (r == 0) {
                        firstReader = current;
                        firstReaderHoldCount = 1;
                    } else if (firstReader == current) {
                        firstReaderHoldCount++;
                    } else {
                        HoldCounter rh = cachedHoldCounter;
                        if (rh == null || rh.tid != getThreadId(current))
                            cachedHoldCounter = rh = readHolds.get();
                        else if (rh.count == 0)
                            readHolds.set(rh);
                        rh.count++;
                    }
                    return 1;
                }
                return fullTryAcquireShared(current);
            }

}
```
# StampedLock

内置三种锁

- 写锁 类似ReentrantLock 不支持重入
- 悲观读 类似读写锁 不支持冲入
- 乐观读锁 不加锁只判断版本号

