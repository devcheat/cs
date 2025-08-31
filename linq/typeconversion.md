
# **Conversion**

Converts sequences into other types.

* **Example Methods**:

  * `ToList()`, `ToArray()`: Converts to a list or an array.
  * `ToDictionary()`: Converts to a dictionary.

```csharp
       var numbers = new List<int> { 1, 2, 3 };
       var array = numbers.ToArray(); // \[1, 2, 3]
```

---

Aside from `ToList()` and `ToDictionary()`, LINQ offers other conversion methods to transform a sequence into different data structures or formats. Here’s a detailed look at the alternatives, along with examples:

---

## 1\. **ToArray()**

Converts a sequence into an array.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var array = numbers.ToArray();

  foreach (var num in array)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3, 4
  }
  ```

---

## 2\. **ToHashSet()** (Available in .NET Core 2.0 and later)

Converts a sequence into a `HashSet<T>`, which automatically removes duplicates and provides fast lookups.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 2, 3, 4 };
  var hashSet = numbers.ToHashSet();

  foreach (var num in hashSet)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3, 4 (no duplicates)
  }
  ```

---

## 3\. **Cast<T>()**

Casts elements in a sequence to a specific type, useful when you know all elements can safely be cast to the desired type.

* **Example**:

```csharp
  var objects = new ArrayList { 1, 2, 3 };
  var integers = objects.Cast<int>();

  foreach (var num in integers)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3
  }
  ```

---

## 4\. **OfType<T>()**

Filters elements of a specific type and converts the sequence to that type. This is useful when working with collections of mixed types.

* **Example**:

```csharp
  var items = new List<object> { 1, "hello", 2.5, 3 };
  var integers = items.OfType<int>();

  foreach (var item in integers)
  {
      Console.WriteLine(item); // Outputs: 1, 3
  }
  ```

---

## 5\. **ToLookup()**

Creates a `Lookup<TKey, TElement>`, which is like a dictionary but allows multiple values for the same key.

* **Example**:

```csharp
  var items = new\[]
  {
      new { Key = "A", Value = 1 },
      new { Key = "A", Value = 2 },
      new { Key = "B", Value = 3 }
  };

  var lookup = items.ToLookup(i => i.Key, i => i.Value);

  foreach (var group in lookup)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // A: 1, 2
  // B: 3
  ```

---

## 6\. **AsEnumerable()**

Returns the input sequence as an `IEnumerable<T>`. It is often used to switch from a more specific type (like `IQueryable<T>`) back to a general enumerable.

* **Example**:

```csharp
  var numbers = new int\[] { 1, 2, 3, 4 };
  IEnumerable<int> enumerable = numbers.AsEnumerable();

  foreach (var num in enumerable)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3, 4
  }
  ```

---

## 7\. **ToListAsync()** (Available in Entity Framework Core)

Converts a sequence into a `List<T>` asynchronously. This is useful when working with databases or asynchronous data streams.

* **Example** (using Entity Framework Core):

```csharp
  var result = await dbContext.Users.Where(u => u.Age > 18).ToListAsync();

  foreach (var user in result)
  {
      Console.WriteLine(user.Name);
  }
  ```

---

## 8\. **AsQueryable()**

Converts an `IEnumerable<T>` into an `IQueryable<T>`, enabling query execution on data providers such as databases.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  IQueryable<int> queryableNumbers = numbers.AsQueryable();

  var result = queryableNumbers.Where(n => n > 2);
  foreach (var num in result)
  {
      Console.WriteLine(num); // Outputs: 3, 4
  }
  ```

---

## 9\. **ToConcurrentBag()** (Using `ConcurrentBag<T>` from System.Collections.Concurrent)

Converts a sequence into a thread-safe collection, like `ConcurrentBag<T>`.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var concurrentBag = new ConcurrentBag<int>(numbers);

  foreach (var num in concurrentBag)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3, 4
  }
  ```

---

## 10\. **Custom Conversion**

You can create custom extension methods for specialized conversions.

* **Example**:

```csharp
  public static string\[] ToStringArray(this IEnumerable<int> source)
  {
      return source.Select(x => x.ToString()).ToArray();
  }

  var numbers = new List<int> { 1, 2, 3, 4 };
  var stringArray = numbers.ToStringArray();

  foreach (var str in stringArray)
  {
      Console.WriteLine(str); // Outputs: "1", "2", "3", "4"
  }
  ```

---
