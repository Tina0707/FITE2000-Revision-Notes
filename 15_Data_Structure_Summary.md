# Data Structure Summary

## List

| 方法 | ArrayList | LinkedList | 说明 |
|---|---|---|---|
| `get(index)` | **O(1)** | **O(n)** | 获取 index 位置的元素 |
| `set(index, e)` | **O(1)** | **O(n)** | 替换并返回**旧元素** |
| `add(e)` (末尾) | **O(1)** | **O(1)** | 追加到末尾 |
| `add(index, e)` (中间) | **O(n)** | **O(n)** | 插入到指定位置 |
| `addFirst(e)` / `addLast(e)` | — | **O(1)** | LinkedList 独有 |
| `remove(index)` | **O(n)** | **O(n)** | 删除并返回该位置元素 |
| `removeFirst()` / `removeLast()` | — | **O(1)** | LinkedList 独有 |
| `indexOf(o)` | **O(n)** | **O(n)** | 返回 index 或 -1 |
| `contains(o)` | **O(n)** | **O(n)** | 返回 boolean |

## Stack (LIFO)

| 方法 | Big-O | 说明 |
|---|---|---|
| `push(e)` | **O(1)** | 入栈（压到栈顶） |
| `pop()` | **O(1)** | 出栈，返回**被删的栈顶元素** |
| `peek()` | **O(1)** | 查看栈顶，不删除 |
| `isEmpty()` | **O(1)** | 是否为空 |

## Queue (FIFO)

| 方法 | Big-O | 说明 |
|---|---|---|
| `offer(e)` | **O(1)** | 入队（加到队尾） |
| `poll()` | **O(1)** | 出队，返回**被删的队首元素** |
| `peek()` | **O(1)** | 查看队首，不删除 |
| `isEmpty()` | **O(1)** | front == back 即为空 |

## Deque

| 方法 | Big-O | 说明 |
|---|---|---|
| `addFirst(e)` / `offerFirst(e)` | **O(1)** | 从队首插入 |
| `addLast(e)` / `offerLast(e)` | **O(1)** | 从队尾插入 |
| `removeFirst()` / `pollFirst()` | **O(1)** | 从队首删除并返回 |
| `removeLast()` / `pollLast()` | **O(1)** | 从队尾删除并返回 |
| `getFirst()` / `peekFirst()` | **O(1)** | 查看队首 |
| `getLast()` / `peekLast()` | **O(1)** | 查看队尾 |

## Set

| 方法 | HashSet | TreeSet | 说明 |
|---|---|---|---|
| `add(e)` | **O(1)** | **O(log n)** | 添加（已存在则不添加） |
| `remove(e)` | **O(1)** | **O(log n)** | 删除元素 |
| `contains(e)` | **O(1)** | **O(log n)** | 是否存在 |

## Map

| 方法 | HashMap | TreeMap | 说明 |
|---|---|---|---|
| `put(key, value)` | **O(1)** | **O(log n)** | 添加或更新，返回**旧值**（无旧值则 null） |
| `get(key)` | **O(1)** | **O(log n)** | 获取 value（无则 null） |
| `remove(key)` | **O(1)** | **O(log n)** | 删除并返回 value |
| `containsKey(key)` | **O(1)** | **O(log n)** | key 是否存在 |
| `getOrDefault(key, default)` | **O(1)** | **O(log n)** | 获取或默认值 |
| `size()` | **O(1)** | **O(1)** | 元素数量 |

### Map 遍历

| 方法 | 返回类型 | 说明 |
|---|---|---|
| `keySet()` | `Set<K>` | 遍历所有 key |
| `values()` | `Collection<V>` | 遍历所有 value（可能重复） |
| `entrySet()` | `Set<Map.Entry<K,V>>` | 遍历 key-value 对 |

## Iterator

| 方法 | Iterator | ListIterator |
|---|---|---|
| `hasNext()` / `next()` | ✅ | ✅ |
| `remove()` | ✅ | ✅ |
| `add(e)` | ❌ | ✅ |
| `previous()` / `hasPrevious()` | ❌ | ✅ |
| `set(e)` | ❌ | ✅ |

## Java API 设计习惯

修改操作通常返回"被改动前"或"被改动掉"的东西：

| 方法 | 返回 | 含义 |
|---|---|---|
| `set(index, e)` | 被替换的旧元素 | "你替换掉的是什么" |
| `remove(index)` | 被删的元素 | "你删掉的是什么" |
| `pop()` | 被删的栈顶 | "你弹出的是什么" |
| `poll()` | 被删的队首 | "你取出的是什么" |
| `put(key, value)` | 被覆盖的旧值 / null | "你覆盖掉的是什么" |
