# Cinema Seat Booking (`Task_CinemaBooking`)

A Java task demonstrating how to use a custom class (`Seat`) as a key in a `HashMap<Seat, String>` and an element in a `HashSet<Seat>` for a cinema seat booking system, highlighting why overriding `equals()` and `hashCode()` is essential when instantiating new `Seat` objects for queries and updates.

## Overview

- **Hall Layout:** Rows **`A` to `E`**, with seats **`1` to `6`** in each row.
- **Bookings Storage:** `HashMap<Seat, String>` where:
  - **Key:** `Seat` (for example, `B5`)
  - **Value:** `String` (customer name)
- **Broken Seats Tracking:** `HashSet<Seat>` used to prevent booking or moving onto broken seats.
- **Seating Chart Legend (`printChart`):**
  - `X` = booked seat
  - `#` = broken seat
  - `.` = free seat

## Repository Files

- `Task_CinemaBooking.java` — Main Java implementation (booking logic `Parts A–G`, `Seat` class with `equals()` and `hashCode()`, and answers to the questions at the bottom).
- `README.md` — Project overview, compilation/execution steps, expected output, and summary of key concepts.

## How to Compile & Run

1. Open a terminal (`PowerShell` or Command Prompt) inside the folder containing `Task_CinemaBooking.java`.
2. Compile the source file:
   ```bash
   javac Task_CinemaBooking.java
   ```
3. Run the program:
   ```bash
   java Task_CinemaBooking
   ```

## Expected Output (After Implementing `equals()` and `hashCode()`)

> Note: `HashMap` does not guarantee iteration/insertion order, so the key-value order inside `{ }` may vary.

```text
Bookings:           {A1=Ali, C3=Mona, A2=Sara, B5=Omar}
Who booked C3:      Mona
Is A2 taken:        true
Is E6 taken:        false
Youssef -> B5:      refused, taken by Omar
After Ali moved:    {C3=Mona, A2=Sara, B5=Omar, D2=Ali}
Hana -> E1:         refused, seat is broken
Seats booked:       4

     1  2  3  4  5  6
A    .  X  .  .  .  .
B    .  .  .  .  X  .
C    .  .  X  .  .  .
D    .  X  .  .  .  .
E    #  #  .  .  .  .
```

## Key Takeaways & Questions Summary (`Q1`–`Q5`)

1. **`Q1` — Why override `equals()` and `hashCode()`?**
   `HashMap` and `HashSet` use `hashCode()` to select a bucket and `equals()` to check key/element equality. Without overriding them, `Object` uses reference equality, so a newly created `new Seat('B', 5)` will not match an existing `new Seat('B', 5)`.

2. **`Q2` — What happens if `hashCode()` is not overridden (even if `equals()` is)?**
   Different `Seat` instances can hash to different buckets, so lookups (`get`, `containsKey`, `remove`, `contains`) may fail to find matching seats in the right bucket.

3. **`Q3` — Why make `row` and `number` `final`?**
   Keeping `Seat` fields `final` (immutable) ensures a `Seat` object's `hashCode()` never changes after it is inserted as a key into a `HashMap` or element in a `HashSet`, preventing unreachable entries.

4. **`Q4` — Overloading vs. Overriding `equals`:**
   Writing `equals(Seat other)` overloads `equals` instead of overriding `Object.equals(Object obj)`. Since `HashMap` and `HashSet` call `Object.equals(Object)`, the custom comparison would be ignored unless properly overridden with `@Override public boolean equals(Object obj)`.

5. **`Q5` — `hashCode()` Contract:**
   Whenever two objects are equal according to `equals(Object)` (`a.equals(b) == true`), they must return the exact same integer from `hashCode()`.