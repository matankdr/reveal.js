# Java Classes


## Classes
```java
public class Person {

}
```
- `public` - visible everywhere
- `class` - keyword
- `Person` - class name (usually, in `Person.java`)


## Classes
```java
public class Person {
  String name;
  int age;
}

Person p = new Person();
p.name = "Alice";
p.age = 30;
```


## Classes
```java
public class Person {
  String name;
  int age;

  void greet() {
    System.out.println("Hi, I'm " + name);
  }
}
```


## Classes
```java
public class Person {
  String name;
  int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

Person p = new Person("Alice", 30);
```
- Construtor: Same name as the class
- No return type
- `this` refers to the current object


---

## Static vs. instance members
```java
class Counter {
    int value; // each instance has its own value
}
```


```java
class Counter {
    static int value; // shared by all instances
}

Counter.value++; // usage
```
- Static is for:
  - Constants
  - Utility methods
  - Class-level state


---

## Equals and Hashcode
- Each object can be compared to other objects
- Default implementation: memory address
- Best practice: override `equals` for your class
- `hashcode`: hash representation of your object
  - must be overriden when `equals` is overriden
- Required when objects are compared or stored in collections





<!-- .slide: data-auto-animate -->
## Functions <!-- .element: data-id="title" -->
### High order functions
- Let's implement HTTP request filters
- Header keys are stored lowered-case
- Header values should be lowered-case

```scala
case class Request(method: String,
                   uri: String,
                   headers: Map[String, String] = Map(),
                   content: String = ""
                  )
def isGET(req: Request): Boolean = ???
def isPOST(req: Request): Boolean = ???

def getContentType(req: Request): String = ???
def getReferer(req: Request): String = ???
```
<!-- .element: data-id="code" -->


<!-- .slide: data-auto-animate -->
## Functions <!-- .element: data-id="title" -->
### High order functions



```scala
case class Request(method: String,
                   uri: String,
                   headers: Map[String, String] = Map(),
                   content: String = ""
                  )
def isGET(req: Request): Boolean = req.method == "GET"
def isPOST(req: Request): Boolean = req.method == "POST"

def getContentType(req: Request): String =
  headers("content-type").toLowerCase

def getReferer(req: Request): String =
  headers("referer").toLowerCase
```
<!-- .element: data-id="code" -->


<!-- .slide: data-auto-animate -->
# Can we do better? <!-- .element: data-id="title" -->


<!-- .slide: data-auto-animate -->
## Functions <!-- .element: data-id="title" -->
### High order functions

```scala
private def methodFilter(method: String)(req: Request) =
  req.method == method

private def getHeader(name: String)(req: Request) =
  req.headers(name).toLowerCase

val isGET: Request => Boolean = methodFilter("GET")
val isPOST: Request => Boolean = methodFilter("POST")

val getContentType: Request => String = getHeader("content-type")
val getReferer: Request => String = getHeader("referer")

getReferer(req)
```
<!-- .element: data-id="code" -->

---

<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Class: blueprint of creating instances
- May contain fields and methods

```scala
class Person(first: String, last: String) {
  def greet(): String = s"My name is $first $last!"
}

val matan = new Person("matan", "keidar")
```


<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Object: a class that has *exactly* one instance
- No constructor!

```scala
object Logger {
  def info(msg: String): Unit = println(s"[INFO] $msg")
}

Logger.info("hello there")
```


<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Common pattern: factory methods
- Hide class constructor
- Companion object creates class instances

```scala
class Person private (first: String, last: String) {
  def greet(): String = s"My name is $first $last!"
}

object Person {
  def apply(first: String, last: String) =
    new Person(first = first, last = last)

  object johnDoe = Person(first = "john", last = "doe")
}

val matan = Person("matan", "keidar")
```


<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Common pattern: factory methods
- Hide class constructor
- Companion object creates class instances

```scala[6-7,12]
class Person private (first: String, last: String) {
  def greet(): String = s"My name is $first $last!"
}

object Person {
  def apply(first: String, last: String) =
    new Person(first = first, last = last)

  object johnDoe = Person(first = "john", last = "doe")
}

val matan = Person("matan", "keidar")
```


<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Trait: an interface
- Contains abstract members/methods
- Or concrete methods implementation

```scala
trait Greeter {
  def greet(): String
}

class Person(first: String, last: String) extends Greeter {
  override def greet() = s"Greetings, I'm $first!"
}
```


<!-- .slide: data-auto-animate -->
## Objects <!-- .element: data-id="title" -->
- Common pattern: mixins
- Adding capabilities to classes on demand

```scala
trait Airplane {
  def fly(): Unit
}

trait Car {
  def drive(): Unit
}

class Ford extends Car
class Boeing extends Airplane
class Batmobile extends Car with Airplane
```

---

