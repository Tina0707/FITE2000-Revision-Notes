# Lecture 1: Introduction to Java & OOP

## 1. Procedural Programming (PP) vs OOP

| | Procedural Programming | Object-Oriented Programming |
|---|---|---|
| **Focus** | Tasks (functions) | Data (objects) |
| **Program =** | Collection of functions operating on shared data | Collection of interacting objects |
| **Example** | Global `balance` + separate `withdraw()`, `deposit()` | `BankAccount` class encapsulating state + behavior |
| **Drawbacks** | Data vulnerable, hard to maintain, low reusability | Protected data, easy to maintain, high reusability |

## 2. Four OOP Features

| Feature | Definition | Key Point |
|---|---|---|
| **Abstraction** | Represent essential features, hide background details | Focus on **what**, not **how** |
| **Encapsulation** | Wrap data + operations into a unit, hide implementation | Information hiding, access via exposed interface only |
| **Inheritance** | Subclass acquires properties of superclass | **IS-A** relationship |
| **Polymorphism** | Ability to take more than one form | Subclass object can replace superclass object |

## 3. Java Overview

- **Simple**: Similar to C/C++, removed pointers/struct
- **Object-oriented**: Everything is an object
- **Platform independent**: `javac` → bytecode → JVM runs anywhere

> ⚠️ **PPT 标注 "We will talk about later" 的概念：**
> - `static` 关键字 → **详见 Lecture 2 第 5 节**
> - 对象引用 (object references) → **详见 Lecture 2 第 2 节**
> - 数组 (arrays) → **详见 Lecture 2 第 6 节**

### Compilation & Execution
```bash
javac MyFile.java    # → MyFile.class (bytecode)
java MyFile          # JVM loads class, runs main() (no .class suffix!)
```

### Program Structure
```java
public class ClassName {           // file name must match class name
    public static void main(String[] args) {
        System.out.println("Hello!");
    }
}
```

> **🔗 详见 Lecture 2:** `static` 关键字（第 5 节）— 静态变量/方法属于类而非实例；`String[] args` 数组（第 6 节）— 命令行参数；对象引用（第 2 节）— 引用变量是对象的"遥控器"

## 4. Variables

### Static vs Dynamic Typing
| Java (Static) | Python (Dynamic) |
|---|---|
| Type checked at compile time | Type checked at runtime |
| Type cannot change | Type can change |
| `String a = "hi"; a = 5;` ❌ | `a = "hi"; a = 5;` ✅ |

### 8 Primitive Types

| Type | Bits | Range |
|---|---|---|
| `boolean` | JVM-dependent | `true` / `false` |
| `char` | 16 | 0 ~ 65535 |
| `byte` | 8 | -128 ~ 127 |
| `short` | 16 | -32768 ~ 32767 |
| `int` | 32 | -2³¹ ~ 2³¹-1 |
| `long` | 64 | huge |
| `float` | 32 | (needs `f` suffix) |
| `double` | 64 | (default for decimals) |

### Variable Scope
| Type | Location |
|---|---|
| **Field** | Inside class, outside methods — one per object |
| **Parameter** | Method parameter list |
| **Local** | Inside a method |

### Naming Rules
- Start with letter, `_`, or `$` (not number)
- Cannot be reserved word (`if`, `else`, `class`, `public`, `static`, `void`, etc.)

## 5. Control Flow

### Branching
```java
if (x == 10) { ... }
else if (x < 10) { ... }
else { ... }
// ⚠️ if (1) {} is COMPILE ERROR — 1 is not boolean
```

### Loops
```java
for (int x = 0; x < 100; x++) { ... }
while (x > 0) { x--; }
```

### Operators
- **Arithmetic**: `+`, `-`, `*`, `/`, `%`
- **Relational**: `==`, `!=`, `>`, `>=`, `<`, `<=`
- **Logical**: `&&`, `||`, `!`
- **Assignment**: `=`, `+=`, `-=`, `++`, `--`
