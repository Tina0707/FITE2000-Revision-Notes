# Lecture 10: Big-O & Performance Analysis

## 1. Why Different Data Structures?

Different structures have different **behavior** and **performance**:
- Keep insertion order? Allow duplicates? Stay sorted?
- How fast is add / search / remove / access by index?

## 2. Performance Measurement

| Approach | Method | Problem |
|---|---|---|
| **Empirical** | Write code, time it | Noise (language, OS, machine, input) |
| **Analytical** | Count operations, derive function | Clean, abstract |

## 3. Big-O Notation

**Definition**: f(n) is O(g(n)) if there exists constant C such that **f(n) ≤ C·g(n)** for all n ≥ K.

"f grows **no faster than** g, up to a constant factor, for large n."

### Common Orders (fastest → slowest)

| Big-O | Name | Sample Functions |
|---|---|---|
| **O(1)** | Constant | `1`, `3000`, `sin(n)+100` |
| **O(log n)** | Logarithmic | `log₂n`, `1000·log₃n` |
| **O(n)** | Linear | `10n`, `n/1000000 + 500log₂n` |
| **O(n log n)** | Linearithmic | `n·log₂n`, `log₂n·n` |
| **O(n²)** | Quadratic | `100n² + 100n`, `n(n+1)/2` |
| **O(2ⁿ)** | Exponential | `2ⁿ + 500n¹⁰⁰` |

## 4. Examples by Category

### O(1) — Constant
```java
int getFirst(int[] arr) { return arr[0]; }
int getValue(HashMap<String, Integer> map) { return map.get("Alice"); }
```

### O(log n) — Logarithmic (Binary Search)
```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

### O(n) — Linear
```java
int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

### O(n²) — Quadratic (Bubble Sort)
```java
void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

## 5. Quick Reference

| Operation | ArrayList | LinkedList | HashSet | TreeSet |
|---|---|---|---|---|
| `get(i)` / `set(i)` | **O(1)** | O(n) | — | — |
| `add` (end) | **O(1)** | O(1) | **O(1)** | O(log n) |
| `add` (middle) | O(n) | O(n) | — | — |
| `addFirst` / `addLast` | — | **O(1)** | — | — |
| `remove` (middle) | O(n) | O(n) | — | — |
| `contains` | O(n) | O(n) | **O(1)** | **O(log n)** |
| `indexOf` | O(n) | O(n) | — | — |

## 6. Key Insight

When n is **large enough**, constants don't matter — focus on **growth rate**.
All basic operations can be treated as taking equal time for Big-O analysis.

> **🔗 各数据结构的详细讲解与代码示例：**
> - `ArrayList` vs `LinkedList`、`Stack`、`Queue`、`Deque` → **Lecture 11**
> - `HashSet`、`TreeSet` → **Lecture 12**
> - `HashMap`、`TreeMap` → **Lecture 13**
