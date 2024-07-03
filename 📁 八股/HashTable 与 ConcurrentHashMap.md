>HashTable 和 ConcurrentHashMap 都是**线程安全的 Map 实现类**

但它们在一些方面存在区别：

1. 线程安全实现方式：
	• `HashTable`： **使用 synchronized 关键字对整个方法进行加锁**，保证线程安全。这意味着在多线程环境下，对 HashTable 的并发访问会导致线程阻塞，性能可能会受到影响。
	• `ConcurrentHashMap`：采用了**更细粒度的锁机制**，将数据分成多个段（Segment），每个段独立加锁。这样可以在多线程并发访问时，提高并发性能，减少线程阻塞。
2. 性能：
	• 由于 ConcurrentHashMap 的锁粒度更细，在多线程环境下的并发性能通常比 HashTable 更好。
	• 此外，ConcurrentHashMap 还支持一些高级的并发操作，如并发迭代和并发更新，进一步提高了并发性能。

3. 迭代器：
	• HashTable 的迭代器是不支持并发修改的，如果在迭代过程中对 HashTable 进行修改，会抛出 ConcurrentModificationException 异常。
	• ConcurrentHashMap 的迭代器是弱一致的，它不会抛出异常，但在迭代过程中可能会反映出其他线程对 ConcurrentHashMap 的修改

4. 数据结构：
	• HashTable 和 ConcurrentHashMap 内部都使用哈希表来存储数据，但具体的数据结构和实现细节可能会有所不同。

## 使用场景
在实际应用中，选择使用 HashTable 还是 ConcurrentHashMap 取决于具体的需求和场景。
- 如果对线程安全要求较高，并且不需要高性能的并发操作，可以选择使用 HashTable。
- 如果需要在多线程环境下具有更好的并发性能和更灵活的并发操作支持，可以选择使用 ConcurrentHashMap。