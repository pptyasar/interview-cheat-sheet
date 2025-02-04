---
title: "Java 8+ Cheat Sheet"
weight: 2
description: "Concise Java interview cheatsheet for quick preparation"  # Short description
keywords: 
  - Java
  - Interview
  - Cheatsheet
  - Programming
tags: 
  - Java
---


## Java 8 Features

### Lambda Expressions  

***Why Use Them?***  
1. ***Simplify Code***: Replace bulky anonymous classes with 1-line logic.  
2. ***Readability***: Make functional operations (like loops or filters) clear and concise.  
3. ***Functional Programming***: Enable passing behavior as data (e.g., "print this" or "sort that").  


***Example Use Cases***  
1. ***Looping Through a List***:  
   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   // Old way (anonymous class):
   names.forEach(new Consumer<String>() {
       public void accept(String name) {
           System.out.println(name);
       }
   });
   // Lambda way (shorter!):
   names.forEach(name -> System.out.println(name));
   ```

2. ***Sorting with Custom Logic***:  
   ```java
   // Sort by string length
   names.sort((s1, s2) -> s1.length() - s2.length());
   ```

3. ***Thread Initialization***:  
   ```java
   // Old way:
   new Thread(new Runnable() {
       public void run() {
           System.out.println("Running!");
       }
   }).start();
   
   // Lambda way:
   new Thread(() -> System.out.println("Running!")).start();
   ```

***Key Points***  
- ***Syntax***: `(parameters) -> { code }`  
  - If one parameter and one line: `n -> System.out.println(n)`  
- ***Works With***: Functional interfaces (interfaces with ***one abstract method*** like `Runnable`, `Comparator`).  

***Why They’re Needed***  
Imagine telling a friend to *"sort these books by color"* instead of giving a 10-step instruction manual.  
***Lambdas do the same for code***—they let you express *what to do* (e.g., print, sort, filter) without the *how* boilerplate.  

***Real-World Use***:  
- Processing data (e.g., filter a list of users by age).  
- Event handling (e.g., button clicks in GUIs).  
- Parallel programming (e.g., using streams for bulk operations).  

```java
// Filter users older than 18
List<User> adults = users.stream()
                         .filter(user -> user.getAge() >= 18)
                         .toList();
``` 

---

### Functional Interfaces  
 An interface with ***exactly one abstract method*** (SAM). Marked with `@FunctionalInterface`.  
***Built-in Functional Interfaces***:  

| Interface       | Abstract Method      | Purpose                            | Example |  
|-----------------|----------------------|------------------------------------|---------|  
| `Predicate<T>`  | `boolean test(T t)`  | Tests a condition.                 | `Predicate<Integer> isEven = n -> n % 2 == 0;` |  
| `Function<T,R>` | `R apply(T t)`       | Transforms input to output.        | `Function<String, Integer> length = s -> s.length();` |  
| `Consumer<T>`   | `void accept(T t)`   | Consumes input (no return).        | `Consumer<String> print = s -> System.out.println(s);` |  
| `Supplier<T>`   | `T get()`            | Supplies a value (no input).       | `Supplier<Double> random = Math::random;` |  
| `Runnable`      | `void run()`         | Represents a task to execute.      | `Runnable task = () -> System.out.println("Running");` |  

***Example***:  
```java
@FunctionalInterface
interface Greeting {
    void greet(String name); // Single abstract method
}
Greeting hello = name -> System.out.println("Hello " + name);
```


***1. Default Methods***  
***What***: A method with a ***default implementation*** in the interface.  
***Why Needed***: To add new features to old interfaces *without breaking existing code*.  

***Example***:  
```java
interface Vehicle {
    // Old rule: All vehicles must implement this
    void start(); 
    
    // New default method (optional to override)
    default void honk() {
        System.out.println("Beep beep!"); 
    }
}

class Car implements Vehicle {
    public void start() { 
        System.out.println("Car started"); 
    }
    // Honk uses the default implementation
}

// Usage:
Car car = new Car();
car.honk(); // Output: "Beep beep!" (no need to define honk() in Car)
```

***Why Use***:  
- Avoid forcing all classes to implement new methods (backward compatibility).  
- Share common code across multiple classes (e.g., logging).  

---

***2. Static Methods***  
***What***: A method ***belonging to the interface itself*** (not instances).  
***Why Needed***: For utility functions related to the interface.  

***Example***:  
```java
interface MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}