<!-- .slide: data-auto-animate -->
## Exercise <!-- .element: data-id="title" -->
- Create `Pizza` that has 3 fields:
  - `pizzaSize` (has values Small, Medium, Large)
  - `crustType` (has values Thin, Regular, Thick)
  - `toppings` (list of toppings)
- Create a method `addTopping` (adds another topping)
- Override `toString` in order to print the pizza.

```scala
// Usage example:
val pizza =
    new Pizza(crustType = ThinCrustType, pizzaSize = LargeCrustSize)
      .addTopping(Olives)
      .addTopping(Mushrooms)

  println(pizza)
```

---

<!-- .slide: data-auto-animate -->
## Case classes/objects <!-- .element: data-id="title" -->
- Like regular classes, but great of data modeling
- Already provides us:
  - Immutability and `copy`
  - Constructor
  - `hashCode`and `equals`
  - `toString`
  - Comparator
  - Great for pattern-matching (later today)


<!-- .slide: data-auto-animate -->
## Case classes/objects <!-- .element: data-id="title" -->
```scala
case class HttpRequest(method: String,
                       uri: String,
                       headers: Map[String, String]
                      )

val req1 = HttpRequest("GET", "/hello", Map.empty)

val req2 = req1.copy(uri = "/world",
                     headers = Map("Content-Length" -> 5)
                    )
```


<!-- .slide: data-auto-animate -->
## Exercise <!-- .element: data-id="title" -->
- Create trait `Ordinal[T]` that defines ordering ops:
  - lt, le, gt, gte
  - Trait should be mixed in with any class
- Hint: how many ops must be defined/abstract?
- Bonus: let's define the ops to be: <, >, <=, >=
  - It's just regular operator overloading

```scala
// Usage example:
case class Person(name: String, age: Int) extends Ordinal[Person] {
  ???
}

val grut = Person("Grut", 100)
val spiderMan = Person("Peter Parker", 16)

println(spiderMan < grut)
```

---

## Basic Effects <!-- .element: data-id="title" -->
- Effect: a value with context <!-- .element: class="fragment" -->
- Represents a computation <!-- .element: class="fragment" -->
- Computations can be wrapped and chained <!-- .element: class="fragment" -->
- Provides semantic information as well <!-- .element: class="fragment" -->
- <span style="color:red">F</span>[<span style="color:yellow">T</span>]: Effect of type <span style="color:red">F</span> that returns a value of type <span style="color:yellow">T</span>  <!-- .element: class="fragment" -->



<!-- .slide: data-auto-animate -->
## Basic Effects <!-- .element: data-id="title" -->
### Option[T]
- Represents emptiness, a value that exists or not
- `Some[T]`: an existing value
- `None`: an empty value
```scala
val x: Option[Int] = Some(42)
val y: Option[Int] = None
```


<!-- .slide: data-auto-animate -->
## Basic Effects <!-- .element: data-id="title" -->
### Try[T]
- Represents a computation that can fail
- `Success[T]`: successful result of the computation
- `Failure[Throwable]`: failed computation

```scala
val x: Try[String] = Success("Hello There")
val y: Try[String] = Fail(new RuntimeException("Boom!!!"))

def div(x: Int, y: Int): Try[Int] = Try { x / y }
```


<!-- .slide: data-auto-animate -->
## Basic Effects <!-- .element: data-id="title" -->
### Either[S, T]
- A computation that can return either `S` or `T`
- But NOT both!
- `Right[T]` or `Left[T]`
- Usually, left side represents failure
```scala
val x: Either[Int, String] = Right("Hello There")
val y: Either[Int, String] = Left(-1)
```


<!-- .slide: data-auto-animate -->
## Basic Effects <!-- .element: data-id="title" -->
### Future[T]
- Represents an async computation that can fail

```scala
val x: Future[String] = Future( "hello world")

def div(n: Int) = Future {
  log("failing")
  n / 0
}
```

---

<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->

### Goal <!-- .element: class="fragment" -->
Write defensive code with minimal lines of code <!-- .element: class="fragment" -->

### Question <!-- .element: class="fragment" -->
Call findUser and print the retrieved user if exists <!-- .element: class="fragment" -->

```scala
case class User(
  id: String,
  name: String,
  manager: Option[User] = None
)

def findUser(id: String): Option[User]
```
<!-- .element: class="fragment" -->


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
```scala
// option 1
def findUser(id: String): Option[User]

val maybeUser = findUser("1234")

if (maybeUser.isDefined) {
  // do domething
  val user = maybeUser.get
  println(user)
}
```


