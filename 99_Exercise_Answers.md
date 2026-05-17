# 讲义中未标注答案的问题解答

本文档整理了各 lecture 幻灯片中出现但**未直接给出答案**的问题（包括绿色高亮问题、代码追问等）。

---

## Lecture 2: Class & Object

### Q1 (Slide 17): myDog = null 后再调用方法会怎样？

```java
Dog myDog = new Dog();
myDog = null;
myDog.makeNoise();  // ❓ 这里会发生什么？
```

**答案：** 抛出 `NullPointerException`（运行时异常）。  
`myDog` 此时指向 `null`，不指向任何对象，调用方法等价于"用没插电的遥控器按按钮"。

---

### Q2 (Slide 19): 哪个重载方法被调用？

```java
myDog.makeNoise();     // ❓ 调用哪个方法？
myDog.makeNoise(3);    // ❓ 调用哪个方法？
```

**答案：**
- `myDog.makeNoise()` → 调用**无参** `void makeNoise()`
- `myDog.makeNoise(3)` → 调用**有参** `void makeNoise(int numOfBarks)`

编译器根据**参数列表**匹配：无参数就调无参版本，传了 `int` 就调带 `int` 参数的版本。

---

### Q3 (Slide 38): 定义了有参构造器后调用 `new Dog()`

```java
class Dog {
    private int size;
    private String name;
    Dog(int size) { this.size = size; }
    public static void main(String[] args) {
        Dog myDog = new Dog();  // ❓ 这里会发生什么？
    }
}
```

**答案：** **编译错误**。  
一旦程序员定义了任何构造器（如 `Dog(int size)`），编译器就**不再提供默认无参构造器**。`new Dog()` 找不到匹配的 `Dog()` 构造器，编译失败。

**修复方法：**
```java
// 方案1：手动添加无参构造器
Dog() { }

// 方案2：new 时传参
Dog myDog = new Dog(10);
```

---

### Q4 (Slide 41): 静态方法为什么不能访问非静态成员？

```java
public static void showCount() {
    System.out.println(count);   // ✅ 可以，count 也是静态的
    System.out.println(name);    // ❓ 为什么编译错误？
}
```

**答案：** **编译错误**。  
静态方法属于**类**，调用时可能**还没有任何对象存在**。而 `name` 是实例变量，必须依附于某个具体对象。  
"连对象都没有，哪来的 `name`？" → 编译器直接报错。

---

### Q5 (Slide 48, 绿色问题): `s[2] = '!'` 会怎样？

```java
String s = new String("Hello");
// ❓ 如果加上这一行会怎样？
s[2] = '!';
```

**答案：** **编译错误**。  
Java 中 `String` 是**不可变 (immutable)** 的，String 类**没有提供数组下标索引操作符** `[]`。  
`s[2] = '!'` 这种写法在 Java 中对 String 是**非法语法**。

与之对比，`char[]` 数组可以这样改：
```java
char[] data = {'H', 'e', 'l', 'l', 'o'};
data[2] = '!';   // ✅ 数组可以这样改 → 内容变为 "He!lo"
```

---

## Lecture 3: Inheritance

### Q6 (Slide 6): 继承树中的变量和方法数量

继承结构如下：
```
Doctor (has: WorkAtHospital, TreatPatient)
  ├── Surgeon (has: MakeIncision)
  └── FamilyDoctor (has: MakeHouseCalls, GiveAdvice)
```

**问题与答案：**

| 问题 | 答案 |
|---|---|
| Surgeon 有几个实例变量？ | Surgeon 自己没定义新变量，但继承 Doctor 的 → **0 个自己定义的 + 继承的**（PPT 示意图中 Doctor 未显示变量，故答案为 0） |
| FamilyDoctor 有几个实例变量？ | **0 个**（同上） |
| Doctor 有几个方法？ | **2 个** (`WorkAtHospital`, `TreatPatient`) |
| Surgeon 有几个方法？ | Surgeon 自己定义 1 个 (`MakeIncision`) + 继承 Doctor 的 2 个 = **共 3 个** |
| FamilyDoctor 有几个方法？ | FamilyDoctor 自己定义 2 个 (`MakeHouseCalls`, `GiveAdvice`) + 继承 Doctor 的 2 个 = **共 4 个** |
| FamilyDoctor 能做 `treatPatient()` 吗？ | **可以。** 该方法继承自 `Doctor`。 |
| FamilyDoctor 能做 `makeIncision()` 吗？ | **不可以。** `makeIncision()` 是 `Surgeon` 独有的，`FamilyDoctor` 没继承它。 |

---

### Q7 (Slide 18): "Dog extends PetShop" 合理吗？

```java
public class Dog extends PetShop { ... }  // ❓ 这样可以吗？
```

**答案：** **不应这样设计。**  
用 IS-A 检验：**"Dog IS-A PetShop" 不成立**。  
正确做法是 HAS-A 关系：**"PetShop HAS-A Dog"**，即 PetShop 中有一个 `Dog` 类型的字段。

---

### Q8 (Slide 33): 为什么编译错误？

```java
public class BankAccount {
    public BankAccount(double balance) { }  // 只有有参构造器
}
public class Savings extends BankAccount {
    public Savings() {
        super();  // ❓ 这里为什么编译错误？
    }
}
```

**答案：** `super()` 试图调用 `BankAccount()` 无参构造器，但 `BankAccount` **没有定义无参构造器**（只定义了 `BankAccount(double)`）。  
**修复：** 要么在 `BankAccount` 中加无参构造器，要么在 `Savings()` 中显式调用 `super(某个数值)`。

---

## Lecture 4: Polymorphism

### Q9 (Slide 30): 向上转型后能调用子类独有的方法吗？

