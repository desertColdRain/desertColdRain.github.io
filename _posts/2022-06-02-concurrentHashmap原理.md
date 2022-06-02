---
layout: post
titile: concurrentHashmap原理分析
tags: ConcurrentHashMap
---
# concurrentHashmap原理分析
## 1. 重要属性和内部类

```Java
// 默认为 0
// 当初始化时, 为 -1
// 当扩容时, 为 -(1 + 扩容线程数)
// 当初始化或扩容完成后，为 下一次的扩容的阈值大小
private transient volatile int sizeCtl;
// 整个 ConcurrentHashMap 就是一个 Node[],内部类，里面有key，value和next Node
static class Node<K,V> implements Map.Entry<K,V> {}
// hash 表
transient volatile Node<K,V>[] table;
// 扩容时的 新 hash 表
private transient volatile Node<K,V>[] nextTable;
// 扩容时如果某个 bin 迁移完毕, 用 ForwardingNode 作为旧 table bin 的头结点
static final class ForwardingNode<K,V> extends Node<K,V> {}
// 用在 compute 以及 computeIfAbsent 时, 用来占位, 计算完成后替换为普通 Node
static final class ReservationNode<K,V> extends Node<K,V> {}
// 作为 treebin 的头节点, 存储 root 和 first
static final class TreeBin<K,V> extends Node<K,V> {}
// 作为 treebin 的节点, 存储 parent, left, right
static final class TreeNode<K,V> extends Node<K,V> {}
```

## 2. 重要方法

```Java
// 获取 Node[] 中第 i 个 Node
static final <K,V> Node<K,V> tabAt(Node<K,V>[] tab, int i)

// cas 修改 Node[] 中第 i 个 Node 的值, c 为旧值, v 为新值
static final <K,V> boolean casTabAt(Node<K,V>[] tab, int i, Node<K,V> c, Node<K,V> v)

// 直接修改 Node[] 中第 i 个 Node 的值, v 为新值
static final <K,V> void setTabAt(Node<K,V>[] tab, int i, Node<K,V> v)
```

## 3. 构造器分析

可以看到实现了懒惰初始化，在构造方法中仅仅计算了 table 的大小，以后在第一次使用时才会真正创建

```Java
public ConcurrentHashMap(int initialCapacity, float loadFactor, int concurrencyLevel) {
        if (!(loadFactor > 0.0f) || initialCapacity &lt; 0 || concurrencyLevel &lt; = 0)
                throw new IllegalArgumentException();
        if (initialCapacity &lt; concurrencyLevel) // Use at least as many bins
                initialCapacity = concurrencyLevel; // as estimated threads
        long size = (long)(1.0 + (long)initialCapacity / loadFactor);
        // tableSizeFor 仍然是保证计算的大小是 2^n, 即 16,32,64 ...
        int cap = (size >= (long)MAXIMUM_CAPACITY) ?
                  MAXIMUM_CAPACITY : tableSizeFor((int)size);
        this.sizeCtl = cap;
}
```

## 4. get方法

首先计算key的hashcode，找到hashcode所在的Node&lt;k,V>数组的下标，如果数组下标所在的节点链表的头节点hashcode相等，利用equals方法比较是否相等，相等则直接返回
如果hash 为负数表示该 bin 在扩容中或是 treebin, 这时调用 find 方法来查找
如果以上方法没找到，就直接遍历链表，利用equals比较找到最终的值

```Java
public V get(Object key) {
        Node&lt;K,V>\[] tab; Node&lt;K,V> e, p; int n, eh; K ek;
        // spread 方法能确保返回结果是正数
        int h = spread(key.hashCode());
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (e = tabAt(tab, (n - 1) & h)) != null) {
                // 如果头结点已经是要查找的 key
                if ((eh = e.hash) == h) {
                        if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                                return e.val;
                }
                // hash 为负数表示该 bin 在扩容中或是 treebin, 这时调用 find 方法来查找
                else if (eh &lt; 0)
                        return (p = e.find(h, key)) != null ? p.val : null;
                // 正常遍历链表, 用 equals 比较
                while ((e = e.next) != null) {
                        if (e.hash == h &&
                            ((ek = e.key) == key || (ek != null && key.equals(ek))))
                                return e.val;
                }
        }
        return null;
}
```
  ## 5. put流程
