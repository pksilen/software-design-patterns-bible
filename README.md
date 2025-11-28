# software-design-patterns-bible

# Result pattern (for error handling, like in Rust)


## Decorator pattern

Yes — **Java’s `BufferedReader` is a classic example of the *Decorator Pattern*.**

Here’s why:

### 1. Structure of the Decorator Pattern

The **Decorator Pattern** lets you dynamically add responsibilities to objects without modifying their code.
It involves:

* A **component interface** (e.g., `Reader`).
* A **concrete component** that implements the interface (e.g., `FileReader`, `InputStreamReader`).
* A **decorator** that also implements the interface and *wraps* another component (e.g., `BufferedReader`).

### 2. How `BufferedReader` fits

* **Component**: `Reader` (abstract class) defines the common API: `read()`, `close()`, etc.
* **Concrete Component**: `FileReader`, `CharArrayReader`, etc. actually perform character input.
* **Decorator**: `BufferedReader` extends `Reader`, but internally holds another `Reader` reference. It enhances it by adding buffering and convenience methods like `readLine()`.

Example:

```java
Reader reader = new FileReader("data.txt");          // concrete component
BufferedReader br = new BufferedReader(reader);     // decorator adds buffering
String line = br.readLine();                        // extra functionality
```

Here, `BufferedReader` does not replace or alter `FileReader`—it **wraps** it, adding efficiency and functionality while still exposing the same `Reader` API.

### 3. Key Point

`BufferedReader` is not a *pure* textbook decorator in that it is a **subclass of `Reader`** (an abstract class, not an interface), but functionally it follows the **decorator intent**:

* It implements the same interface/abstract base as the object it wraps.
* It forwards calls to the wrapped object.
* It adds new behavior (buffering + `readLine()`).

---

✅ **Conclusion**: Yes, `BufferedReader` implements the *Decorator Pattern* in Java I/O. It decorates another `Reader` by adding buffering and convenience without changing the original class.

Would you like me to also show how this relates to the *InputStream* side (`BufferedInputStream`) so you can see the parallel in the Java I/O design?

Also: `Collections.synchronizedMap/Set/List`


## Proxy Pattern
`Collections.unmodifiableMap`

Good question! Let's break it down carefully.

### What `Collections.unmodifiableMap` returns

In Java, when you call:

```java
Map<K, V> unmodifiable = Collections.unmodifiableMap(original);
```

you get back **a wrapper object** that implements `Map<K, V>` and delegates all read-only operations (like `get`, `containsKey`, `size`) directly to the underlying `original` map. But **all mutating operations** (like `put`, `remove`, `clear`) throw `UnsupportedOperationException`.

This wrapper is typically implemented as a **static inner class** inside `java.util.Collections`, e.g.:

```java
private static class UnmodifiableMap<K,V> implements Map<K,V>, Serializable {
    private final Map<? extends K, ? extends V> m;
    ...
}
```

### Is it the Proxy Pattern?

The **Proxy Pattern** (from the GoF book) involves an object (the proxy) that implements the same interface as another object (the subject) and controls access to it—often adding some kind of behavior like access control, lazy loading, logging, etc.

`UnmodifiableMap` **does fit this description**:

* It implements the `Map` interface (same as the wrapped object).
* It holds a reference to a "real" map.
* It delegates read methods but restricts write methods by throwing exceptions.

So yes, you can reasonably describe it as using the **Proxy Pattern**—specifically, it’s an **access proxy**, since its purpose is to *restrict access* (prevent modifications).

### Related patterns

* It is also often described as the **Decorator Pattern**: wrapping another object to alter some aspects of its behavior. In fact, the GoF themselves note that the line between "Proxy" and "Decorator" is thin:

    * **Decorator** is usually about *adding responsibilities* (like logging or caching).
    * **Proxy** is usually about *controlling access* (like protection, remote proxy, virtual proxy).

In this case, `UnmodifiableMap`’s main intent is **access control** → fits **Proxy** better.

---

✅ **Conclusion:**
The object returned by `Collections.unmodifiableMap` is indeed an implementation of the **Proxy Pattern** (specifically a **Protection Proxy**). It could also be described as a Decorator in a broad sense, but the intent (restricting mutability) matches Proxy more precisely.

---

Would you like me to also compare this with `Collections.synchronizedMap`? That one’s interesting because it’s closer to a **Decorator** (adds thread-safety behavior) than a Proxy.


## Static Factory Method Pattern

## Factory Method Pattern

Relies on inheritance and polymorphism.

The NotificationFactory declares the createNotification() method.

Subclasses decide which concrete product to instantiate, e.g. email notification, sms notification or push notification

Dynamic flexibility:

You can easily add a new notification type without modifying existing code.

This follows the Open/Closed Principle.

## Abstract Factory Pattern

DBC (DriverManager and Connection)

Slightly less obvious, but JDBC can be seen as using Abstract Factory semantics:

Driver acts as a factory for Connection objects.

Different database vendors provide concrete drivers (MySQLDriver, PostgresDriver), each producing a family of objects (Connection, Statement, ResultSet) that are compatible with that DB.

Client code just interacts with the interfaces; the concrete classes are supplied by the driver.