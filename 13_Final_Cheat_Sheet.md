# Final Exam Cheat Sheet

## Exam Structure

- **Part I: OOP Design (10 pts)** — Class hierarchy design
- **Part II: MCQ (40 pts)** — 20 code-tracing questions (2 pts each, -0.5 penalty)
- **Part III: Short-Answer (50 pts)** — File I/O & exception, debug-and-trace, data processing

---

## 1. OOP Basics

### 4 Features
| Feature | Description |
|---|---|
| **Abstraction** | Essential features only, hide details |
| **Encapsulation** | Wrap data + methods, information hiding |
| **Inheritance** | Subclass acquires superclass properties (IS-A) |
| **Polymorphism** | Many forms — overriding (runtime) + overloading (compile-time) |

### Access Modifiers
```
private < default < protected < public
```
- `private`: same class only
- `default` (none): same package only
- `protected`: package + subclasses
- `public`: everywhere

---

## 2. Classes & Objects

### Class Declaration
```java
public class MyClass {
    // fields (instance variables)
    // constructors
    // methods
}
```

### Object Creation
```java
MyClass obj = new MyClass();  // declare → create → assign
```

### Constructors
- Same name as class, **no return type**
- Default no-arg constructor added only if **no** constructors defined
- `this()` — call another constructor in same class
- `super()` — call superclass constructor (must be **first** statement)

### Method Overloading
- Same name, **different parameters** (number, type, order)
- Return type and access level can differ

### Static
- `static` variable/method → **per-class** (one copy, shared)
- Static methods **cannot** access non-static members
- Access via `ClassName.member`

### this Keyword
```java
this.size = size;    // refers to current instance
```

---

## 3. Inheritance

```java
class Child extends Parent { ... }
```

### Method Overriding
- Same name, same params, compatible return type
- Cannot be **less** accessible
- `super.method()` to call parent's version
- Most specific version wins (lowest in tree)

### Constructor Chaining
- Subclass constructor calls `super()` **implicitly** (first line)
- If superclass has **no no-arg constructor**, must call `super(args)` explicitly
- `this()` and `super()` cannot both appear in same constructor

---

## 4. Polymorphism

### Runtime (Overriding + Upcasting)
```java
Animal a = new Dog();   // upcast (automatic)
a.makeNoise();          // calls Dog's version
```

### Polymorphic Features
| Feature | Example |
|---|---|
| Parameter | `void feed(Animal a)` — accepts Dog, Cat... |
| Return type | `Animal create()` — returns Dog, Cat... |
| Array | `Animal[]` — stores Dog, Cat... |

### Downcasting
```java
if (obj instanceof Dog) {
    Dog d = (Dog) obj;
}
```

### Method Overloading Resolution
Resolved at **compile time** based on **reference type**, not object type:
```java
Animal a = new Dog();
test.process(a);  // calls process(Animal), not process(Dog)
```

---

## 5. Object Class Methods

| Method | Returns | Notes |
|---|---|---|
| `toString()` | String | `System.out.println(obj)` calls this automatically |
| `equals(Object)` | boolean | `==` checks references, `.equals()` checks content |
| `hashCode()` | int | Must override if `equals()` is overridden |
| `getClass()` | Class<?> | Cannot override |

---

## 6. Abstract & Interface

### Abstract Class
```java
public abstract class Animal {
    public abstract void makeNoise();  // no body
    public void sleep() { ... }        // concrete method
}
```
- Cannot be instantiated
- Concrete subclass must implement **all** abstract methods

### Interface
```java
public interface Pet {
    void beFriendly();              // implicitly public abstract
    default void play() { ... }    // Java 8+ default method
    static void info() { ... }     // Java 8+ static method
}
```
- `implements` keyword
- A class can implement **multiple** interfaces
- Can be used as polymorphic type

---

## 7. Exception Handling

### Checked vs Unchecked
| | Checked | Unchecked (RuntimeException) |
|---|---|---|
| Compiler checks? | ✅ Yes | ❌ No |
| Examples | `IOException`, `FileNotFoundException` | `NullPointerException`, `ClassCastException`, `IndexOutOfBoundsException` |

