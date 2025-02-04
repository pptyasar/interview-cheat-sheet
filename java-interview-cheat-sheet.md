---
title: "Java Cheat Sheet"
weight: 1
description: "Concise Java interview cheatsheet for quick preparation"  # Short description
keywords: 
  - Java
  - Interview
  - Cheatsheet
  - Programming
tags: 
  - Java
---
## Core Java Topics

---

### Basics of Java

****Definition****: Java is a high-level, object-oriented, platform-independent language designed for simplicity, security, and portability.  

***Key Features***:  
1. ***Platform Independence***: Write once, run anywhere (WORA) via bytecode and JVM.  
2. ***Object-Oriented***: Supports encapsulation, inheritance, polymorphism, and abstraction.  
3. ***Robust***: Automatic garbage collection, exception handling, and type checking.  
4. ***Multithreading***: Built-in support for concurrent programming.  
5. ***Secure***: No explicit pointers, bytecode verification, and sandboxing.  

***Example***:  
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); // Runs on any OS with JVM
    }
}
```

---

### JDK, JRE, JVM  
- ***JDK (Java Development Kit)***: Toolkit to develop Java apps (includes JRE + compiler, debugger, etc.).  
- ***JRE (Java Runtime Environment)***: Environment to execute Java bytecode (includes JVM + libraries).  
- ***JVM (Java Virtual Machine)***: Interprets bytecode and runs it on any platform.  

| ***JDK*** | ***JRE*** | ***JVM*** |  
|---------|---------|---------|  
| Used for ***development***. | Used for ***execution***. | Executes bytecode. |  
| Contains JRE + tools. | Contains JVM + libraries. | Part of JRE. |  

***Example***:  
- Compile with JDK: `javac HelloWorld.java`  
- Run with JRE: `java HelloWorld`  

---

### Data Types, Variables, Operators  
***Primitive Data Types***  
 Basic types predefined by Java. Stored directly in memory.  

| Type      | Example Code                     | Description                           |  
|-----------|-----------------------------------|---------------------------------------|  
| `int`     | `int age = 25;`                  | 32-bit integer value.                |  
| `double`  | `double price = 19.99;`          | 64-bit floating-point value.         |  
| `boolean` | `boolean isJavaFun = true;`      | `true` or `false` values.            |  
| `char`    | `char grade = 'A';`              | Single Unicode character.            |  
| `byte`    | `byte b = 100;`                  | 8-bit integer (range: -128 to 127).  |  
| `short`   | `short s = 200;`                 | 16-bit integer (range: -32k to 32k). |  
| `long`    | `long population = 8_000_000_000L;` | 64-bit integer (suffix `L`).       |  
| `float`   | `float temperature = 36.6f;`     | 32-bit floating-point (suffix `f`).  |  

***Non-Primitive Data Types***  
 Objects created from classes. Stored in heap memory.  

| Type          | Example Code                              | Description                          |  
|---------------|-------------------------------------------|--------------------------------------|  
| `String`      | `String name = "Alice";`                  | Sequence of characters (immutable). |  
| Array         | `int[] numbers = {1, 2, 3};`              | Fixed-size collection of elements.  |  
| Object (Class)| `Person person = new Person("Bob", 30);`  | Instance of a user-defined class.    |  
| `ArrayList`   | `ArrayList<String> list = new ArrayList<>();` | Resizable collection.             |  

***Operators***:  
- Arithmetic: `+`, `-`, `*`, `/`  
- Relational: `==`, `>`, `<`  
- Logical: `&&`, `||`, `!`  

| ***Primitive***                          | ***Non-Primitive***                      |  
|----------------------------------------|----------------------------------------|  
| Starts with lowercase (e.g., `int`).   | Starts with uppercase (e.g., `String`).|  
| Fixed size in memory (e.g., `int = 4 bytes`). | Size varies (depends on object). |  
| Can’t invoke methods (e.g., `age.length()` ❌). | Can invoke methods (e.g., `name.length()` ✅). |  
| Default values (e.g., `int` defaults to `0`). | Default to `null`.          |  

---

### Control Flow Statements  
 Statements that control the flow of execution (e.g., conditionals, loops).  
***Types***:  
1. ***Conditional***: `if-else`, `switch`  
   ```java
   if (age >= 18) {
       System.out.println("Adult");
   } else {
       System.out.println("Minor");
   }
   ```  
2. ***Looping***: `for`, `while`, `do-while`  
   ```java
   for (int i = 0; i < 5; i++) {
       System.out.println(i);
   }
   ```  

---
***Arrays***:  
-  Fixed-size collection of elements of the same type.  
- ***Types***: Single-dimensional, multi-dimensional.  
  ```java
  int[] numbers = {1, 2, 3}; // Single-dimensional array
  ```  

***Strings***  
 Immutable sequence of characters (once created, cannot be modified).  
***Storage***: Created in the ***string pool*** (memory-efficient for reuse).  

```java
String s1 = "Hello";       // Stored in string pool  
String s2 = new String("Hello"); // Stored in heap memory  
s1 = s1 + " World";        // Creates a new object ("Hello World"), original "Hello" remains  
```

***Key Points***:  
- Operations like `concat()`, `substring()`, or `toUpperCase()` return ***new objects***.  
- Thread-safe (immutability ensures no data corruption).  


***StringBuilder***  
 Mutable sequence of characters (modifiable).  
***Use Case***: Efficient for frequent string modifications (e.g., loops).  
***Thread Safety***: ***Not thread-safe*** (faster in single-threaded environments).  

```java
StringBuilder sb = new StringBuilder("Java");  
sb.append(" is fun");       // Modifies the same object → "Java is fun"  
sb.reverse();               // Reverses to "nuf si avaJ"  
```

***Key Methods***:  
- `append()`, `insert()`, `delete()`, `reverse()`.  

***StringBuffer***  
 Mutable sequence of characters (legacy class).  
***Use Case***: Thread-safe string modifications (slower due to synchronization).  

```java
StringBuffer buffer = new StringBuffer("Hello");  
buffer.append(" World");    // Modifies the same object → "Hello World"  
```

***Key Methods***: Same as `StringBuilder` but synchronized (e.g., `append()` is thread-safe).  


| Feature              | `String`                  | `StringBuilder`          | `StringBuffer`           |  
|----------------------|---------------------------|--------------------------|--------------------------|  
| ***Mutability***       | Immutable                 | Mutable                  | Mutable                  |  
| ***Thread Safety***    | Thread-safe (immutable)   | Not thread-safe          | Thread-safe (synchronized)|  
| ***Performance***      | Slow for frequent changes | Fast (no synchronization)| Slower (synchronized)    |  
| ***Storage***          | String pool/heap          | Heap                     | Heap                     |  
| ***Use Case***         | Fixed strings             | Single-threaded edits    | Multi-threaded edits     |  

***Example Comparison***  
```java
// String (creates new objects)
String str = "A";  
for (int i = 0; i < 3; i++) {  
    str += i; // New objects: "A0", "A01", "A012"  
}  

