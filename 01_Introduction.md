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
- **Platform independent**: 一次编写，到处运行 (Write Once, Run Anywhere)

### 实现平台无关的核心流程

```
MyApp.java  ──javac编译──▶  MyApp.class  ──java运行──▶  程序输出
  (源代码)         ↓       (字节码)          ↓
                 人类可读                 JVM 逐行翻译
                                      成当前系统的
                                      机器码去执行
```

**三个角色的分工：**

| 角色 | 全称 | 做什么 | 类比（翻译） |
|---|---|---|---|
| **`javac`** | Java 编译器 | 把 `.java` 源代码**编译**成 `.class` 字节码 | 把中文书翻译成**国际音标** |
| **Bytecode** | 字节码 (`.class` 文件) | 一种**中间语言**，与操作系统无关 | 国际音标——不管在哪国，音标写法都一样 |
| **JVM** | Java 虚拟机 | 把字节码**解释/编译**成当前系统的机器码去执行 | 不同国家的人看国际音标，用自己的母语发音读出来 |

**为什么能做到平台无关？**
- `javac` 编译出来的 `.class` 文件（字节码）**在任何平台上都是同一份** —— Windows、macOS、Linux 上编译结果一样
- 每个平台有**对应版本的 JVM** —— Windows 版 JVM 把字节码翻译成 Windows 认识的机器码，macOS 版翻译成 macOS 认识的
- 程序员只写一份代码，剩下的交给 **"javac 统一编译 → 不同平台 JVM 各自翻译"** 这条流水线

> ⚠️ **PPT 标注 "We will talk about later" 的概念：**
> - `static` 关键字 → **详见 Lecture 2 第 5 节**
> - 对象引用 (object references) → **详见 Lecture 2 第 2 节**
> - 数组 (arrays) → **详见 Lecture 2 第 6 节**

### 编译与执行（三步骤）
```bash
# 第 1 步：编译（javac）
javac MyFile.java
#     ↓ 生成 MyFile.class（字节码 bytecode）

# 第 2 步：运行（java）
java MyFile
#     ↓ JVM 将字节码翻译为当前系统的机器码并执行
```

> **注意：** `java MyFile` 不加 `.class` 后缀，也不加路径 `java ./MyFile`

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
