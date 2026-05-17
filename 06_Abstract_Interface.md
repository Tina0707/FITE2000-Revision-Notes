# Lecture 6: Abstract Classes & Interfaces

## 1. Why Abstract?

Some classes exist only for inheritance/polymorphism but should **never be instantiated**:
```java
Animal a = new Animal();  // What does an Animal look like? How does it eat?
```

Solution: mark class as `abstract`.

## 2. Abstract Classes

```java
public abstract class Animal { ... }
```
- **Cannot** be instantiated with `new`
- **Can** be used as a reference type: `Animal a = new Dog();` ✅
- Can have **both** abstract and non-abstract methods
- Can have constructors, fields, concrete methods

## 3. Abstract Methods

- No method body, ends with `;`
- Must be overridden by subclasses
- Class with abstract method **must** be declared abstract

```java
public abstract void makeNoise();    // no body, just declaration
```

### Rules:
| Class Type | Can have abstract methods? | Must implement inherited abstract methods? |
|---|---|---|
| `abstract class` | ✅ Yes | Optional (can leave to subclasses) |
| `concrete class` | ❌ No | **Must** implement ALL inherited abstract methods |

### Example:
```java
public abstract class Animal {
    public abstract void eat();       // abstract
    public void sleep() { ... }       // concrete
}
public abstract class Canine extends Animal {
    // implements eat() optionally — Canine is still abstract
}
public class Dog extends Canine {
    public void eat() { ... }         // MUST implement eat() — Dog is concrete
}
```

## 4. Deadly Diamond of Death (DDD)

Java **does not allow** multiple inheritance of classes:
```
    Animal
   /      \
Canine    Pet        ← both extend Animal (different size/eat())
   \      /
     Dog              ← Which size? Which eat()?
```

## 5. Interfaces

Java's solution to multiple inheritance: **interfaces**.

### Interface (pre-Java 8)
```java
public interface Pet {
    public abstract void beFriendly();
    public abstract void play();
}
// 'public' and 'abstract' are optional in interfaces
public interface Pet {
    void beFriendly();
    void play();
}
```

### Implementing an Interface
```java
public class Dog extends Canine implements Pet {
    public void beFriendly() { ... }   // must implement
    public void play() { ... }         // must implement
}
```

### Interface (Java 8+)
```java
public interface Pet {
    void beFriendly();                 // abstract (must implement)
    default void play() {              // default method (can override)
        System.out.println("Play with the Pet");
    }
    static void info() { ... }         // static method (belongs to interface)
}
```

### Multiple Interface Implementation
```java
public class Dog extends Canine implements Pet, Serializable { ... }
```

> ⚠️ **PPT 标注 "We will talk about serialization later"：** `Serializable` 是标记接口（无方法需要实现），用于对象序列化 — **详见 Lecture 7（I/O）第 3 节**

### Interface as Polymorphic Type
```java
ArrayList<Pet> pets = new ArrayList<>();
pets.add(new Dog());
pets.add(new Cat());
for (Pet p : pets) {
    p.play();  // polymorphic call
}
```

## 6. Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---|---|---|
| Instantiation | ❌ Cannot | ❌ Cannot |
| Constructor | ✅ Yes | ❌ No |
| Instance variables | ✅ Yes | ❌ No (only `static final`) |
| Concrete methods | ✅ Yes | ✅ Yes (default/static in Java 8+) |
| Multiple inheritance | ❌ Not allowed | ✅ Can implement multiple |
| `extends` vs `implements` | `extends` | `implements` |

## 7. Design Tips

- **Abstract class**: subclasses share common **state and behavior** (IS-A)
- **Interface**: unrelated classes share common **capability** (CAN-DO)
- A class can `extends` **one** class and `implements` **multiple** interfaces