// StringBuilder (modifies same object)  
StringBuilder sb = new StringBuilder("A");  
for (int i = 0; i < 3; i++) {  
    sb.append(i); // Result: "A012"  
}  
```

***When to Use***:  
- `String`: For constants or infrequent changes.  
- `StringBuilder`: For single-threaded, high-performance string manipulation.  
- `StringBuffer`: For multi-threaded environments (rarely used in modern Java).  

---

## Object-Oriented Programming (OOP)

***Class***: Blueprint for creating objects.  
***Object***: Instance of a class with state and behavior.  
***Example***:  
```java
class Car { // Class
    String color; // State
    void drive() { // Behavior
        System.out.println("Driving...");
    }
}
Car myCar = new Car(); // Object
```

---

### Inheritance:  
-  Child class inherits properties/methods from a parent class.  
- ***Example***:  
  ```java
  class Animal { }  
  class Dog extends Animal { } // Dog inherits from Animal
  ```  

### Polymorphism:  
-  Same method behaves differently based on the object.  
- ***Types***:  
  - ***Compile-time*** (method overloading).  
  - ***Runtime*** (method overriding).  

### Abstraction:  
-  Hiding implementation details (using abstract classes/interfaces).  
  ```java
  abstract class Shape { abstract void draw(); }  
  ```  

### Encapsulation:  
-  Binding data and methods into a single unit (e.g., using private fields with getters/setters).  
  ```java
  class Student {  
      private String name;  
      public String getName() { return name; }  
  }  
  ```  

---

| ***Overloading*** | ***Overriding*** |  
|------------------|----------------|  
| Same method name, ***different parameters***. | Same method name ***and parameters***. |  
| Occurs in the ***same class***. | Occurs in ***parent-child classes***. |  
| ***Compile-time*** polymorphism. | ***Runtime*** polymorphism. |  

***Example***:  
```java
// Overloading
class Calculator {  
    int add(int a, int b) { return a + b; }  
    double add(double a, double b) { return a + b; }  
}  

