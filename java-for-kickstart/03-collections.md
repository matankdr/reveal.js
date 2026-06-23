# Java Collections


## Java Collections

<table>
  <thead>
    <tr><th>List</th><th>Set</th><th>Map</th></tr>
  </thead>
  <tbody style="font-size: 0.55em">
    <tr>
      <td>Ordered sequence of elements. Maintains insertion order and allows duplicates.</td>
      <td>Collection that contains no duplicate elements. Deduplicated data by nature.</td>
      <td>Stores data in Key-Value pairs. Maps keys to values for fast lookup.</td>
    </tr>
  </tbody>
</table>

---

## Concrete Implementations

```java
// ArrayList (List)
var names = new ArrayList<String>();

// HashSet (Set)
var ids = new HashSet<Long>();

// HashMap (Map)
var config = new HashMap<String, String>();
```


## Alternative Implementations

```java
var names  = new ArrayList<String>();
var names2 = new LinkedList<String>();

var ids    = new HashSet<Long>();
var ids2   = new TreeSet<Long>();

var config  = new HashMap<String, String>();
var config2 = new TreeMap<String, String>();
```

---

<!-- .slide: data-auto-animate -->
## Modern Syntax and Immutability <!-- .element: data-id="title" -->

### Mutable (Legacy)

```java
List list = new ArrayList<>();
list.add("A");
list.add("B");
```


<!-- .slide: data-auto-animate -->
## Modern Syntax and Immutability <!-- .element: data-id="title" -->

### Immutable (Modern)

```java
var list = List.of("A", "B", "C");

var map = Map.of(
  "key1", "value1",
  "key2", "value2"
);
```

- Modern factory methods: `List.of()`, `Map.of()`
- Collections are Immutable (unmodifiable) by default
- Safer for concurrent operations

---

## Hands-on Lab: User Discovery
[Exercise 2](https://github.com/wix-private/wix-academy/tree/master/server-onboarding/java-exercises/java-fundamentals#exercise-2-collections--list-vs-set-vs-map)

Java collections