// Usage:
int sum = MathUtils.add(5, 3); // 8 (called via interface name)
```

***Why Use***:  
- Keep helper methods ***organized*** under the interface (no need for a separate utility class).  
- Example: `Comparator.comparing()` is a static method in the `Comparator` interface.  


| ***Default Method***              | ***Static Method***               |  
|----------------------------------|----------------------------------|  
| Called on ***instances*** of a class. | Called via ***interface name***. |  
| Can be ***overridden*** by classes. | ***Cannot*** be overridden.       |  

---

### Stream API  
 A sequence of elements supporting sequential/parallel operations.  
***Two Types of Operations***:  
1. ***Intermediate Methods***:  
   - Allow chaining operations (e.g., filter → map → sort) for clean, readable code.  
   - Optimize performance (e.g., `limit(5)` stops processing after 5 elements).  

2. ***Terminal Methods***:  
   - Trigger the actual processing of the stream (nothing happens until a terminal method is called).  
   - Produce results (e.g., a list, sum, or boolean).  


***Intermediate Methods***  
 Transform or filter data *without producing a final result* (lazy evaluation).  

| Method                | Purpose                                      | Example                                   |  
|-----------------------|----------------------------------------------|-------------------------------------------|  
| `filter(Predicate)`   | Keep elements matching a condition.         | `stream.filter(n -> n > 10)`             |  
| `map(Function)`       | Transform elements (e.g., `String` → `int`).| `stream.map(s -> s.length())`             |  
| `flatMap(Function)`   | Flatten nested streams (e.g., lists → elements). | `stream.flatMap(list -> list.stream())` |  
| `sorted()`            | Sort elements (natural order or custom).    | `stream.sorted()`                        |  
| `distinct()`          | Remove duplicates.                          | `stream.distinct()`                      |  
| `peek(Consumer)`      | Debug/log elements without modifying them.  | `stream.peek(System.out::println)`       |  
| `limit(long)`         | Keep only the first `n` elements.           | `stream.limit(5)`                        |  
| `skip(long)`          | Skip the first `n` elements.                | `stream.skip(2)`                         |  

***Terminal Methods***  
 Produce a result or side-effect *and close the stream*.  

| Method                | Purpose                                      | Example                                   |  
|-----------------------|----------------------------------------------|-------------------------------------------|  
| `collect(Collector)`  | Convert stream to a collection (e.g., `List`, `Map`). | `stream.collect(Collectors.toList())` |  
| `forEach(Consumer)`   | Perform an action on each element.          | `stream.forEach(System.out::println)`    |  
| `reduce()`            | Combine elements into a single value (e.g., sum). | `stream.reduce(0, (a, b) -> a + b)`    |  
| `count()`             | Count elements in the stream.               | `long count = stream.count()`            |  
| `anyMatch(Predicate)` | Check if ***any*** element matches a condition. | `boolean hasEven = stream.anyMatch(n -> n % 2 == 0)` |  
| `allMatch(Predicate)` | Check if ***all*** elements match a condition. | `boolean allPositive = stream.allMatch(n -> n > 0)` |  
| `findFirst()`         | Get the ***first*** element (if exists).      | `Optional<String> first = stream.findFirst()` |  
| `min()/max()`         | Find the smallest/largest element.          | `Optional<Integer> min = stream.min()`   |  

***Example Workflow***  
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = numbers.stream()
    .filter(n -> n % 2 == 0)  // Keep even numbers (2, 4)
    .map(n -> n * 2)          // Double them (4, 8)
    .reduce(0, Integer::sum); // Sum the result (12)

System.out.println(sum); // Output: 12
```

***Key Takeaway***:  
- Use ***intermediate*** methods to build a pipeline.  
- Use ***terminal*** methods to execute it and get results.  
- Streams make data processing ***declarative*** (focus on *what*, not *how*).  


***Example***:  
```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5);
List<Integer> processed = numbers.stream()
    .filter(n -> n > 2)         // Intermediate
    .map(n -> n * 2)            // Intermediate
    .sorted()                   // Intermediate
    .collect(Collectors.toList()); // Terminal
System.out.println(processed); // [6, 8, 10]
```

***Example Workflow***:  
```java
// Find a user and get their uppercase name, or return "GUEST"
String username = userRepository.findById(1)
    .map(User::getName)       // Transform User → String
    .filter(name -> !name.isEmpty()) // Check name isn't empty
    .map(String::toUpperCase) // Convert to uppercase
    .orElse("GUEST");         // Default if any step fails
```  

---

### Optional Class  

Think of `Optional` as a ***gift box*** 🎁: It can either hold a value (`Optional.of("Gift")`) or be empty (`Optional.empty()`). Use it to avoid `null` checks and handle missing values safely.