// Overriding
class Animal {  
    void sound() { System.out.println("Animal sound"); }  
}  
class Dog extends Animal {  
    void sound() { System.out.println("Bark"); } // Overridden  
}  
```

---

### Constructors  
 Special method to initialize objects.  
***Types***:  
1. ***Default Constructor***: No arguments (auto-generated if none defined).  
2. ***Parameterized Constructor***: Takes arguments.  

***Example***:  
```java
class Book {  
    String title;  
    Book() { title = "Unknown"; } // Default  
    Book(String t) { title = t; } // Parameterized  
}  
Book b1 = new Book();  
Book b2 = new Book("Java 101");  
```

---

| ***this*** | ***super*** |  
|----------|-----------|  
| Refers to the ***current instance***. | Refers to the ***parent class instance***. |  
| Used to resolve variable shadowing. | Used to call parent methods/constructors. |  

***Example***:  
```java
class Parent {  
    Parent() { System.out.println("Parent constructor"); }  
}  
class Child extends Parent {  
    Child() {  
        super(); // Calls Parent's constructor  
        System.out.println("Child constructor");  
    }  
}  
```

***Static***:  
-  Belongs to the class (not instances).  
  ```java
  class Counter {  
      static int count = 0; // Shared across all instances  
  }  
  ```  

***Final***:  
Think of `final` as a way to say: ***"You can’t change this!"***  
It works in 3 ways:  

1. ***Final Variables*** → *"This value is permanent!"*  
   ```java
   final double PI = 3.14; // Like writing in pen ✒️
   // PI = 5.0; // ERROR! Can’t change a final variable
   ```

2. ***Final Methods*** → *"Don’t override this method in a child class!"*  
   ```java
   class Parent {
       final void display() { // Locked method 🔒
           System.out.println("Parent's method");
       }
   }
   
   class Child extends Parent {
       // void display() { } // ERROR! Can’t override final method
   }
   ```

3. ***Final Classes*** → *"No one can inherit from this class!"*  
   ```java
   final class Immutable { } // Closed for inheritance 🚫
   
   // class MyClass extends Immutable { } // ERROR! Can’t extend final class
   ```


- ***Variables***: For constants (like `PI`, `MAX_SIZE`).  
- ***Methods***: To protect critical logic from being changed.  
- ***Classes***: To prevent unwanted extensions (e.g., `String` class is final in Java).  


| Type       | What It Does                              | Example                          |  
|------------|-------------------------------------------|----------------------------------|  
| Variable   | Value can’t change after assignment.      | `final int AGE = 25;`            |  
| Method     | Child classes can’t override it.          | `final void calculate() { ... }` |  
| Class      | No other class can inherit from it.       | `final class MyFinalClass { }`   |  

---

## Exception Handling

| ***Checked Exceptions*** | ***Unchecked Exceptions*** |  
|------------------------|--------------------------|  
| Checked at ***compile time***. | Checked at ***runtime***. |  
| Must be handled using `try-catch` or `throws`. | Subclasses of `RuntimeException`. |  
| Example: `IOException`, `SQLException`. | Example: `NullPointerException`, `ArrayIndexOutOfBoundsException`. |  

***Example***:  
```java
// Checked exception (requires handling)
try {  
    FileReader file = new FileReader("file.txt");  
} catch (IOException e) {  
    e.printStackTrace();  
}  

// Unchecked exception (no compile-time check)
int[] arr = {1, 2};  
System.out.println(arr[3]); // Throws ArrayIndexOutOfBoundsException  
```

---

***Try-catch-finally***  
- ***try***: Block of code to monitor for exceptions.  
- ***catch***: Handles the exception.  
- ***finally***: Always executes (used for cleanup).  

***Example***:  
```java
try {  
    int result = 10 / 0;  
} catch (ArithmeticException e) {  
    System.out.println("Division by zero!");  
} finally {  
    System.out.println("Cleanup done.");  
}  
```


***Custom Exceptions***  User-defined exceptions by extending `Exception` or `RuntimeException`.  
***Example***:  
```java
class InsufficientFundsException extends Exception {  
    InsufficientFundsException(String message) {  
        super(message);  
    }  
}  

