# Lecture 6: Input/Output

## 1. Object Serialization

Saving object state (instance variable values) so identical objects can be restored.

### Serializable Interface
- **Marker interface** — no methods to implement
- If superclass is Serializable, subclasses are automatically Serializable

```java
public class Dog implements Serializable { ... }
public class Puppy extends Dog { ... }          // also Serializable
```

### The Object Graph
When an object is serialized, **all objects it references** are also serialized:
```java
class Zoo implements Serializable {
    private Hippo h;     // must also be Serializable!
    private Tiger t;
    private Lion l;
}
```
"All or nothing" — either entire graph serializes, or serialization fails.

### Transient Keyword
Mark fields that **cannot or should not** be serialized:
```java
public class Chat implements Serializable {
    transient String currentID;   // skipped during serialization
    String userName;               // will be serialized
}
```
Deserialized transient fields get **default values** (`null`, `0`, `false`).

## 2. Serialization Process

### Writing Objects (Serializing)
```java
FileOutputStream fos = new FileOutputStream("MyGame.ser");
ObjectOutputStream os = new ObjectOutputStream(fos);
os.writeObject(myDog);
os.writeObject(myCat);
os.close();    // closes underlying streams too
```

### Reading Objects (Deserializing)
```java
FileInputStream fis = new FileInputStream("MyGame.ser");
ObjectInputStream is = new ObjectInputStream(fis);
Dog myDog = (Dog) is.readObject();    // read in SAME order as written
Cat myCat = (Cat) is.readObject();
is.close();
```

## 3. Stream Architecture

| Type | Example | Purpose |
|---|---|---|
| **Connection stream** | `FileOutputStream`, `FileInputStream` | Connects to source/destination (file) |
| **Chain stream** | `ObjectOutputStream`, `ObjectInputStream` | Higher-level methods (writes objects) |

Chain streams are wrapped around connection streams for flexibility.

## 4. Deserialization Details

- JVM determines class from info stored with serialized object
- **Constructor does NOT run** during deserialization
- Instance variables restored from serialized state
- `transient` and `static` variables get **default values**
- Non-serializable superclass: its **no-arg constructor runs**

```java
class Animal { public Animal() { color = "White"; } }  // non-serializable
class Dog extends Animal implements Serializable { }    // serializable
// Deserializing Dog: Animal constructor runs (color = "White"),
// Dog's instance variables from stream, but transient = default
```

## 5. Text I/O

### Writing Text
```java
FileWriter fw = new FileWriter("Test.txt");
BufferedWriter writer = new BufferedWriter(fw);
writer.write("This is a plain text file");
writer.newLine();              // platform-independent newline
writer.close();
```

### Reading Text
```java
FileReader fr = new FileReader("Test.txt");
BufferedReader reader = new BufferedReader(fr);
String line;
while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();
```

### BufferedWriter
- Holds data in buffer until full, then writes to disk
- Call `flush()` to force write before buffer is full

## 6. Key Points

- If file doesn't exist, `FileOutputStream`/`FileWriter` **creates it**
- If file exists, writing **overwrites** it
- Always **close** streams (closing top stream closes underlying ones)
- I/O operations throw checked exceptions (`IOException`)
- Text I/O simpler but parsing is manual; serialization harder to read but safer
