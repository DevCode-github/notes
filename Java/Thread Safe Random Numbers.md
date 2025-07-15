### ThreadLocal
Creates a variable unique to each thread, each thread has its own isolated copy of value.

#### Example
```
ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

Runnable task = () -> {
    threadLocal.set((int) (Math.random() * 100));
    System.out.println(Thread.currentThread().getName() + " → " + threadLocal.get());
};

new Thread(task).start();
new Thread(task).start();
```
Each thread will get a different Random number which is independent of other threads

---

### ThreadLocalRandom
A special `Random` class optimized for **multi-threaded performance** — uses internal `ThreadLocal` for each thread to avoid contention.
#### Example
```
import java.util.concurrent.ThreadLocalRandom;

Runnable task = () -> {
    int num = ThreadLocalRandom.current().nextInt(1, 100);
    System.out.println(Thread.currentThread().getName() + " → " + num);
};

new Thread(task).start();
new Thread(task).start();
```
Each thread calls `ThreadLocalRandom.current()` and gets **its own thread-safe generator**.


#### Differences
- `ThreadLocal` is thread-specific but `ThreadLocalRandom` offers a high concurrency with no bottlenecks  and contention for random numbers.