// Usage
throw new InsufficientFundsException("Balance too low!");  
```

***Try-with-Resources***  

Imagine you borrow a book from a library.  
***Old way***: You *manually* return it (easy to forget).  
***Try-with-resources***: The book ***automatically returns itself*** when you’re done! 📚✨  

In Java, it’s used for resources like files, databases, or network connections that need to be ***closed*** after use.


***How it works***:  
1. ***Declare the resource*** inside `try( )` → Java takes care of closing it.  
2. No need for `finally` blocks to close resources manually.  

***Example***:  
```java
// Automatically closes the file after the try block
try (FileInputStream file = new FileInputStream("notes.txt")) {
    // Read the file here...
} catch (IOException e) {
    System.out.println("Oops! File error: " + e.getMessage());
}
```


***Why use it?***  
- ***Avoid leaks***: Prevents forgetting to close resources (like leaving a file open).  
- ***Cleaner code***: Removes messy `try-catch-finally` boilerplate.  

***Rule***:  
- The resource must implement `AutoCloseable` (e.g., files, sockets, database connections).  

---

## Java Collections Framework

### ***1. List***  
***What it is***: A ***to-do list*** where order matters and duplicates are allowed.  
***Types & When to Use***:  
- ***`ArrayList`***:  
  - *Think*: Like a notebook with numbered pages.  
  - *Use*: Fast for ***searching*** (`get(3)`), but slower for adding/removing in the middle.  
  ```java
  List<String> groceries = new ArrayList<>();  
  groceries.add("Milk"); // ["Milk"]
  ```

- ***`LinkedList`***:  
  - *Think*: Like a chain of friends holding hands.  
  - *Use*: Fast for ***adding/removing*** items at the start/end (e.g., playlists).  
  ```java
  LinkedList<String> playlist = new LinkedList<>();  
  playlist.addFirst("Song1"); // Adds to the front
  ```

---

### ***2. Set***  
***What it is***: A ***unique collection*** (no duplicates), like a bag of distinct keys.  
***Types & When to Use***:  
- ***`HashSet`***:  
  - *Think*: Jumbled keys in a bowl.  
  - *Use*: Fastest for checking if an item exists (`contains()`).  
  ```java
  Set<String> usernames = new HashSet<>();  
  usernames.add("Alice"); // Ignores duplicates
  ```

- ***`TreeSet`***:  
  - *Think*: Keys sorted alphabetically.  
  - *Use*: When you need items ***sorted*** (e.g., leaderboard).  
  ```java
  TreeSet<Integer> scores = new TreeSet<>();  
  scores.add(85); // Automatically sorted
  ```

---

### ***3. Map***  
***What it is***: ***Key-value pairs***, like a dictionary (word = key, definition = value).  
***Types & When to Use***:  
- ***`HashMap`***:  
  - *Think*: Randomly arranged dictionary entries.  
  - *Use*: Fast lookups by key (e.g., user database).  
  ```java
  Map<String, Integer> ages = new HashMap<>();  
  ages.put("Alice", 30); // Key = "Alice", Value = 30
  ```

- ***`TreeMap`***:  
  - *Think*: Dictionary sorted by words.  
  - *Use*: When keys need to be ***sorted*** (e.g., phonebook).  

---

### ***4. Queue***  
***What it is***: A ***line of people*** (FIFO: First-In-First-Out).  
***Types & When to Use***:  
- ***`LinkedList` (as Queue)***:  
  - *Think*: A grocery store checkout line.  
  - *Use*: Processing tasks in order (e.g., printing jobs).  
  ```java
  Queue<String> tasks = new LinkedList<>();  
  tasks.add("Print");  
  String nextTask = tasks.poll(); // Removes the first task
  ```

- ***`PriorityQueue`***:  
  - *Think*: VIP line (higher priority goes first).  
  - *Use*: Emergency room (critical patients first).  
  ```java
  PriorityQueue<Integer> emergencies = new PriorityQueue<>();  
  emergencies.add(5); // Lower number = higher priority
  ```

---

| ***Collection*** | ***Order***       | ***Duplicates*** | ***Use Case***                      |  
|-----------------|-----------------|----------------|-----------------------------------|  
| `ArrayList`     | Yes (by index)  | Yes            | Frequent access by position.      |  
| `HashSet`       | No              | No             | Check if an item exists quickly.  |  
| `HashMap`       | No              | Unique keys    | Fast key-value lookups.           |  
| `LinkedList`    | Yes (insertion) | Yes            | Frequent add/remove at ends.      |  


***When to Use Which?***  
- ***Need order + duplicates?*** → `ArrayList`/`LinkedList`.  
- ***Need uniqueness?*** → `HashSet`/`TreeSet`.  
- ***Key-value pairs?*** → `HashMap`/`TreeMap`.  
- ***Process in FIFO order?*** → `Queue`/`PriorityQueue`.  

***Example***:  
- ***Social Media Posts*** → Use `ArrayList` (order matters).  
- ***Unique User IDs*** → Use `HashSet`.  
- ***User Profiles (ID → Details)*** → Use `HashMap`.  

## Multithreading and Concurrency


### Creating Threads  
***Types***:  
1. ***Extend `Thread` class***:  
   ```java
   class MyThread extends Thread {
       public void run() {
           System.out.println("Thread running");
       }
   }
   new MyThread().start();
   ```  
2. ***Implement `Runnable`***:  
   ```java
   Runnable task = () -> System.out.println("Runnable task");
   new Thread(task).start();
   ```  

| ***Extending Thread*** | ***Implementing Runnable*** |  
|-----------------------|---------------------------|  
| Less flexible (Java doesn’t support multiple inheritance). | More flexible (can implement multiple interfaces). |  

### Life Cycle of Threads  

***1. New***  
***State***: Thread is created but not started.  
***Method***:  
- `new Thread()` → Creates a thread in the ***New*** state.  

```java
Thread thread = new Thread(() -> System.out.println("Running"));
// Thread is in NEW state
```

---

***2. Runnable***  
***State***: Thread is ready to run (may or may not be executing).  
***Methods***:  
- `start()` → Transitions from ***New*** to ***Runnable***.  
- `yield()` → Suggests the scheduler to pause the thread (voluntarily).  

```java
thread.start(); // Moves to RUNNABLE state
```

---

***3. Blocked/Waiting***  
***State***: Thread is paused, waiting for a resource or signal.  

***Blocked*** (Waiting for a monitor lock)  
***Methods***:  
- Entering a `synchronized` block/method → If the lock is held by another thread.  

```java
synchronized (lock) { // Thread BLOCKED if lock is held by another thread
    // Critical section
}
```

***Waiting*** (Indefinite pause)  
***Methods***:  
- `wait()` → Releases the lock and waits for `notify()`/`notifyAll()` (from `Object` class).  
- `join()` → Waits for another thread to finish.  
- `LockSupport.park()` → Pauses the thread indefinitely.  

```java
synchronized (lock) {
    lock.wait(); // Moves to WAITING state
}
```

---

***4. Timed Waiting***  
***State***: Thread pauses for a specific time.  
***Methods***:  
- `sleep(long millis)` → Sleeps for a fixed time.  
- `wait(long timeout)` → Waits with a timeout.  
- `join(long millis)` → Waits for another thread with a timeout.  
- `LockSupport.parkNanos(long nanos)` → Parks with a timeout.  

```java
Thread.sleep(1000); // Moves to TIMED_WAITING state for 1 second
```

---

***5. Terminated***  
***State***: Thread completes execution or is terminated.  
***Methods***:  
- Natural exit when `run()` finishes.  
- `stop()` → ***Deprecated*** (unsafe, avoids using).  

```java
// Thread terminates after run() completes
Thread thread = new Thread(() -> { /* Task */ });
thread.start(); // Ends in TERMINATED state after execution
```

---

***Key Methods & Transitions***  
| ***State***       | ***Trigger Methods***                     | ***Transition***                          |  
|------------------|-----------------------------------------|------------------------------------------|  
| ***New***          | `new Thread()`                          | Created but inactive.                    |  
| ***Runnable***     | `start()`                               | Ready to execute.                        |  
| ***Blocked***      | Contention in `synchronized` block      | Waiting for a lock.                      |  
| ***Waiting***      | `wait()`, `join()`                      | Waiting indefinitely.                    |  
| ***Timed Waiting***| `sleep()`, `wait(timeout)`, `join(time)`| Waiting with a timeout.                  |  
| ***Terminated***   | `run()` completes                       | Thread exits.                            |  

---

***Example Workflow***  
```java
Thread thread = new Thread(() -> {
    synchronized (lock) {
        try {
            lock.wait(2000); // TIMED_WAITING for 2 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
});
thread.start(); // NEW → RUNNABLE
```

---

***Avoid Deprecated Methods***  
- ***`stop()`***, ***`suspend()`***, ***`resume()`*** → Unsafe and deprecated. Use ***interrupts*** or ***flags*** instead.  

```java
// Safe termination using a flag
volatile boolean running = true;
public void run() {
    while (running) { /* Task */ }
}
``` 

---

### Synchronization and Locking  
 Preventing multiple threads from accessing shared resources simultaneously.  
***Types***:  
1. ***Synchronized Methods***:  
   ```java
   public synchronized void increment() { count++; }
   ```  
2. ***Synchronized Blocks***:  
   ```java
   synchronized (this) { count++; }
   ```  
3. ***Locks (e.g., `ReentrantLock`)***:  
   ```java
   Lock lock = new ReentrantLock();
   lock.lock();
   try { count++; } finally { lock.unlock(); }
   ```  

---

***Deadlock***: Two threads block each other by holding resources the other needs.  
***Race Condition***: Unpredictable behavior when threads access shared data concurrently.  

***Example (Deadlock)***:  
```java
// Thread 1
synchronized (A) { synchronized (B) { ... } }

// Thread 2
synchronized (B) { synchronized (A) { ... } }
```


***Thread Pools*** Reuse a group of threads to execute tasks efficiently.  
***Types***:  
1. ***FixedThreadPool***: Fixed number of threads.  
   ```java
   ExecutorService executor = Executors.newFixedThreadPool(4);
   ```  
2. ***CachedThreadPool***: Creates threads as needed.  
3. ***ScheduledThreadPool***: Executes tasks after a delay.  

***Example***:  
```java
executor.submit(() -> System.out.println("Task executed"));
executor.shutdown();
```

- ***`volatile`***: Ensures variable changes are visible across threads.  
  ```java
  private volatile boolean flag = true;
  ```  
- ***`AtomicInteger`***: Provides atomic operations (e.g., `incrementAndGet()`).  
  ```java
  AtomicInteger count = new AtomicInteger(0);
  count.incrementAndGet();
  ```  

***Difference***:  
| ***volatile*** | ***Atomic Variables*** |  
|--------------|----------------------|  
| Solves visibility, not atomicity. | Solve both visibility and atomicity. |  

***Wait, Notify, NotifyAll***  
Think of these as ***walkie-talkie commands*** for threads:  

1. ***`wait()`*** → "I'll pause here. Wake me up when something changes!"  
   - Releases the lock and sleeps.  
   ```java
   synchronized (lock) {
       while (conditionNotMet) {
           lock.wait(); // Sleeps until notify()/notifyAll()
       }
   }
   ```

2. ***`notify()`*** → "Hey, one of you can wake up now!"  
   - Wakes ***one*** random waiting thread.  

3. ***`notifyAll()`*** → "Everyone, wake up and check!"  
   - Wakes ***all*** waiting threads.  

| `notify()` | `notifyAll()` |  
|------------|---------------|  
| Wakes ***1 thread*** | Wakes ***all threads*** |  
| Risk of missing the right thread | Safer for complex logic |  

***When to Use***:  
- `notify()`: When only ***one*** waiting thread can proceed (e.g., single resource).  
- `notifyAll()`: When ***multiple*** threads might need to react (e.g., multiple conditions).  

***Example***:  
```java
// Producer-Consumer Example
synchronized (buffer) {
    while (buffer.isFull()) {
        buffer.wait(); // Producer waits
    }
    buffer.add(item);
    buffer.notifyAll(); // Wake up all consumers
}
```  

***Rule***: Always use `wait()` in a ***loop*** (to handle accidental wakeups).

---

## Java I/O and NIO

| ***Byte Streams*** | ***Character Streams*** |  
|-------------------|-----------------------|  
| Handle binary data (e.g., images). | Handle text data. |  
| Classes: `FileInputStream`, `FileOutputStream`. | Classes: `FileReader`, `FileWriter`. |  

***Example (Byte Stream)***:  
```java
try (FileInputStream fis = new FileInputStream("file.jpg")) {
    int data;
    while ((data = fis.read()) != -1) { /* process byte */ }
}
```


### File Handling  
***Reading a File***:  
```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) System.out.println(line);
}
```

---

### Serialization and Deserialization  
 Convert objects to bytes (persist/send over network) and reconstruct them.  
```java
// Serialization
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("data.ser"))) {
    oos.writeObject(new Person("Alice"));
}

// Deserialization
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("data.ser"))) {
    Person p = (Person) ois.readObject();
}
```

---

### Java NIO  
 Non-blocking I/O API with buffers and channels.  
***Key Classes***:  
- `Path`: Represents a file path.  
- `Files`: Utility methods for file operations.  
- `Channels`: For reading/writing data.  

***Example (Read File with NIO)***:  
```java
Path path = Paths.get("file.txt");
List<String> lines = Files.readAllLines(path);
```