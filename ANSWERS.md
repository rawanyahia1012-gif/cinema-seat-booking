# Cinema Seat Booking — Questions & Answers

Below are the detailed answers to the theoretical and conceptual questions (`Q1`–`Q5`) for the `Task_CinemaBooking` assignment.

---

### **Q1: Why did `bookings.get(new Seat('C', 3))` and `bookings.containsKey(new Seat('A', 2))` fail before writing `equals()` and `hashCode()`?**
* **Answer:** By default, every Java class inherits `equals()` and `hashCode()` from `java.lang.Object`.
  * `Object.hashCode()` generates a hash based on the object's memory address/identity, so two separate `new Seat('C', 3)` objects produce different hash codes and land in different buckets inside the `HashMap`.
  * `Object.equals()` checks reference equality (`this == obj`), which returns `false` for two distinct objects even if they hold the exact same `row` and `number`.
  * Overriding both methods ensures `HashMap` compares seats by their **logical content** (`row` and `number`) rather than their memory references.

---

### **Q2: What happens if you override `equals()` ONLY, without overriding `hashCode()`?**
* **Answer:** Lookups (`get`, `containsKey`, `remove`) will still fail most of the time.
  * `HashMap` and `HashSet` first use `hashCode()` to calculate which internal **bucket** to search in.
  * If `hashCode()` is not overridden, two equal `Seat` objects (e.g., `new Seat('B', 5)`) will likely return different hash codes and be placed into different buckets. Because `HashMap` only calls `equals()` on objects inside the matching bucket, it will never even reach the stored key to compare them.

---

### **Q3: Why should the fields `row` and `number` in the `Seat` class be `final` (immutable)?**
* **Answer:** Making a key class immutable protects the integrity of `HashMap` and `HashSet`.
  * When a `Seat` object is inserted into a `HashMap`, its bucket location is computed from its `hashCode()` at the time of insertion.
  * If `row` or `number` were modified afterward, the object's `hashCode()` would change, causing future lookups (`get`, `containsKey`, `remove`) to search in a different bucket and fail to find the entry—effectively losing the object inside the map.

---

### **Q4: What would happen if you wrote `public boolean equals(Seat other)` instead of `public boolean equals(Object obj)`?**
* **Answer:** That would **overload** `equals` instead of **overriding** `Object.equals(Object obj)`.
  * Java's `HashMap` and `HashSet` internally call `Object.equals(Object)`, not `equals(Seat)`.
  * Without overriding `equals(Object obj)` (marked with `@Override`), the `HashMap` would fall back to `Object`'s default reference comparison (`==`), and lookups using `new Seat(...)` would continue to fail.

---

### **Q5: What is the contract between `equals()` and `hashCode()` in Java?**
* **Answer:** According to the Java specification:
  1. **Consistency:** Whenever `hashCode()` is invoked on the same object multiple times during execution, it must consistently return the same integer (provided no information used in `equals()` is modified).
  2. **Equal objects must have equal hash codes:** If `a.equals(b)` is `true`, then `a.hashCode() == b.hashCode()` **must** also be `true`.
  3. **Unequal objects do not require distinct hash codes:** If `a.equals(b)` is `false`, `a.hashCode()` and `b.hashCode()` do not have to be different (though producing distinct hash codes for unequal objects improves `HashMap` performance by reducing hash collisions).