```java
Doctor s = new Surgeon();
s.MakeIncision();  // ❓ 编译通过吗？
```

**答案：** **编译错误。**  
`s` 的**引用类型**是 `Doctor`，编译器只检查 `Doctor` 类有没有 `MakeIncision()` 方法——没有，所以报错。  
解决：向下转型 `((Surgeon) s).MakeIncision();` 或引用类型直接设为 `Surgeon`。

---

### Q10 (Slide 32): Object 引用能调用子类方法吗？

```java
Object myDog = new Dog();
myDog.makeNoise();  // ❓ 编译通过吗？
```

**答案：** **编译错误。**  
`myDog` 的引用类型是 `Object`，编译器只看到 `Object` 类的方法——`Object` 没有 `makeNoise()`。

---

### Q11 (Slide 33): ArrayList\<Object\> 取出来的是什么类型？

```java
ArrayList<Object> list = new ArrayList<>();
list.add(new Dog());
Dog d = list.get(0);  // ❓ 编译通过吗？
```

**答案：** **编译错误。**  
`get(0)` 返回的是 `Object` 类型引用，不能直接赋值给 `Dog`。需要向下转型：
```java
Dog d = (Dog) list.get(0);  // ✅ 显式转型
```

---

## Lecture 6: Interface

### Q12 (Slide 4): 实例化一个 Animal 对象有意义吗？

```java
Animal a = new Animal();  // ❓ 这样做好吗？
```

**答案：** 没有意义。`Animal` 是一个抽象概念——没有具体的体型、叫声、进食方式。应该将其声明为 `abstract class` 来**阻止实例化**，只能实例化其具体子类如 `Dog`、`Cat`。

---

### Q13 (Slide 24): 两个接口有相同 default 方法会怎样？

```java
interface A { default void hello() { ... } }
interface B { default void hello() { ... } }
class MyClass implements A, B {  // ❓ 编译通过吗？
}
```

**答案：** **编译错误。** 两个接口的默认方法签名冲突，实现类**必须重写 (override)** 来解决冲突：
```java
class MyClass implements A, B {
    @Override
    public void hello() {    // 必须自己实现，解决冲突
        A.super.hello();     // 可以选择调用某个接口的版本
    }
}
```

---

## Lecture 11: Lists

### Q14 (Slide 31): 括号匹配问题

**题目：** 判断字符数组中的括号是否匹配。匹配规则：每个左括号必须有同类型的右括号按正确顺序闭合。

**答案：使用 Stack 实现**
```java
public boolean isBalanced(char[] brackets) {
    Stack<Character> stack = new Stack<>();
    for (char c : brackets) {
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);           // 左括号入栈
        } else {
            if (stack.isEmpty()) return false;  // 没有左括号来匹配
            char top = stack.pop();
            // 检查是否匹配
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return stack.isEmpty();  // 最后栈应为空
}
```

---

### Q15 (Slide 37): 服务队列模拟

**题目：** 模拟一个服务队列——客户从队尾加入，从队头被服务。

**答案：使用 Queue 实现**
```java
Queue<String> queue = new LinkedList<>();
queue.offer("Alice");   // 加入队尾
queue.offer("Bob");
queue.offer("Cindy");

while (!queue.isEmpty()) {
    String customer = queue.poll();  // 从队头移除
    System.out.println("Serving " + customer);
}
// 输出:
// Serving Alice
// Serving Bob
// Serving Cindy
```

---

### Q16 (Slide 41): 回文判断

**题目：** 判断字符数组是否回文（正着读反着读一样）。

**答案：使用 Deque 实现**
```java
public boolean isPalindrome(char[] chars) {
    Deque<Character> deque = new LinkedList<>();
    for (char c : chars) {
        deque.addLast(c);
    }
    while (deque.size() > 1) {
        char front = deque.removeFirst();
        char back = deque.removeLast();
        if (front != back) return false;  // 首尾不相等 → 不是回文
    }
    return true;
}

// "madam" → true
// "noon"  → true
// "a"     → true
// "hello" → false
// "ab"    → false
```

---

## Lecture 12: Sets

### Q17 (Slide 12): 能在基于节点的结构上进行二分查找吗？

**答案：** **可以，但前提是节点结构是有序的二叉树（BST）。**  
这正是 `TreeSet` 的原理——它基于**自平衡二叉搜索树**，搜索时从根节点开始，每次比较后进入左子树或右子树，每步排除一半元素，时间复杂度 **O(log n)**。  
但普通的链表（如 `LinkedList`）**不能**进行二分查找，因为无法通过索引直接跳到中间位置。

---

## Lecture 13: Maps

### Q18 (Slide 20): 统计句子中每个单词出现的频率

**题目：** 写一个程序统计句子中每个单词出现的次数，然后按频率降序输出。

**答案：**
```java
import java.util.*;

public class WordFrequency {
    public static void main(String[] args) {
        String sentence = "apple banana apple cherry banana apple";
        String[] words = sentence.split(" ");

        // 第 1 步：统计频率（HashMap）
        Map<String, Integer> freq = new HashMap<>();
        for (String word : words) {
            freq.put(word, freq.getOrDefault(word, 0) + 1);
        }

        // 第 2 步：按频率排序（转为 List 再排序）
        List<Map.Entry<String, Integer>> list = new ArrayList<>(freq.entrySet());
        list.sort((a, b) -> b.getValue().compareTo(a.getValue()));  // 降序

        // 第 3 步：输出
        for (Map.Entry<String, Integer> e : list) {
            System.out.println(e.getKey() + " = " + e.getValue());
        }
    }
}
// 输出:
// apple = 3
// banana = 2
// cherry = 1
```