***Key Methods & One-Line Examples***  
| ***Method***               | ***What It Does***                                | ***Example***                                                                 |  
|--------------------------|------------------------------------------------|-----------------------------------------------------------------------------|  
| `Optional.of(value)`     | Creates a box with a ***non-null*** value.       | `Optional<String> box = Optional.of("Hello");`                             |  
| `Optional.ofNullable()`  | Creates a box that ***may be empty*** (if null). | `Optional<String> box = Optional.ofNullable(getName());`                   |  
| `isPresent()`             | Checks if the box has a value.                 | `if (box.isPresent()) { ... }`                                             |  
| `ifPresent(Consumer)`     | Do something if the box has a value.           | `box.ifPresent(name -> System.out.println(name));`                         |  
| `orElse(T)`               | Get the value or a default.                    | `String result = box.orElse("Unknown");`                                   |  
| `orElseGet(Supplier)`     | Get the value or compute a default.            | `String result = box.orElseGet(() -> fetchDefault());`                     |  
| `orElseThrow()`           | Throw an exception if empty.                   | `String value = box.orElseThrow(() -> new RuntimeException("Missing!"));` |  
| `map(Function)`           | Transform the value (e.g., `String` → `int`).  | `Optional<Integer> length = box.map(String::length);`                      |  
| ***`filter(Predicate)`***   | Check a condition on the value.                | `Optional<String> longName = box.filter(n -> n.length() > 5);`             |  
| ***`flatMap(Function)`***   | Transform and flatten nested `Optional`.       | `Optional<String> upper = box.flatMap(s -> Optional.of(s.toUpperCase()));` |  

***`Optional.of(value)`***  
- ***Use Case***: When you ***know the value is NOT `null`***.  
- ***Behavior***: Throws ***`NullPointerException`*** if you pass `null`. 
- ***Avoid `NullPointerException`***: No more `if (x != null)` checks!  
- ***Clear Code***: Shows intent that a value might be missing.  
- ***Chain Operations***: Safely transform values with `map`, `filter`, etc.  
- ***Example***:  
  ```java
  String name = "Alice";
  Optional<String> box = Optional.of(name); // ✅ Works (name is NOT null)
  
  String nullName = null;
  Optional<String> brokenBox = Optional.of(nullName); // ❌ Throws NPE!
  ```  
  → Use this only when you’re ***100% sure the value is NOT null***.


***`Optional.ofNullable(value)`***  
- ***Use Case***: When the value ***might be `null`***.  
- ***Behavior***: Returns an ***empty `Optional`*** if the value is `null`.  
- ***Example***:  
  ```java
  String name = getUserName(); // Could return "Bob" or null
  Optional<String> safeBox = Optional.ofNullable(name); // ✅ Works even if name is null!
  
  if (safeBox.isPresent()) {
      System.out.println(safeBox.get()); // Prints value if present
  } else {
      System.out.println("Name is missing!"); // Handles null case
  }
  ```  
  → Use this when you ***aren’t sure*** if the value is null (common case).

---

***Key Difference***  
| ***`Optional.of()`*** | ***`Optional.ofNullable()`*** |  
|----------------------|-----------------------------|  
| ***Crashes*** if you pass `null`. | ***Handles `null` safely*** (returns empty box). |  
| Use for ***guaranteed non-null values***. | Use for ***values that might be null***. |  

---

***When to Use Which***  
- Use `Optional.of()` for ***constants*** or ***known non-null values***:  
  ```java
  Optional<String> greeting = Optional.of("Hello"); // Safe because "Hello" is not null
  ```  
- Use `Optional.ofNullable()` for ***method returns*** or ***variables that might be null***:  
  ```java
  Optional<String> userInput = Optional.ofNullable(request.getParameter("name")); // HTTP param could be null
  ```  
****Rule****: Never use `Optional.get()` without checking `isPresent()`! Use `orElse`/`orElseThrow` instead.
 
---

### Method References  
 Shorthand syntax for calling existing methods via lambdas.  
***4 Types***:  
1. ***Static Method***: `ClassName::staticMethod`  
   ```java
   Function<Integer, String> converter = String::valueOf; // String.valueOf(n)
   ```  
2. ***Instance Method of Object***: `object::instanceMethod`  
   ```java
   List<String> names = Arrays.asList("A", "B");
   names.forEach(System.out::println); // System.out.println(s)
   ```  
3. ***Instance Method of Arbitrary Object***: `ClassName::instanceMethod`  
   ```java
   Function<String, String> upper = String::toUpperCase; // s.toUpperCase()
   ```  
