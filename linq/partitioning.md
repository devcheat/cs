
# **Partitioning**

Splits the sequence into parts.

* **Example Methods**:

  * `Take()`: Takes a specified number of elements.
  * `Skip()`: Skips a specified number of elements.

```csharp
       var numbers = new List<int> { 1, 2, 3, 4 };
       var firstTwo = numbers.Take(2); // 1, 2
```

---

If you're looking for alternatives to `Take()` and `Skip()` for partitioning in LINQ, here are some methods and approaches that can help you achieve similar functionality with examples:

---

## 1\. **TakeWhile()**

`TakeWhile()` retrieves elements from a sequence as long as a specified condition is true. It stops processing as soon as the condition becomes false.

* **Example**:

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

`SkipWhile()` skips elements in the sequence as long as a condition is true, and then returns the remaining elements.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var result = numbers.SkipWhile(n => n < 4);

  foreach (var num in result)
  {
      Console.WriteLine(num); // Outputs: 4, 5, 6
  }
  ```

---

## 3\. **Partitioning with Where()**

`Where()` can be used with custom logic to filter elements into different groups, effectively partitioning them.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var evenNumbers = numbers.Where(n => n % 2 == 0); // Partition of even numbers
  var oddNumbers = numbers.Where(n => n % 2 != 0);  // Partition of odd numbers

  Console.WriteLine("Even: " + string.Join(", ", evenNumbers)); // Outputs: 2, 4, 6
  Console.WriteLine("Odd: " + string.Join(", ", oddNumbers));   // Outputs: 1, 3, 5
  ```

---

## 4\. **Manual Looping**

For custom partitioning that requires more control, manual loops can be used to create partitions based on custom logic.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var evenNumbers = new List<int>();
  var oddNumbers = new List<int>();

  foreach (var num in numbers)
  {
      if (num % 2 == 0)
          evenNumbers.Add(num);
      else
          oddNumbers.Add(num);
  }

  Console.WriteLine("Even: " + string.Join(", ", evenNumbers)); // Outputs: 2, 4, 6
  Console.WriteLine("Odd: " + string.Join(", ", oddNumbers));   // Outputs: 1, 3, 5
  ```

---

## 5\. **Using GroupBy()**

`GroupBy()` is useful for partitioning elements into multiple groups based on a shared property or condition.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var grouped = numbers.GroupBy(n => n % 2 == 0 ? "Even" : "Odd");

  foreach (var group in grouped)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // Odd: 1, 3, 5
  // Even: 2, 4, 6
  ```

---

## 6\. **Chunk()** (Available in .NET 6 and Above)

`Chunk()` splits a sequence into chunks of a specified size.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var chunks = numbers.Chunk(2);

  foreach (var chunk in chunks)
  {
      Console.WriteLine(string.Join(", ", chunk)); // Outputs: 1, 2 | 3, 4 | 5, 6
  }
  ```

---

## 7\. **Using LINQ Query Syntax**

Partitioning can also be done using query syntax combined with other LINQ methods like `Where()`.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var firstHalf = from n in numbers
                  where n <= 3
                  select n;

  var secondHalf = from n in numbers
                   where n > 3
                   select n;

  Console.WriteLine("First Half: " + string.Join(", ", firstHalf)); // Outputs: 1, 2, 3
  Console.WriteLine("Second Half: " + string.Join(", ", secondHalf)); // Outputs: 4, 5, 6
  ```

---

## 8\. **Custom Extension Methods**

You can define your own extension methods for partitioning if you frequently require custom logic.

* **Example**:

```csharp
  public static class Extensions
  {
      public static (IEnumerable<T>, IEnumerable<T>) Partition<T>(this IEnumerable<T> source, Func<T, bool> predicate)
      {
          var group1 = source.Where(predicate);
          var group2 = source.Where(x => !predicate(x));
          return (group1, group2);
      }
  }

  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var (evens, odds) = numbers.Partition(n => n % 2 == 0);

  Console.WriteLine("Even: " + string.Join(", ", evens)); // Outputs: 2, 4, 6
  Console.WriteLine("Odd: " + string.Join(", ", odds));   // Outputs: 1, 3, 5
  ```

---