## Can we do better?  <!-- .element: data-id="title" -->
<img src="https://media.giphy.com/media/TNO6mwK8s38vpHjh8Y/giphy.gif"> <!-- .element: class="fragment" -->


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
### map
Traverse each element and apply a function
```scala
val myList = List(1,2,3,4)

// myList.map(f: A => B)

myList.map(x => x * 2) // List(2,4,6,8)
myList.map(_ * 2) // same
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
### flatMap
Same as map, but `f` returns value wrapped with context

```scala
val myList = List(1,2,3,4)

myList.flatMap(x => List(x, x)) // List(1,1,2,2,3,3,4,4)
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
### filter
Choose which elements are passed through

```scala
val myList = List(1,2,3,4)

myList.filter(x => x % 2 == 0) // List(2,4)
myList.filter(_ % 2 == 0) // same
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
### reduce
Combine all elements to a single value

```scala
val myList = List(1,2,3,4)

myList.reduce{ case (a, b) => a + b}
myList.reduce(_ + _)
// returns 10
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
```scala
// option 1
def findUser(id: String): Option[User]

val maybeUser = findUser("1234")

if (maybeUser.isDefined) {
  // do domething
  val user = maybeUser.get
  println(user)
}
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
```scala
// option 2
def findUser(id: String): Option[User]

val maybeUser = findUser("1234")

maybeUser.map(user => println(user))
// or
maybeUser.map(println)
// or
findUser("1234").map(println)
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
How to return a managers id list?
```scala
case class User(
  id: String,
  name: String,
  manager: Option[User] = None
)

val avishai = User("1", "avishai")
val nirzo   = User("2", "nirzo")
val user3   = User("3", "Alice", manager = Some(avishai))
val user4   = User("4", "Bob", manager = Some(nirzo))
val userIds = List("1", "2", "3", "4")

def findUser(id: String): Option[User]

val managerIds: List[String] = ???
```


<!-- .slide: data-auto-animate -->
## Basic Functional Programming  <!-- .element: data-id="title" -->
How to return a managers id list?

```scala
userIds
  .flatMap(id => findUser(id))
  .flatMap(user => user.manager)
  .map(manager.id)

userIds
  .flatMap(findUser)
  .flatMap(_.manager)
  .map(_.id)
```

---

## For Comprehension
- Chains of functional combinators are cumbersome <!-- .element: class="fragment" -->
- Syntactic sugar for flatMap/map/filter <!-- .element: class="fragment" -->
- Has to be on the same "Monad" <!-- .element: class="fragment" -->
  - i.e., operate on same type
- Very useful for sequential flows! <!-- .element: class="fragment" -->

```scala
for {
  userId  <- userIds
  user    <- findUser(userId)
  if user.manager.isDefined // just to show if example
  manager <- user.manager
} yield manager.id
```
<!-- .element: class="fragment" -->


## For Comprehension
Can be considered as SQL-like syntax

```scala
val result = for {
  x <- List(1,2,3) // FROM
  if x % 2 == 0    // WHERE
} yield {
  x + 1            // SELECT
}

// result: List(3)
```


## Basic Functional Programming
### Examples
<p float="left">
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala" data-trim>
        List(🐮, 🥔, 🐔, 🌽).map(cook) ==
    </code>
  </pre>
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala fragment" data-trim>
        List(🍔, 🍟, 🍗, 🍿)
    </code>
  </pre>
</p>


## Basic Functional Programming
### Examples
<p float="left">
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala" data-trim>
        List(🍔, 🍟, 🍗, 🍿).filter(isVegan) ==
    </code>
  </pre>
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala fragment" data-trim>
        List(🍟, 🍿)
    </code>
  </pre>
</p>


## Basic Functional Programming
### Examples
<p float="left">
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala" data-trim>
        List(🍔, 🍟, 🍗, 🍿).reduce(eat) ==
    </code>
  </pre>
  <pre style="font-size: 70%; width: fit-content;">
    <code class="Scala fragment" data-trim>
        💩
    </code>
  </pre>
</p>

---

<!-- .slide: data-auto-animate -->
## Pattern Matching <!-- .element: data-id="title" -->
- One of the most powerful Scala features <!-- .element: class="fragment" -->
- Adopted by other languages <!-- .element: class="fragment" -->
- Switch/case on steroids <!-- .element: class="fragment" -->
- Works both on case classes and primitive types <!-- .element: class="fragment" -->


<!-- .slide: data-auto-animate -->
## Pattern Matching <!-- .element: data-id="title" -->
<pre><code class="r-stretch scala" style="padding=50px; height=100%" >
case class User(id: Int, name: String, manager: Option[User])

val nirzo = User(id = 1,   name = "Nirzo", manager = None)
val dima  = User(id = 100, name = "Dima",  manager = None)
val matan = User(id = 101, name = "Matan", manager = Some(dima))