4. ***Constructor***: `ClassName::new`  
   ```java
   Supplier<List<String>> listSupplier = ArrayList::new; // new ArrayList()
   ```  

---

## Java Annotations

***Built-in Annotations***  
1. ***`@Override`***: Indicates method overriding.  
2. ***`@Deprecated`***: Marks obsolete code.  
3. ***`@SuppressWarnings`***: Suppresses compiler warnings.  
4. ***`@FunctionalInterface`***: Ensures an interface is a functional interface.  

***Example***:  
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b); // SAM
    default void log() { }        // Allowed: default method
}
```

---

## Java Modules (Java 9+)

***Module System (JPMS)***
 The Java Platform Module System (JPMS) modularizes the JDK and allows developers to create modular applications. Modules explicitly declare dependencies and exported packages.

***Example***:
```java
// module-info.java
module com.example.myapp {
    requires java.sql;
    exports com.example.myapp.api;
}
```

***module-info.java***
 A configuration file that defines a module’s name, dependencies (`requires`), and exported packages (`exports`).

---

## Java Memory Management


- ***Heap***: Stores objects and JRE classes. Shared across threads.
- ***Stack***: Stores method calls and local variables. Thread-specific.

- Heap is for objects; Stack for method execution.
- Heap is GC-managed; Stack auto-cleared post-method return.

***Example***:
```java
void method() {
    int x = 5; // Stack
    Object obj = new Object(); // Heap
}
```

***Garbage Collection (GC) Algorithms***
***Types***:
1. ***Serial GC***: Single-threaded; for small apps.
2. ***Parallel GC***: Multi-threaded; throughput-focused.
3. ***G1 GC***: Balances latency/throughput; divides heap into regions.
4. ***ZGC***: Low-latency; handles large heaps.

***Example*** (Enable G1 GC):
```bash
java -XX:+UseG1GC MyApp
```

### Memory Leaks and OutOfMemoryError
 Unintentional object retention preventing GC. Causes `OutOfMemoryError`.

***Example***:
```java
List<Object> list = new ArrayList<>();
while (true) list.add(new Object()); // Leak
```

---

## Java Reflection API

***Dynamic Class Loading***
 Load classes at runtime using `Class.forName()`.

***Example***:
```java
Class<?> clazz = Class.forName("java.util.ArrayList");
Object list = clazz.getDeclaredConstructor().newInstance();
```

### Accessing Private Methods and Fields
***Methods***: `getDeclaredField()`, `setAccessible(true)`.

***Example***:
```java
Field field = MyClass.class.getDeclaredField("secret");
field.setAccessible(true);
field.set(obj, "newSecret");
```

---

## Java Generics

***Generic Classes, Methods, and Interfaces***
***Example (Class)***:
```java
class Box<T> {
    private T content;
    public void set(T content) { this.content = content; }
}
Box<String> box = new Box<>();
```

***Example (Method)***:
```java
<T> void print(T item) { System.out.println(item); }
```

***Type Erasure***
 Generics are removed at runtime. `Box<String>` becomes `Box<Object>`.

***Bounded***:
```java
<T extends Number> void process(T num) { /* ... */ }
```

***Wildcard***:
```java
List<? extends Number> numbers = new ArrayList<Double>();
```

---

## Java Networking

***Socket Programming***
 TCP communication using `Socket` (client) and `ServerSocket` (server).

***Example (Server)***:
```java
try (ServerSocket server = new ServerSocket(8080);
     Socket client = server.accept();
     PrintWriter out = new PrintWriter(client.getOutputStream(), true)) {
    out.println("Hello from server!");
}
```

***HTTP/HTTPS Communication***
***Example (Java 11+ HttpClient)***:
```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://example.com"))
        .build();
HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
```

***URL and URLConnection Classes***
***Example***:
```java
URL url = new URL("https://example.com");
URLConnection conn = url.openConnection();
try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
    String line;
    while ((line = reader.readLine()) != null) System.out.println(line);
}
```

```markdown
## Java Database Connectivity (JDBC)

***CRUD Operations***
 CRUD (Create, Read, Update, Delete) operations are fundamental database interactions.  
***Methods***:
- `executeQuery()`: For READ (SELECT).
- `executeUpdate()`: For CREATE (INSERT), UPDATE, DELETE.

***Example***:
```java
// INSERT (Create)
Statement stmt = connection.createStatement();
int rows = stmt.executeUpdate("INSERT INTO users VALUES (1, 'John')");

// SELECT (Read)
ResultSet rs = stmt.executeQuery("SELECT * FROM users");
```

