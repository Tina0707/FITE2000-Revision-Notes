# Lecture 10: Lists, Stack, Queue & Deque

## 1. ArrayList

- Wraps a plain Java array (`Object[]`), auto-resizes
- Elements in **contiguous memory**
- **Implements**: `Collection`, `List`

| Operation | Big-O | Why |
|---|---|---|
| `get(index)` | **O(1)** | Direct address calculation |
| `set(index, value)` | **O(1)** | Direct address calculation |
| `add(element)` (end) | **O(1)** | Fill next empty slot |
| `add(index, element)` (middle) | **O(n)** | Shift elements right |
| `remove(index)` | **O(n)** | Shift elements left |
| `indexOf(o)` / `contains(o)` | **O(n)** | Linear scan |

## 2. LinkedList

- Each element in a **node** with `prev` and `next` pointers
- Nodes are **not contiguous** in memory
- **Implements**: `Collection`, `List`, `Deque`
- Direct access to both ends (`first`, `last`)

| Operation | Big-O | Why |
|---|---|---|
| `get(index)` / `set(index)` | **O(n)** | Must walk node-by-node |
| `addFirst(e)` / `addLast(e)` | **O(1)** | Rewire pointers at ends |
| `removeFirst()` / `removeLast()` | **O(1)** | Rewire pointers at ends |
| `add(index, e)` | **O(n)** | Walk O(n) + rewire O(1) |
| `add` during `ListIterator` | **O(1)** total | Iterator holds position |
| `indexOf(o)` / `contains(o)` | **O(n)** | Must visit every node |

### ArrayList vs LinkedList

| Scenario | Choose |
|---|---|
| Frequent index access | **ArrayList** |
| Mostly appending to end | **ArrayList** |
| Memory efficiency matters | **ArrayList** |
| Frequent insert/remove at front | **LinkedList** |
| Implementing queue/deque | **LinkedList** |

## 3. Stack (LIFO — Last In, First Out)

- Access restricted to **one end** (the **top**)
- Common operations: `push`, `pop`, `peek`

| Operation | Big-O | Description |
|---|---|---|
| `push(e)` | **O(1)** | Add to top |
| `pop()` | **O(1)** | Remove from top |
| `peek()` | **O(1)** | View top without removing |

**Exercise: Balanced Brackets**
```java
// Check if brackets are balanced
// e.g., [(())] ✅, [(]) ❌
// Idea: push opening brackets, pop and match on closing
```

## 4. Queue (FIFO — First In, First Out)

- Insert at **back**, remove from **front**
- Common operations: `offer`, `poll`, `peek`

```java
Queue<String> queue = new LinkedList<>();
queue.offer("Alice");     // enqueue
queue.offer("Bob");
String next = queue.peek();  // view front
String served = queue.poll(); // dequeue
```

| Operation | Big-O | Description |
|---|---|---|
| `offer(e)` | **O(1)** | Insert at back |
| `poll()` | **O(1)** | Remove from front |
| `peek()` | **O(1)** | View front element |

## 5. Deque (Double-Ended Queue)

- Insert/remove at **both** ends
- Can behave as **stack** or **queue**
- `LinkedList` implements `Deque`

```
         Front          Back
           ↓             ↓
    ←─ [  |  |  |  |  |  ] ─→
       remove/add    remove/add
```

**Exercise: Palindrome Checker**
```java
// Check if char array is palindrome
// [m, a, d, a, m] → true
// [h, e, l, l, o] → false
// Idea: compare chars from front and back
```

## 6. Big-O Summary

| DS | push/add front | add back | insert middle | remove front | remove back | get by index | contains |
|---|---|---|---|---|---|---|---|
| **ArrayList** | O(n) | O(1) | O(n) | O(n) | O(1) | **O(1)** | O(n) |
| **LinkedList** | **O(1)** | **O(1)** | O(n) | **O(1)** | **O(1)** | O(n) | O(n) |
| **Stack** | — | O(1)* | — | O(1)* | — | — | — |
| **Queue** | — | O(1)† | — | O(1)† | — | — | — |

*Stack: `push` = O(1), `pop` = O(1) at top  
†Queue: `offer` = O(1) at back, `poll` = O(1) at front