```Java
public V put(K key, V value) {
        return putVal(key, value, false);
}
final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        // 其中 spread 方法会综合高位低位, 具有更好的 hash 性
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
                // f 是链表头节点
                // fh 是链表头结点的 hash
                // i 是链表在 table 中的下标
                Node<K,V> f; int n, i, fh;
                // 要创建 table
                if (tab == null || (n = tab.length) == 0)
                        // 初始化 table 使用了 cas, 无需 synchronized 创建成功, 进入下一轮循环
                        tab = initTable();
                // 要创建链表头节点
                else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                        // 添加链表头使用了 cas, 无需 synchronized
                        if (casTabAt(tab, i, null,
                                     new Node<K,V>(hash, key, value, null)))
                                break;
                }
                // 帮忙扩容
                else if ((fh = f.hash) == MOVED)
                        // 帮忙之后, 进入下一轮循环
                        tab = helpTransfer(tab, f);
                else {
                        V oldVal = null;
                        // 锁住链表头节点
                        synchronized (f) {
                                // 再次确认链表头节点没有被移动
                                if (tabAt(tab, i) == f) {
                                        // 链表
                                        if (fh >= 0) {
                                                binCount = 1;
                                                // 遍历链表
                                                for (Node<K,V> e = f;; ++binCount) {
                                                        K ek;
                                                        // 找到相同的 key
                                                        if (e.hash == hash &&
                                                            ((ek = e.key) == key ||
                                                             (ek != null && key.equals(ek)))) {
                                                                oldVal = e.val;
                                                                // 更新
                                                                if (!onlyIfAbsent)
                                                                        e.val = value;
                                                                break;
                                                        }
                                                        Node<K,V> pred = e;
                                                        // 已经是最后的节点了, 新增 Node, 追加至链表尾
                                                        if ((e = e.next) == null) {
                                                                pred.next = new Node<K,V>(hash, key,
                                                                                          value, null);
                                                                break;
                                                        }
                                                }
                                        }
                                        // 红黑树
                                        else if (f instanceof TreeBin) {
                                                Node<K,V> p;
                                                binCount = 2;
                                                // putTreeVal 会看 key 是否已经在树中, 是, 则返回对应的 TreeNode
                                                if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                                                      value)) != null) {
                                                        oldVal = p.val;
                                                        if (!onlyIfAbsent)
                                                                p.val = value;
                                                }
                                        }
                                }
                                // 释放链表头节点的锁
                        }

                        if (binCount != 0) {
                                if (binCount >= TREEIFY_THRESHOLD)
                                        // 如果链表长度 >= 树化阈值(8), 进行链表转为红黑树
                                        treeifyBin(tab, i);
                                if (oldVal != null)
                                        return oldVal;
                                break;
                        }
                }
        }
        // 增加 size 计数
        addCount(1L, binCount);
        return null;
}
private final Node<K,V>[] initTable() {
        Node<K,V>[] tab; int sc;
        while ((tab = table) == null || tab.length == 0) {
                if ((sc = sizeCtl) < 0)
                        Thread.yield();
                // 尝试将 sizeCtl 设置为 -1（表示初始化 table）
                else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
                        // 获得锁, 创建 table, 这时其它线程会在 while() 循环中 yield 直至 table 创建
                        try {
                                if ((tab = table) == null || tab.length == 0) {
                                        int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                                        Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                                        table = tab = nt;
                                        sc = n - (n >>> 2);
                                }
                        } finally {
                                sizeCtl = sc;
                        }
                        break;
                }
        }
        return tab;
}
// check 是之前 binCount 的个数
// check 是之前 binCount 的个数
private final void addCount(long x, int check) {
        CounterCell[] as; long b, s;
        if (
                // 已经有了 counterCells, 向 cell 累加
                (as = counterCells) != null ||
                // 还没有, 向 baseCount 累加
                !U.compareAndSwapLong(this, BASECOUNT, b = baseCount, s = b + x)
                ) {
                CounterCell a; long v; int m;
                boolean uncontended = true;
                if (
                        // 还没有 counterCells
                        as == null || (m = as.length - 1) < 0 ||
                        // 还没有 cell
                        (a = as[ThreadLocalRandom.getProbe() & m]) == null ||
                        // cell cas 增加计数失败
                        !(uncontended = U.compareAndSwapLong(a, CELLVALUE, v = a.value, v + x))
                        ) {
                        // 创建累加单元数组和cell, 累加重试
                        fullAddCount(x, uncontended);
                        return;
                }
                if (check <= 1)
                        return;
                // 获取元素个数
                s = sumCount();
        }
        if (check >= 0) {
                Node<K,V>[] tab, nt; int n, sc;
                while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&
                       (n = tab.length) < MAXIMUM_CAPACITY) {
                        int rs = resizeStamp(n);
                        if (sc < 0) {
                                if ((sc >>> RESIZE_STAMP_SHIFT) != rs || sc == rs + 1 ||
                                    sc == rs + MAX_RESIZERS || (nt = nextTable) == null ||
                                    transferIndex <= 0)
                                        break;
                                // newtable 已经创建了，帮忙扩容
                                if (U.compareAndSwapInt(this, SIZECTL, sc, sc + 1))
                                        transfer(tab, nt);
                        }
                        // 需要扩容，这时 newtable 未创建
                        else if (U.compareAndSwapInt(this, SIZECTL, sc,
                                                     (rs << RESIZE_STAMP_SHIFT) + 2))
                                transfer(tab, null);
                        s = sumCount();
                }
        }
}

```

size 计算流程
size 计算实际发生在 put，remove 改变集合元素的操作之中
没有竞争发生，向 baseCount 累加计数
有竞争发生，新建 counterCells，向其中的一个 cell 累加计数
counterCells 初始有两个 cell
如果计数竞争比较激烈，会创建新的 cell 来累加计数
Java 8 数组（Node） +（ 链表 Node | 红黑树 TreeNode ） 以下数组简称（table），链表简称（bin）
初始化，使用 cas 来保证并发安全，懒惰初始化 table
树化，当 table.length &lt; 64 时，先尝试扩容，超过 64 时，并且 bin.length > 8 时，会将链表树化，树化过程
会用 synchronized 锁住链表头
put，如果该 bin 尚未创建，只需要使用 cas 创建 bin；如果已经有了，锁住链表头进行后续 put 操作，元素
添加至 bin 的尾部
get，无锁操作仅需要保证可见性，扩容过程中 get 操作拿到的是 ForwardingNode 它会让 get 操作在新
table 进行搜索
扩容，扩容时以 bin 为单位进行，需要对 bin 进行 synchronized，但这时妙的是其它竞争线程也不是无事可
做，它们会帮助把其它 bin 进行扩容，扩容时平均只有 1/6 的节点会把复制到新 table 中
size，元素个数保存在 baseCount 中，并发时的个数变动保存在 CounterCell\[] 当中。最后统计数量时累加
即可