***PreparedStatement and CallableStatement***
- `PreparedStatement`: Parameterized SQL queries (prevents SQL injection).
- `CallableStatement`: Executes stored procedures.

***Example***:
```java
// PreparedStatement
PreparedStatement ps = connection.prepareStatement("INSERT INTO users VALUES (?, ?)");
ps.setInt(1, 2);
ps.setString(2, "Alice");
ps.executeUpdate();

// CallableStatement
CallableStatement cs = connection.prepareCall("{call get_user(?)}");
cs.setInt(1, 1);
cs.execute();
```

***Connection Pooling***
 Reuses database connections to reduce overhead.  
***Libraries***: HikariCP, Apache DBCP.  
***Example***:
```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
HikariDataSource ds = new HikariDataSource(config);
Connection conn = ds.getConnection(); // Reusable connection
```

---

## Java Design Patterns

***Creational Patterns***
 Handle object creation.  
***Types***: Singleton, Factory, Builder.  
***Example (Singleton)***:
```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

***Structural Patterns***
 Organize classes/objects into larger structures.  
***Types***: Adapter, Decorator, Facade.  
***Example (Adapter)***:
```java
interface Printer { void print(String text); }
class LegacyPrinter {
    void printDocument(String text) { System.out.println(text); }
}
class PrinterAdapter implements Printer {
    private LegacyPrinter legacyPrinter = new LegacyPrinter();
    public void print(String text) { legacyPrinter.printDocument(text); }
}
```

***Behavioral Patterns***
 Manage object collaboration.  
***Types***: Observer, Strategy, Command.  
***Example (Observer)***:
```java
interface Observer { void update(String message); }
class NewsAgency {
    private List<Observer> observers = new ArrayList<>();
    void addObserver(Observer o) { observers.add(o); }
    void notifyObservers(String msg) { observers.forEach(o -> o.update(msg)); }
}
```

---

# Java Performance Tuning

***JVM Tuning***
***Parameters***:
- `-Xmx`: Max heap size (e.g., `-Xmx512m`).
- `-XX:+UseG1GC`: Use G1 garbage collector.

***Example Command***:
```
java -Xmx1024m -Xms256m -XX:+UseG1GC MyApp
```

***Profiling Tools***
***Tools***: VisualVM, YourKit, JProfiler.  
***Use Case***: Analyze memory leaks, CPU usage.

---

# Java Security

### Java Security Manager ###
 Restricts JVM operations via policies (deprecated in Java 17).  
***Example Policy File***:
```
grant {
    permission java.io.FilePermission "/tmp/*", "read,write";
};
```

### Cryptography in Java
***APIs***: `javax.crypto` (e.g., AES encryption).  
***Example***:
```java
Cipher cipher = Cipher.getInstance("AES");
SecretKey key = KeyGenerator.getInstance("AES").generateKey();
cipher.init(Cipher.ENCRYPT_MODE, key);
byte[] encrypted = cipher.doFinal("Secret".getBytes());
```

---

# Java 9 to Java 17 Features

***JShell (Java 9)**
 REPL tool for quick code testing.  
***Example***:
```bash
jshell> int sum = 2 + 3;
sum ==> 5
```

***Local Variable Type Inference (Java 10)***
 Use `var` for type inference.  
***Example***:
```java
var list = new ArrayList<String>(); // Inferred as ArrayList<String>
```

***HTTP Client API (Java 11)***
 Non-blocking HTTP client.  
***Example***:
```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder().uri(URI.create("https://example.com")).build();
HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
```

***Switch Expressions (Java 12)***
 Simplify `switch` syntax.  
***Example***:
```java
String dayType = switch (day) {
    case "Mon", "Tue" -> "Weekday";
    default -> "Unknown";
};
```

***Text Blocks (Java 13)***
 Multi-line strings with `"""`.  
***Example***:
```java
String json = """
    {
        "name": "John",
        "age": 30
    }
""";
```

***Records (Java 14)***
 Immutable data classes.  
***Example***:
```java
record Person(String name, int age) {}
Person p = new Person("Alice", 25);
```

***Sealed Classes (Java 15)***
 Restrict class inheritance.  
***Example***:
```java
public sealed class Shape permits Circle, Square {}
```

***Pattern Matching for switch (Java 17)***
 Type patterns in `switch`.  
***Example***:
```java
Object obj = "Hello";
switch (obj) {
    case String s -> System.out.println(s.length());
    default -> System.out.println("Unknown");
}
```