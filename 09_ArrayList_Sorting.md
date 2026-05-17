# Lecture 9: ArrayList, Sorting & Iterator

## 1. Recap: ArrayList

```java
ArrayList<String> list = new ArrayList<>();
list.add("hello");         // add to end
list.get(0);               // get by index
list.remove(0);            // remove by index
list.size();               // current size
list.contains("hello");    // check existence
```

Reading from file into ArrayList:
```java
ArrayList<String> songList = new ArrayList<>();
BufferedReader reader = new BufferedReader(new FileReader("Songs.txt"));
String line;
while ((line = reader.readLine()) != null) {
    String[] parts = line.split("/");
    songList.add(parts[0]);
}
```

## 2. Sorting: Collections.sort()

```java
import java.util.Collections;
Collections.sort(songList);    // sort Strings (dictionary order)
```

## 3. Comparable<T> (Natural Ordering)

Built-in sorting rule for a class:

```java
public class Song implements Comparable<Song> {
    private String title;
    private String artist;

    @Override
    public int compareTo(Song other) {
        return this.title.compareTo(other.title);  // sort by title
    }
}
```
Now `Collections.sort(songList)` works.

### compareTo() Return Values:
| Return | Meaning |
|---|---|
| **negative** | `this` comes **before** `other` |
| **zero** | equal |
| **positive** | `this` comes **after** `other` |

### Generic Interface
```java
interface Comparable<T> {
    int compareTo(T o);
}
```
`T` is a type placeholder — substituted with actual type (e.g., `Song`).

## 4. Comparator<T> (Custom Ordering)

For multiple sorting orders:

```java
import java.util.Comparator;
public class TitleComparator implements Comparator<Song> {
    @Override
    public int compare(Song s1, Song s2) {
        return s1.getTitle().compareTo(s2.getTitle());
    }
}
public class ArtistComparator implements Comparator<Song> {
    @Override
    public int compare(Song s1, Song s2) {
        return s1.getArtist().compareTo(s2.getArtist());
    }
}
```
Usage:
```java
Collections.sort(songList, new TitleComparator());
Collections.sort(songList, new ArtistComparator());
```

## 5. Comparable vs Comparator

| | Comparable | Comparator |
|---|---|---|
| Package | `java.lang` | `java.util` |
| Method | `compareTo(T o)` | `compare(T o1, T o2)` |
| Usage | Natural ordering (one default) | Custom ordering (many alternatives) |
| Class modified | The class itself | Separate class |

## 6. Iterator

**Problem**: Removing elements during enhanced for loop causes `ConcurrentModificationException`.

**Solution**: Use `Iterator`:

```java
import java.util.Iterator;

Iterator<Song> it = songList.iterator();
while (it.hasNext()) {
    Song song = it.next();
    if (song.getRating() < 3) {
        it.remove();    // safe removal!
    }
}
```

### Iterator Methods
| Method | Description |
|---|---|
| `hasNext()` | Returns `true` if more elements |
| `next()` | Returns next element, advances cursor |
| `remove()` | Removes last element returned by `next()` |

## 7. Collection Framework Overview

| Collection | Order | Duplicates | Sorted | Access |
|---|---|---|---|---|
| `ArrayList` | Insertion order | ✅ Yes | ❌ No | By index |
| `LinkedList` | Insertion order | ✅ Yes | ❌ No | Both ends |
| `HashSet` | No order | ❌ No | ❌ No | By element |
| `TreeSet` | Sorted | ❌ No | ✅ Yes | By element |
| `HashMap` | No order | Keys: ❌ | ❌ No | By key |
| `TreeMap` | Sorted by key | Keys: ❌ | ✅ Yes | By key |
| `Queue` | FIFO | ✅ Yes | ❌ No | Front/back |
| `Stack` | LIFO | ✅ Yes | ❌ No | Top only |

> ⚠️ **PPT 标注后续课程会详细讲解各集合类型：**
> - `LinkedList`、`Stack`、`Queue`、`Deque` → **详见 Lecture 11**
> - `HashSet`、`TreeSet` → **详见 Lecture 12**
> - `HashMap`、`TreeMap` → **详见 Lecture 13**
> - 各操作的 Big-O 性能分析 → **详见 Lecture 10**
