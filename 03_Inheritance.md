# Lecture 3: Inheritance

## 1. What is Inheritance?

- Subclass **acquires properties** (fields + methods) of superclass
- Use keyword `extends`
- Subclass = **specialization** of superclass

```java
public class BankAccount { ... }
public class Savings extends BankAccount { ... }
```

### What subclass can do:
- Inherit all non-private fields and methods
- Add its own fields and methods
- **Override** inherited methods
- Give inherited fields any value

## 2. IS-A vs HAS-A

| Relationship | Test | Example |
|---|---|---|
| **IS-A** (inheritance) | X **is a** Y? | `Savings IS-A BankAccount` ✅ |
| **HAS-A** (composition) | X **has a** Y? | `PetShop HAS-A Dog` ✅ → field |

### IS-A Test Rules:
- Works **upward** in tree: `Dog IS-A Canine IS-A Animal`
- Does **not** work downward: `Animal IS-A Dog` ❌
- Design: use inheritance only when IS-A holds

## 3. Method Overriding

Subclass provides **specific implementation** of superclass method:

```java
class Animal { void makeNoise() { ... } }
class Dog extends Animal {
    void makeNoise() { System.out.println("Woof!"); }  // override
}
```

### Overriding Rules:
- Same method name **and** same parameter list
- Return type must be compatible (same or subclass)
- Cannot be **less** accessible (e.g., `public` → `private` ❌)

### Calling Superclass Method
```java
super.makeNoise();  // calls parent's version
```

## 4. Overriding vs Overloading

| | Method Overriding | Method Overloading |
|---|---|---|
| Relationship | Inheritance | Same class |
| Method name | Same | Same |
| Parameter list | **Same** | **Different** |
| Return type | Compatible | Can differ |
| Access level | Same or friendlier | Can differ |

## 5. Constructor Chaining

When creating subclass object:
1. Subclass constructor calls `super()` implicitly
2. Walks up hierarchy to `Object` class
3. Superclass parts constructed **first**, then subclass

> ⚠️ **PPT 标注 "We will talk about Object later"：** `Object` 类是所有类的终极父类，其关键方法（`toString()`, `equals()`, `hashCode()`, `getClass()`）— **详见 Lecture 4（多态）第 3 节**

### Rules:
```java
public Savings(double balance, double rate) {
    super(balance);          // must be FIRST statement
    this.rate = rate;
}
public Savings(double rate) {
    this(0.0, rate);         // calls another constructor in SAME class
}
```
- `super()` and `this()` must be **first statement** in constructor
- A constructor can have `super()` **or** `this()`, **never both**
- If superclass has **no no-arg constructor**, you must explicitly call `super(args)`

### Common Error:
```java
class BankAccount {
    BankAccount(double balance) { ... }  // no no-arg constructor!
}
class Savings extends BankAccount {
    Savings() { }  // ❌ COMPILE ERROR: implicit super() can't find match
}
```
**Fix**: Add no-arg constructor to superclass, or call `super(value)` explicitly.

## 6. Access Modifiers (Inheritance Context)

| Modifier | Same Class | Same Package | Subclass | World |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `default` | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

- Subclass **cannot** access `private` members of superclass
- Subclass **can** access `protected` and `public` members

## 7. Inheritance Design Steps

1. Find objects with **common attributes and behaviors**
2. Put common code in a **superclass**
3. Decide if subclass needs **specific behavior**
4. Look for **more abstraction** opportunities (e.g., intermediate classes like `Feline`, `Canine`)
