# software-design-patterns-bible


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