matan match {
  case User(_, "Nirzo", None) =>
    println(s"user $name does not have a manager")

  case User(id, name, _) if id < 3 =>
    println(s"user $name is a founder!")

  case User(_, name, None) =>
    println(s"user $name does not have manager")

  case User(_, name, Some(User(_,manager,_))) =>
    println(s"user $name has manager: $manager")
}
</code></pre>


## Exercise
- Write a function that gets a `WixUser`
- If score > 1000, return "$name you are legend!"
- If name size is exactly 2 chars, return "fake!"
- If user has more than 3 sites, return "$name, you are a premium"
- if name is "bot" and has more than 4 sites, return "rise of the machines"

```scala
case class WixUser(name: String, score: Int, sites: List[String])

def func(user: WixUser): String = ???
```

---

<!-- .slide: data-auto-animate -->
## Implicit Args <!-- .element: data-id="title" -->
As said, a func can have multiple arg list

```scala
def add(x: Int, y: Int) = x + y // call add(1,2)
def add(x: Int)(y: Int) = x + y // call add(1)(2)
```


<!-- .slide: data-auto-animate -->
## Implicit Args <!-- .element: data-id="title" -->

Let's consider the following use case:

```scala
def sendGet(url: String, client: Sttp)
def sendPost(url: String, payload: String, client: Sttp)
def sendPut(url: String, payload: String, client: Sttp)
def sendOption(url: String, client: Sttp)
```

The developer has to *explictly* pass the `client`.
```scala
val client = new Sttp("localhost", 8000)

sendGet("/foo", client)
sendPost("/bar", "hello", client)
```


<!-- .slide: data-auto-animate -->
## Implicit Args <!-- .element: data-id="title" -->

- Boilerplate is contained within implicit args
- Focus on business logic itself

```scala
def sendGet(url: String)(implicit client: Sttp)
def sendPost(url: String, payload: String)(implicit client: Sttp)
def sendPut(url: String, payload: String)(implicit client: Sttp)
def sendOption(url: String)(implicit client: Sttp)

implicit val client = new Sttp("localhost", 8000)

sendGet("/foo")
sendPost("/bar", "hello")
```


<!-- .slide: data-auto-animate -->
## Implicit Functions <!-- .element: data-id="title" -->
- Also known as implicit conversions
- Compiler assistance on steroids

```scala
def func(str: String) = println(s"length = ${str.size}")

val x: Int = 42
func(42) // error! does not type checked
```


<!-- .slide: data-auto-animate -->
## Implicit Functions <!-- .element: data-id="title" -->
```scala
def func(str: String) = println(s"length = ${str.size}")

implicit intToStr(x: Int): String = x.toString

val x: Int = 42
func(42) // output: 2
```


# Very BAD!
# 👿

---

<!-- .slide: data-auto-animate -->
## Extension Methods <!-- .element: data-id="title" -->
- A.K.A Monkey Patching 🐵 <!-- .element: class="fragment" -->
- Provides the best of all worlds: <!-- .element: class="fragment" -->
  - Explicit conversion
  - While having flexibility


<!-- .slide: data-auto-animate -->
## Extension Methods <!-- .element: data-id="title" -->
## How?
  - Create a class that contains the dynamic behavior
  - Import the class to current scope
  - The compiler auto instantiates the wrapper class
  - Wrapper contains the extension logic


<!-- .slide: data-auto-animate -->
## Extension Methods <!-- .element: data-id="title" -->
## Example:
Let's extend `Int` type for creating `'*'` strings
```scala
5.stars // output: "*****"
```


<!-- .slide: data-auto-animate -->
## Extension Methods <!-- .element: data-id="title" -->
```scala
implicit class IntOps(x: Int) {
  def stars = x * "*"
}
```


## Exercise
- Let's revisit the `Ordinal[T]` trait
- Now, let's implement ordinal logic as extension methods
- Meaning, we cannot directly extend any class
  - In our example `Person` class

---

## Big Exercise
<!-- .slide: data-background="https://media.giphy.com/media/UrEQirmnMPxBwToULv/giphy.gif" -->


## Big Exercise
- Clone repo:
  - https://github.com/matankdr/scala-workshop
- Follow readme
- Enjoy!

<img width="300px" src="https://www.wix.com/tools/qr-code-generator/_functions/png/500/000000/FFFFFF/aHR0cHMlM0ElMkYlMkZnaXRodWIuY29tJTJGbWF0YW5rZHIlMkZzY2FsYS13b3Jrc2hvcA==">

---

# Questions <!-- .element: style="color: white; border: 10px; border-color: black;" -->
<!-- .slide: data-background="https://thehomebasedmom.com/wp-content/uploads/2019/05/Frequently-Asked-Questions.jpg"  data-background-opacity="0.7" -->

---

<!-- .slide: data-background="https://media.giphy.com/media/Pnh0Lou03fv92J4puZ/giphy.gif" data-background-size="50%" -->
