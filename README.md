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


Here’s a **condensed, high-signal cheat sheet** of **non-GoF design patterns**, grouped by domain and stripped to essentials.

---

# 🚀 **Non-GoF Design Patterns — Condensed Cheat Sheet**

## **1. Architectural & Enterprise Patterns**

* **Layered Architecture** — Organize system into presentation, business, data layers.
* **Hexagonal / Ports & Adapters** — Core domain isolated from external systems.
* **Clean / Onion Architecture** — Dependency rule: outer rings depend on inner.
* **CQRS** — Separate read and write models.
* **Event Sourcing** — Persist all domain events; reconstruct state by replaying.
* **Service Layer** — API exposing application operations.
* **Repository** — Collections-like interface for aggregates or entities.
* **Unit of Work** — Track changes, commit as a transaction.
* **Domain Model** — Rich business logic inside objects.
* **Active Record** — Object wraps DB row + CRUD.
* **Data Mapper** — Object ↔ DB table mapping (ORM style).
* **Identity Map** — Ensure object identity uniqueness.
* **Lazy Load** — Defer expensive loading until needed.

---

## **2. Concurrency & Parallelism Patterns**

* **Reactor** — Event loop dispatches non-blocking I/O.
* **Proactor** — OS completes I/O asynchronously and invokes handlers.
* **Half-Sync/Half-Async** — Async front; sync workers.
* **Leader–Followers** — Threads share request queue, rotate leadership.
* **Thread Pool** — Reusable worker threads.
* **Active Object** — Encapsulate asynchronous method calls.
* **Guarded Suspension** — Wait until precondition holds.
* **Future / Promise** — Placeholder for async result.
* **Producer–Consumer** — Queue between producers and workers.
* **Pipeline** — Break tasks into sequential processing stages.

---

## **3. Distributed Systems & Integration Patterns**

* **Pub/Sub** — Publish messages to subscribers.
* **Message Bus** — Shared transport for events/commands.
* **Circuit Breaker** — Prevent cascading failures by opening/closing calls.
* **Retry** — Re-execute failed operations with backoff.
* **Timeout** — Fail on slow operations.
* **Bulkhead** — Isolate resources to contain failures.
* **Saga** — Distributed transactions via compensating steps.
* **API Gateway** — Single router for services.
* **Message Translator / Filter / Router / Aggregator / Splitter** — Message-shaping patterns (EIP).
* **Sidecar / Ambassador** — Infrastructure logic moved to helper process.
* **Service Discovery** — Automatically locate service endpoints.

---

## **4. DDD (Domain-Driven Design) Patterns**

* **Entity** — Identity-based domain object.
* **Value Object** — Immutable, equality by value.
* **Aggregate** — Consistency boundary + root.
* **Domain Event** — Represents something that happened.
* **Factory (DDD)** — Create complex aggregates.
* **Bounded Context** — Explicit domain boundary.
* **Context Map:**

    * **Anti-corruption Layer**
    * **Shared Kernel**
    * **Conformist**
    * **Published Language**

---

## **5. UI / Presentation Patterns**

* **MVC** — Model ↔ View mediated by Controller.
* **MVP** — Presenter handles logic; view is passive.
* **MVVM** — View ↔ ViewModel via data binding.
* **Flux** — Unidirectional data flow.
* **Redux** — Single state container + reducers.
* **Coordinator** — Centralizes navigation logic.

---

## **6. Microservices Patterns**

* **Strangler Fig** — Gradual replacement of legacy system.
* **Database per Service** — Independent persistence.
* **CQRS** (cross-listed)
* **Saga** (cross-listed)
* **Backends for Frontends (BFF)** — Per-client API layers.
* **Sidecar / Ambassador / Adapter** — Service mesh roles.

---

## **7. Testing & Deployment Patterns**

* **Test Double** — Stub, Mock, Fake, Spy.
* **Snapshot Testing** — Persist output snapshots for comparison.
* **Golden Master** — Compare against known-good output.
* **Feature Toggle** — Turn features on/off at runtime.
* **Blue–Green Deployment** — Two environments for safe switching.
* **Canary Release** — Roll out to subset of users first.

---

## **8. Language / Paradigm Patterns**

* **Dependency Injection** — Externalize object dependencies.
* **Mixin** — Reusable behavior via composition.
* **Null Object** — Non-operational placeholder object.
* **Monad** — FP-style effect packaging.
* **Prototype Delegation (JS)** — Behavior via prototype chain.

---

If you want, I can also create a **printable one-page PDF**, a **visual diagram**, or a **quiz-style version** to memorize these patterns.
