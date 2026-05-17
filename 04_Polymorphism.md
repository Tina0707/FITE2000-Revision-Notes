# Lecture 4: Polymorphism

## 1. Types of Polymorphism

| Type | When | Mechanism |
|---|---|---|
| **Runtime** | Method execution | Method overriding + upcasting |
| **Compile-time** | Method resolution | Method overloading |

## 2. Runtime Polymorphism Examples

### Upcasting
Subclass object assigned to superclass reference:
```java
Animal myDog = new Dog();     // upcasting (automatic)
Animal myCat = new Cat();
```

### Method Overriding (Dynamic Binding)
The **most specific version** of the method runs:
```java
myDog.makeNoise();  // calls Dog's makeNoise(), not Animal's
myCat.makeNoise();  // calls Cat's makeNoise()
```

### Polymorphic Parameters
```java
public void feed(Animal animal) {   // superclass parameter
    animal.makeNoise();
}
feed(new Dog());  // ✅ accepts any subclass
feed(new Cat());
```

### Polymorphic Return Types
```java
public Animal createAnimal(String type) {  // returns superclass type
    if (type.equals("dog")) return new Dog();
    else return new Cat();
}
```

### Polymorphic Arrays
```java
Animal[] animals = new Animal[3];
animals[0] = new Dog();
animals[1] = new Cat();

for (Animal a : animals) {   // enhanced for loop
    a.makeNoise();
}
```

## 3. The Object Class

- **Every** class extends `Object` implicitly
- `Object` is the superclass of **everything**

### Key Object Methods

| Method | Purpose | Notes |
|---|---|---|
| `getClass()` | Returns runtime Class object | Cannot override |
| `toString()` | String representation | Should override |
| `equals(Object)` | Equality check | Should override for content comparison |
| `hashCode()` | Hash code value | Must override if `equals()` is overridden |

### toString() Example
```java
@Override
public String toString() {
    return "Dog | name: " + this.name;
}
```
`System.out.println(myDog)` automatically calls `myDog.toString()`.

### equals() Example
```java
@Override
public boolean equals(Object obj) {
    if (obj == this) return true;
    if (obj == null) return false;
    if (!(obj instanceof Dog)) return false;
    return this.name.equals(((Dog) obj).name);
}
```

### == vs equals()
```java
String a = new String("Apple");
String b = a;
String c = new String("Apple");
a == b    // true (same reference)
a == c    // false (different references)
a.equals(c) // true (same content)
```

## 4. Type Casting

### Upcasting (automatic)
```java
Dog dog = new Dog();
Object o = dog;       // upcast automatically
```

### Downcasting (explicit)
```java
Dog sameDog = (Dog) o;   // explicit cast required
```

### Why Not Use Object Everywhere?
Java is **strongly-typed** — compiler checks methods exist on reference type:
```java
Object myDog = new Dog();
myDog.makeNoise();  // ❌ COMPILE ERROR: Object has no makeNoise()
```

### Safe Casting with instanceof
```java
if (o instanceof Dog) {
    Dog sameDog = (Dog) o;
}
```

### Reference Type vs Object Type
```java
Animal animal = new Dog();       // Animal ref, Dog object
test.process(animal);            // Overloading resolved by REFERENCE type (Animal)
                                 // Overriding resolved by OBJECT type (Dog)
```

### The Reference Remote Control Analogy
- `Object` reference = few buttons (only Object methods)
- `Animal` reference = more buttons (Object + Animal methods)
- `Dog` reference = all buttons (Object + Animal + Dog methods)
