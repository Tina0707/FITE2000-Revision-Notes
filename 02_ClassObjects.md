# Lecture 2: Class & Object

## 1. Classes and Objects

- **Class** = blueprint for an object (defines state + behavior)
- **Object** = instance of a class (unique, has its own state)

```java
class Dog {
    int size;              // instance variable (state)
    String breed;
    String name;

    void makeNoise() {     // method (behavior)
        System.out.println("Woof!");
    }
}
```

## 2. Creating Objects (3 Steps)

```java
Dog myDog = new Dog();
//  1.     2.   3.
```
1. **Declare** reference variable: `Dog myDog`
2. **Create** object on heap: `new Dog()`
3. **Assign** reference: `myDog = ...`

- Reference variable = "remote control" with buttons (dot operator `.`)
- Access fields/methods via dot: `myDog.size = 40; myDog.makeNoise();`
- `null` = reference points to nothing; calling methods on null → `NullPointerException`

## 3. Method Overloading

Same method name, **different parameter lists** (number, type, or order):
```java
void makeNoise() { ... }
void makeNoise(int numOfBarks) { ... }
void makeNoise(String s, int i) { ... }
void makeNoise(int i, String s) { ... }  // different order = overloaded
```
- Return type and access level **can** differ
- `void makeNoise(int i)` and `void makeNoise(int num)` are **NOT** overloading

## 4. Encapsulation & Access Modifiers

| Modifier | Class | Package | Subclass | World |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `default` (none) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

> ⚠️ **PPT 标注 "We will talk about later"：** `package` 和 `subclass` 的概念在此处仅初步介绍 — **详见 Lecture 3（继承）第 6 节**，其中详细说明了不同访问级别在子类中的表现，以及默认包的概念

### Getters & Setters
```java
private int size;
public int getSize() { return size; }
public void setSize(int size) {
    this.size = size;   // 'this' refers to current instance
}
```

## 5. Constructors

- Same name as class, **no return type** (not even `void`)
- Automatically called when `new` is used
- Used to initialize instance variables

```java
Dog() { }                         // default constructor
Dog(int size) { this.size = size; }
Dog(String name) { this.name = name; }
```

**Key rules:**
- If **no constructor** defined → compiler adds empty no-arg constructor
- If **any constructor** defined → compiler does NOT add default
- Default values if uninitialized: `0` / `0.0` / `false` / `null`

## 6. Static Keyword

| | Static | Non-static |
|---|---|---|
| **Belongs to** | Class (per-class) | Instance (per-object) |
| **Shared?** | Yes, one copy for all instances | No, each object has own copy |
| **Access** | `ClassName.member` | `objectRef.member` |

```java
public static int count = 0;      // static variable
public static void clear() { ... } // static method
```

- Static methods **cannot** access non-static members directly
- Static variables initialized when class is first loaded

## 7. Arrays

```java
int[] nums = new int[4];        // array of primitives
nums[0] = 6; nums[1] = 19;

Dog[] pets = new Dog[4];         // array of references
pets[0] = new Dog();
```
- Zero-indexed, length fixed at creation (`nums.length` — **not** a method)
- Arrays are objects; elements are variables (primitive or reference)

## 8. String

- **Immutable** — cannot change after creation
- `String s = "Hello!";` or `new String("Hello!");`

### Common Methods
| Method | Returns |
|---|---|
| `s.length()` | number of characters |
| `s.charAt(i)` | char at index `i` |
| `s.equals(t)` | true if same content |
| `s.trim()` | remove leading/trailing whitespace |
| `s.toUpperCase()` / `s.toLowerCase()` | converted string |
| `s.split(regex)` | String[] split around matches |
| `s1 + s2` | concatenation |

### Command-Line Arguments
```java
public static void main(String[] args) {
    // args[0], args[1], ... from command line
}
```

## 9. ArrayList

Dynamic array from `java.util`:
```java
import java.util.ArrayList;
ArrayList<String> list = new ArrayList<>();
list.add("hello");             // add to end
String s = list.get(0);        // get by index
list.remove(0);                // remove by index
int size = list.size();        // current size
boolean has = list.contains(s);// check existence
```

### ArrayList vs Array

| Feature | ArrayList | Array |
|---|---|---|
| Size | Dynamic (grows/shrinks) | Fixed at creation |
| Syntax | Regular object methods | Special `[]` syntax |
| Type parameter | Classes only (use wrappers for primitives) | Any type |
| Length | `size()` method | `.length` field |

### Wrapper Classes & Autoboxing
```java
ArrayList<Integer> nums = new ArrayList<>();
nums.add(3);                     // autoboxing: int → Integer
int n = nums.get(0);             // unboxing: Integer → int
```

| Primitive | Wrapper |
|---|---|
| `int` | `Integer` |
| `char` | `Character` |
| `double` | `Double` |
| `boolean` | `Boolean` |