### try-catch-finally
```java
try { risky code }
catch (ExceptionType e) { handle }
finally { always runs }
```

### throws vs throw
- `throws` — declares exception in method signature
- `throw` — actually throws an exception object

### Ducking
Pass exception to caller via `throws` instead of handling.

---

## 8. File I/O

### Serialization
```java
// Write
ObjectOutputStream os = new ObjectOutputStream(new FileOutputStream("file.ser"));
os.writeObject(obj);
os.close();

// Read
ObjectInputStream is = new ObjectInputStream(new FileInputStream("file.ser"));
MyClass obj = (MyClass) is.readObject();
is.close();
```
- `implements Serializable` (marker interface)
- `transient` — skip field during serialization (gets default value)
- Constructor does **not** run during deserialization

### Text I/O
```java
// Write
BufferedWriter w = new BufferedWriter(new FileWriter("file.txt"));
w.write("text"); w.newLine(); w.close();

// Read
BufferedReader r = new BufferedReader(new FileReader("file.txt"));
String line;
while ((line = r.readLine()) != null) { ... }
r.close();
```

---

## 9. ArrayList & Sorting

### ArrayList Methods
| Method | Description |
|---|---|
| `add(e)` | Add to end |
| `get(i)` | Get by index |
| `set(i, e)` | Replace at index |
| `remove(i)` | Remove by index |
| `size()` | Current size |
| `contains(o)` | Check existence |

### Comparable (natural order)
```java
class Song implements Comparable<Song> {
    public int compareTo(Song o) {
        return this.title.compareTo(o.title);
    }
}
```
`Collections.sort(list)` uses natural ordering.

### Comparator (custom order)
```java
Collections.sort(list, new Comparator<Song>() {
    public int compare(Song a, Song b) {
        return a.getArtist().compareTo(b.getArtist());
    }
});
```

### Iterator (safe removal during traversal)
```java
Iterator<Song> it = list.iterator();
while (it.hasNext()) {
    if (it.next().getRating() < 3) it.remove();
}
```

---

## 10. Data Structures — Big-O

| DS | add | remove | get/contains | Notes |
|---|---|---|---|---|
| **ArrayList** | O(1) end, O(n) mid | O(n) | **O(1)** get | Contiguous, index access |
| **LinkedList** | O(1) ends, O(n) mid | O(1) ends | O(n) get/indexOf | Doubly-linked nodes |
| **Stack** | **O(1)** push | **O(1)** pop | **O(1)** peek | LIFO |
| **Queue** | **O(1)** offer | **O(1)** poll | **O(1)** peek | FIFO |
| **HashSet** | **O(1)** | **O(1)** | **O(1)** contains | hashCode()+equals() |
| **TreeSet** | **O(log n)** | **O(log n)** | **O(log n)** contains | compareTo(), sorted |
| **HashMap** | **O(1)** | **O(1)** | **O(1)** get | Key: hashCode()+equals() |
| **TreeMap** | **O(log n)** | **O(log n)** | **O(log n)** get | Key: compareTo(), sorted |

---

## 11. Map Iteration

```java
// keys
for (K key : map.keySet()) { }

// values
for (V val : map.values()) { }

// entries
for (Map.Entry<K, V> e : map.entrySet()) {
    e.getKey();
    e.getValue();
}
```

---

## 12. Common Mistakes to Avoid

| Mistake | Correct |
|---|---|
| `java MyFile.class` | `java MyFile` |
| `if (1)` | `if (true)` |
| `int[] arr; arr.length()` | `arr.length` (field, not method) |
| Forgetting `f` suffix: `float f = 3.14` | `float f = 3.14f` |
| `String s; s.length;` | `s.length()` (method) |
| `==` for String content comparison | `s1.equals(s2)` |
| Calling method on `null` reference | Check for `null` first |
| Removing during enhanced for loop | Use `Iterator.remove()` |
| Forgetting `import java.util.*` | Import before using List, Map, etc. |
| No-arg constructor missing when subclass needs `super()` | Add one or call `super(args)` |
