# Lecture 13: Maps

## 1. What is a Map?

Stores **key–value pairs** — like a dictionary:
- **Key**: unique identifier (no duplicates)
- **Value**: data associated with the key (can be duplicated)

| Example | Key | Value |
|---|---|---|
| Student directory | student ID | student name |
| Word frequency | word | count |
| Product catalog | product code | price |
| Calendar | date | event |

## 2. HashMap

- Backed by a **hash table** (same structure as `HashSet`)
- Relies on **key's** `hashCode()` and `equals()`
- **O(1)** for basic operations

### Structure
```
Internal array:
[0] → Node<K,V> → Node<K,V>  (chain of entries with same bucket)
[1] → Node<K,V>
[2] → Node<K,V>
```

Each node stores: `<key, value>` pair, hash, and pointer to next node.

### Key Operations
| Operation | Big-O | Description |
|---|---|---|
| `put(key, value)` | **O(1)** | Add pair OR update existing key's value |
| `get(key)` | **O(1)** | Retrieve value by key (returns `null` if absent) |
| `remove(key)` | **O(1)** | Remove pair by key |
| `containsKey(key)` | **O(1)** | Check if key exists |
| `size()` | **O(1)** | Number of entries |

### Example: Map<Integer, String>
```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "Tom");
map.put(2, "Alice");
System.out.println(map.get(1));  // "Tom"
```

⚠️ **Immutability note**: Map stores reference to value. If value object is mutable, changes reflect in map:
```java
Map<Integer, Student> map = new HashMap<>();
map.put(1, s1);
s1.name = "Jack";    // ✅ map.get(1).name is now "Jack"
```

But with `String` (immutable):
```java
Map<Integer, String> map = new HashMap<>();
map.put(1, s1.name);
s1.name = "Jack";    // ❌ map still has "Tom" (String reference changed)
```

## 3. TreeMap

- Backed by a **self-balancing BST** (same as `TreeSet`)
- Keys are **always sorted** (natural order or by `Comparator`)
- **O(log n)** for basic operations

| Operation | Big-O |
|---|---|
| `put(key, value)` | **O(log n)** |
| `get(key)` | **O(log n)** |
| `remove(key)` | **O(log n)** |
| `containsKey(key)` | **O(log n)** |

### Use When:
- Sorted keys are required
- Range queries are useful
- Ordered traversal is needed

## 4. Iterating Through Maps

Maps are **not iterable** directly — use special methods:

### 1. Iterate keys
```java
for (Integer id : map.keySet()) {
    System.out.println(id);
}
```

### 2. Iterate values
```java
for (String name : map.values()) {
    System.out.println(name);
}
```

### 3. Iterate key-value pairs (Map.Entry)
```java
for (Map.Entry<Integer, String> e : map.entrySet()) {
    System.out.println(e.getKey() + " -> " + e.getValue());
}
```

`Map.Entry<K,V>` is a **nested interface** inside `Map`:
```java
public interface Map<K, V> {
    interface Entry<K, V> {
        K getKey();
        V getValue();
        V setValue(V value);
    }
}
```

## 5. HashMap vs TreeMap

| Feature | HashMap | TreeMap |
|---|---|---|
| **Order** | None | Sorted by key |
| **Performance** | **O(1)** | **O(log n)** |
| **Structure** | Hash table | Self-balancing BST |
| **Key requirement** | `hashCode()` + `equals()` | `compareTo()` or `Comparator` |
| **Use when** | Speed is main concern | Keys must stay sorted |

## 6. Map vs Set

| Set | Map |
|---|---|
| Stores **elements only** | Stores **key–value pairs** |
| Useful for **uniqueness** | Useful for **lookup by key** |

## 7. Example: Word Frequency Counter
```java
// Count word frequencies, then sort by frequency (descending)
Map<String, Integer> freq = new HashMap<>();
for (String word : words) {
    freq.put(word, freq.getOrDefault(word, 0) + 1);
}
```
