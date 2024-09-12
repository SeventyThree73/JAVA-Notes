在 Java 集合框架中，**快速失败（fail-fast）**和**安全失败（fail-safe）**是两种迭代器行为，它们描述了在迭代过程中，如果集合发生结构性修改（如插入、删除、或修改元素），迭代器如何响应。

### 1. **快速失败（fail-fast）**

**快速失败**机制指的是，当通过一个集合的迭代器进行迭代时，如果在迭代过程中集合发生了结构性修改（即除了通过迭代器自身的 `remove()` 方法外的任何增删操作），则迭代器会立即抛出 **`ConcurrentModificationException`** 异常。

#### 1.1 **原理**
快速失败机制的实现依赖于集合中的**modCount**字段（Modification Count）。`modCount` 记录集合的结构性修改次数。每当集合发生结构性修改时（如增删元素），`modCount` 的值会增加。

- 当迭代器被创建时，它会保存集合当前的 `modCount` 值。
- 在迭代过程中，迭代器会不断检查 `modCount` 是否发生变化。
- 如果在迭代过程中，`modCount` 被其他线程或代码修改了，而迭代器没有感知到这次修改，迭代器就会抛出 `ConcurrentModificationException`，通知开发者有并发修改的情况发生。

#### 1.2 **示例代码**

```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");

Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String fruit = iterator.next();
    if (fruit.equals("Banana")) {
        list.remove(fruit);  // 会抛出 ConcurrentModificationException
    }
}
```

在这个例子中，当你通过 `list.remove(fruit)` 修改了集合时，迭代器会抛出 `ConcurrentModificationException`，因为这次修改并不是通过迭代器进行的。

#### 1.3 **快速失败的特点**
- **并发修改敏感**：在检测到集合被修改后会立即抛出异常。
- **不保证行为**：快速失败的机制不是完全可靠的。特别是在多线程环境中，它只能尽力检测到并发修改，因此不能作为并发安全的解决方案。

#### 1.4 **常见的快速失败集合**
- `ArrayList`
- `HashMap`
- `LinkedList`
- `HashSet`

这些集合在迭代时是**快速失败**的，如果在迭代过程中有结构性修改，会抛出异常。

### 2. **安全失败（fail-safe）**

**安全失败**机制指的是，当在遍历集合时，如果集合发生了结构性修改，迭代器**不会抛出异常**，它能够**安全地继续遍历**。通常，安全失败的迭代器会在遍历时**使用副本**（即拷贝原集合的数据），因此在遍历过程中即便原集合发生了修改，也不会影响到迭代器的遍历过程。

#### 2.1 **原理**
安全失败机制的迭代器在遍历集合时，通常会创建原集合的**副本**。因此，迭代器实际上遍历的是集合的一个副本，而不是原集合本身。这意味着，即使在遍历期间集合发生了结构性修改，迭代器也不会察觉到，因为它遍历的不是原始集合。

由于迭代器访问的是副本，修改原集合不会影响到迭代器的行为，因此不会抛出异常。

#### 2.2 **示例代码**

```java
ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>();
map.put("A", "Apple");
map.put("B", "Banana");

Iterator<String> iterator = map.keySet().iterator();
while (iterator.hasNext()) {
    String key = iterator.next();
    map.put("C", "Cherry");  // 可以安全修改，不会抛出异常
}
```

在这个例子中，`ConcurrentHashMap` 是一个**安全失败**的集合，因此即使在迭代过程中修改集合，也不会抛出 `ConcurrentModificationException`。

#### 2.3 **安全失败的特点**
- **结构性修改安全**：集合可以在遍历期间被修改，迭代器依然可以继续遍历。
- **性能开销**：由于创建了集合的副本，安全失败迭代器的性能开销较大，尤其是当集合较大时，副本的创建可能带来额外的内存开销。
- **多线程环境支持**：安全失败机制通常用于并发集合，支持多线程环境。

#### 2.4 **常见的安全失败集合**
- `ConcurrentHashMap`
- `CopyOnWriteArrayList`

这些集合在迭代时是**安全失败**的，它们通常用于多线程环境，允许在遍历时进行修改而不会抛出异常。

### 3. **快速失败 vs 安全失败**

| **特点**               | **快速失败（Fail-Fast）**                                  | **安全失败（Fail-Safe）**                                 |
|------------------------|------------------------------------------------------------|-----------------------------------------------------------|
| **原理**               | 直接遍历原集合，修改时检查 `modCount` 是否发生变化。        | 通过遍历集合的副本，修改不会影响迭代过程。                |
| **并发修改**           | 并发修改时会抛出 `ConcurrentModificationException`。         | 并发修改时不会抛出异常。                                   |
| **实现方式**           | 使用 `modCount` 检测集合结构性修改。                       | 通过副本遍历集合，原集合的修改不会影响副本。               |
| **性能**               | 性能较高，没有副本创建的开销。                              | 创建副本带来额外的内存和时间开销。                         |
| **适用场景**           | 单线程或少量并发修改的场景。                               | 多线程环境下，允许并发修改的场景。                         |
| **常见集合**           | `ArrayList`、`HashSet`、`HashMap` 等。                      | `ConcurrentHashMap`、`CopyOnWriteArrayList` 等并发集合。   |

### 4. **总结**

- **快速失败**（fail-fast）集合在检测到结构性修改时会立即抛出 `ConcurrentModificationException`，这是为避免并发修改带来不一致性和数据错误的保护机制。
- **安全失败**（fail-safe）集合通过在迭代时使用集合的副本来实现对并发修改的容忍，即使在遍历时集合发生变化，迭代器也不会受到影响。

在多线程编程中，**安全失败**机制通常用于并发集合（如 `ConcurrentHashMap`），而在单线程或轻量并发修改场景中，**快速失败**机制更为常见（如 `ArrayList` 和 `HashMap` 等非并发集合）。