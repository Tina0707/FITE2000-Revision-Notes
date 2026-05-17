# Lecture 11: Sets

## 1. HashSet

- **No duplicates** — contains at most one of each element
- **No order** — iteration order appears random
- Backed by a **hash table** (internal array of buckets)

### How It Works
```java
element → hashCode() → bucket index (hashCode % array.length)
```

```
Internal array:
[0] → Node A → Node C  (chain of elements with same bucket)
[1] → Node B
[2] → Node D
```

### Key Operations
| Operation | Big-O | How |
|---|---|---|
| `add(e)` | **O(1)** | Hash → find bucket → scan chain → insert if absent |
| `remove(e)` | **O(1)** | Hash → find bucket → scan chain → unlink |
| `contains(e)` | **O(1)** | Hash → find bucket → scan chain |
| `size()` | **O(1)** | Tracked count |

### Why O(1)?
- Good hash function spreads elements **evenly** across buckets
- Java auto-resizes to keep chain length ~1
- Each chain has roughly `n / b` elements (≈ constant)

### Example: Count Duplicates
```java
int[] numbers = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
Set<Integer> set = new HashSet<>();
int duplicates = 0;
for (int n : numbers) {
    if (!set.add(n)) {
        duplicates++;       // n was already in set
    }
}
System.out.println("Unique: " + set);    // [1, 2, 3, 4, 5, 6, 9]
System.out.println("Duplicates: " + duplicates);  // 3
```

### Dependencies
- `hashCode()` — determines bucket placement
- `equals()` — determines element equality within same bucket

## 2. TreeSet

- **No duplicates**
- **Always sorted** — natural order (via `Comparable`) or custom (`Comparator`)
- Backed by a **self-balancing Binary Search Tree (BST)**

### Binary Search Tree
- Each node ≤ 2 children
- Left subtree: **smaller** elements
- Right subtree: **larger** elements

```
        4
      /   \
     2     6
    / \   / \
   1   3 5   7
```

- **Search**: start at root, go left/right at each step
- **Height** of balanced BST = O(log n)

### Key Operations
| Operation | Big-O | How |
|---|---|---|
| `add(e)` | **O(log n)** | Walk root→leaf, insert, rebalance |
| `remove(e)` | **O(log n)** | Walk to target, unlink, rebalance |
| `contains(e)` | **O(log n)** | Walk root→leaf following comparisons |
| `size()` | **O(1)** | Tracked count |

### Dependencies
- `compareTo()` (Comparable) **or** `Comparator` — determines ordering and equality

### Example: Student with Comparable
```java
public class Student implements Comparable<Student> {
    String name;
    int age;
    int id;

    @Override
    public int compareTo(Student other) {
        int result = Integer.compare(this.age, other.age);
        if (result != 0) return result;
        result = this.name.compareTo(other.name);
        if (result != 0) return result;
        return Integer.compare(this.id, other.id);
    }
}
```

## 3. HashSet vs TreeSet

| Feature | HashSet | TreeSet |
|---|---|---|
| **Order** | None (random) | Sorted (ascending) |
| **Performance** | **O(1)** for add/remove/contains | **O(log n)** for add/remove/contains |
| **Underlying structure** | Hash table | Self-balancing BST |
| **Element requirement** | `hashCode()` + `equals()` | `compareTo()` or `Comparator` |
| **Null elements** | ✅ Allowed (one null) | ❌ Not allowed (needs comparison) |

## 4. List vs Set

| Use **List** when... | Use **Set** when... |
|---|---|
| Need index-based access | Need to ensure **no duplicates** |
| Insertion order matters | Frequently check **contains()** |
| Duplicates allowed | Don't need index access |
| | Membership testing is priority |
