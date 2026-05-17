# Lecture 8: Exception Handling

## 1. Exception Types

```
Object
 └── Throwable
      ├── Error (unrecoverable, e.g., OutOfMemoryError)
      └── Exception
           ├── RuntimeException (unchecked)
           └── Other (checked, e.g., IOException)
```

| Type | Checked at compile time? | Examples |
|---|---|---|
| **Checked** (not `RuntimeException`) | ✅ Yes — must handle or declare | `IOException`, `FileNotFoundException` |
| **Unchecked** (`RuntimeException`) | ❌ No — compiler ignores | `NullPointerException`, `ClassCastException`, `IndexOutOfBoundsException` |

## 2. Checked vs Unchecked

### Unchecked Exceptions (RuntimeException)
Come from **code logic errors**:
- `ClassCastException` — casting incompatible type
- `NullPointerException` — calling method on `null`
- `IndexOutOfBoundsException` — invalid array/list index

### Checked Exceptions
Must be either **handled** or **declared**:
```java
public FileReader(String fileName) throws FileNotFoundException
```

## 3. try/catch/finally

### Try-Catch
```java
try {
    FileReader fr = new FileReader("Test.txt");  // risky!
} catch (Exception ex) {
    ex.printStackTrace();  // handle the exception
}
```

### Flow:
| If risky method... | Try block | Catch block |
|---|---|---|
| **Succeeds** | Completes fully | Never runs |
| **Throws** | Stops immediately | Runs |

### Finally Block
Always runs **regardless** of exception:
```java
FileReader fr = null;
try {
    fr = new FileReader("Test.txt");
} catch (Exception ex) {
    ex.printStackTrace();
} finally {
    if (fr != null) { /* close resources */ }
}
```

### Multiple Catch Blocks
```java
try {
    FileReader fr = new FileReader("Test.txt");
    // ...
} catch (FileNotFoundException ex) {
    // specific handler
} catch (IOException ex) {
    // broader handler
}
```
JVM checks catch blocks **in order**, first match wins.

## 4. Defining Risky Methods

### throws (declare)
```java
public void takeRisk() throws Exception {
    // method might throw Exception
}
```

### throw (create and throw)
```java
public void takeRisk() throws Exception {
    if (someCondition) {
        throw new Exception("Something went wrong");
    }
}
```

## 5. Ducking (Exception Propagation)

Instead of handling an exception, declare it with `throws` to pass it to the caller:

```java
public void foo() throws ClothingException {
    laundry.doLaundry();                   // may throw
}

public static void main(String[] args) throws ClothingException {
    Washer w = new Washer();
    w.foo();                               // caller ducks too
}
```

The exception propagates up the **call chain** until someone handles it via `catch`.

## 6. try-with-resources (modern Java)
```java
try (BufferedReader reader = new BufferedReader(new FileReader("Test.txt"))) {
    String line = reader.readLine();
} catch (IOException e) {
    e.printStackTrace();
}
// reader automatically closed (no finally needed)
```

## 7. Key Points

- Checked exceptions **must** be handled or declared
- Unchecked exceptions are **optional** to handle
- `finally` always runs (even if catch throws or return is hit)
- Catch more **specific** exceptions before general ones
- Use `throws` to duck, use `throw` to actually throw an exception
- `printStackTrace()` prints the method call sequence leading to the exception
