# **Filtering**

These operations are used to filter data based on specific conditions.

**Example Methods**:

  * `Where()`: Filters elements based on a condition.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0); // 2, 4
```

While `Where()` is the primary method in LINQ for filtering data, there are some alternative methods that can also be used for filtering based on specific scenarios:

---

## 1\. **TakeWhile()**

This method takes elements from a sequence as long as a specified condition is true. It stops processing as soon as the condition becomes false.

**Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var result = numbers.TakeWhile(n => n < 4);

  foreach (var num in result)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3
  }
  ```

---

## 2\. **SkipWhile()**

This method skips elements in the sequence as long as a condition is true, and then returns the remaining elements.

**Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var result = numbers.SkipWhile(n => n < 4);

  foreach (var num in result)
  {
      Console.WriteLine(num); // Outputs: 4, 5, 6
  }
  ```

---

## 3\. **OfType<T>()**

This method filters elements in a sequence based on their type, making it useful for filtering collections containing mixed types.

**Example**:

```csharp
  var items = new List<object> { 1, "hello", 2.5, 3 };
  var integers = items.OfType<int>();

  foreach (var item in integers)
  {
      Console.WriteLine(item); // Outputs: 1, 3
  }
  ```

---

## 4\. **First() and FirstOrDefault() with Predicate**

Although these methods are typically used to retrieve a single element, they can act as a filter when used with a predicate.

**Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5 };
  var firstEven = numbers.First(n => n % 2 == 0); // Retrieves the first even number: 2
  ```

---

## 5\. **Single() and SingleOrDefault() with Predicate**

These methods work similarly to `First()` but enforce that there is only one matching element in the sequence.

**Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3 };
  var uniqueEven = numbers.Single(n => n % 2 == 0); // Output: 2
  ```

---

## 6\. **All()**

This method doesn't filter but checks whether all elements satisfy a condition. It can be combined with other LINQ operations for effective filtering.

**Example**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = numbers.All(n => n % 2 == 0); // true
  ```

---

## Use Cases

Each of these alternatives complements specific filtering needs. For example:

* Use `TakeWhile()` and `SkipWhile()` for filtering based on a sequence order.
* Use `OfType<T>()` when working with mixed-type collections.
* Use `First()` or `Single()` when interested in retrieving specific elements that match a condition.



---
