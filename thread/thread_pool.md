# JDK线程池

## ThreadPoolExecutor
默认的线程池实现类
### 重要属性说明
#### ctl
``` java
/**
 * is an atomic integer packing two conceptual fields
 * workerCount, indicating the effective number of threads
 * runState,    indicating whether running, shutting down etc
 */
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
```
根据注释，这个变量里面保存了两个信息，有效线程数（workerCount，保存在前3位）、运行状态（runState，保存在后29位）
##### ctl位操作相关属性和方法
Java中int类型占用4字节，也就是32位（第1个是符号位）
``` java
private static final int COUNT_BITS = Integer.SIZE - 3; // =29
private static final int CAPACITY   = (1 << COUNT_BITS) - 1; // 最大线程数 00011111111111111111111111111111

// runState is stored in the high-order bits (线程池的运行状态，保存在高3位)
private static final int RUNNING    = -1 << COUNT_BITS; // 高3位=111
private static final int SHUTDOWN   =  0 << COUNT_BITS; // 高3位=000
private static final int STOP       =  1 << COUNT_BITS; // 高3位=001
private static final int TIDYING    =  2 << COUNT_BITS; // 高3位=010
private static final int TERMINATED =  3 << COUNT_BITS; // 高3位=011

// Packing and unpacking ctl （封装ctl的位操作方法）
private static int runStateOf(int c)     { return c & ~CAPACITY; } // 获得线程池状态
private static int workerCountOf(int c)  { return c & CAPACITY; } // 获得有效线程数
private static int ctlOf(int rs, int wc) { return rs | wc; } // 合并线程池状态和有效线程数
```











