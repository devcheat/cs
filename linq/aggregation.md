
# 8\. **Aggregation**

Performs aggregate operations on data.

* **Example Methods**:

  * `Count()`: Counts elements.
  * `Sum()`: Computes the sum of elements.
  * `Average()`: Computes the average of elements.
  * `Max()`, `Min()`: Retrieves maximum or minimum value.

```cs    
var numbers = new List<int> { 1, 2, 3 };
var total = numbers.Sum(); // 6
```

---

In LINQ, while `Count()`, `Sum()`, `Max()`, and `Average()` are standard aggregation methods, there are other approaches or alternative methods to aggregate data depending on specific requirements. Here are some alternatives for performing aggregation operations with examples:

---

## 1\. **Aggregate()**

`Aggregate()` is a flexible and powerful LINQ method for custom aggregation. It allows you to define how elements in a sequence should be combined into a single result.

* **Example for Custom Sum**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var sum = numbers.Aggregate(0, (acc, n) => acc + n);
  Console.WriteLine(sum); // Output: 10
  ```

* **Example for Custom Max**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var max = numbers.Aggregate((acc, n) => n > acc ? n : acc);
  Console.WriteLine(max); // Output: 4
  ```

---

## 2\. **GroupBy() for Aggregation**

`GroupBy()` can be used for grouped aggregation, where each group produces an aggregated result.

* **Example**:

```csharp
  var items = new\[]
  {
      new { Category = "A", Value = 10 },
      new { Category = "A", Value = 20 },
      new { Category = "B", Value = 30 }
  };

  var grouped = items.GroupBy(i => i.Category)
                     .Select(g => new { Category = g.Key, Total = g.Sum(i => i.Value) });

  foreach (var group in grouped)
  {
      Console.WriteLine($"{group.Category}: {group.Total}");
  }
  // Output:
  // A: 30
  // B: 30
  ```

---

## 3\. **Using Query Syntax**

Query syntax can sometimes make aggregation operations more readable and expressive.

* **Example for Average**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var average = (from n in numbers
                 select n).Average();
  Console.WriteLine(average); // Output: 2.5
  ```

---

## 4\. **Loop-Based Aggregation**

Manual looping allows complete control over aggregation logic and is useful for scenarios where LINQ methods are not flexible enough.

* **Example for Sum**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  int sum = 0;
  foreach (var number in numbers)
  {
      sum += number;
  }
  Console.WriteLine(sum); // Output: 10
  ```

* **Example for Max**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  int max = numbers\[0];
  foreach (var number in numbers)
  {
      if (number > max)
      {
          max = number;
      }
  }
  Console.WriteLine(max); // Output: 4
  ```

---

## 5\. **Distinct() and Custom Aggregation**

You can use `Distinct()` along with other LINQ methods to perform aggregation on unique elements.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 2, 3, 4 };
  var distinctSum = numbers.Distinct().Sum();
  Console.WriteLine(distinctSum); // Output: 10
  ```

---

## 6\. **MinBy() and MaxBy()** (Available in .NET 6 and Above)

`MinBy()` and `MaxBy()` are newer LINQ methods that allow you to find the element with the minimum or maximum value based on a specified property.

* **Example**:

```csharp
  var items = new\[]
  {
      new { Name = "Alice", Age = 30 },
      new { Name = "Bob", Age = 25 },
      new { Name = "Charlie", Age = 35 }
  };

  var youngest = items.MinBy(i => i.Age);
  Console.WriteLine(youngest.Name); // Output: Bob
  ```

---

## 7\. **Custom Extensions**

If you frequently need custom aggregation, you can define your own LINQ extension methods.

* **Example**:

```csharp
  public static int CustomSum(this IEnumerable<int> source)
  {
      int sum = 0;
      foreach (var num in source)
      {
          sum += num;
      }
      return sum;
  }

  var numbers = new List<int> { 1, 2, 3, 4 };
  Console.WriteLine(numbers.CustomSum()); // Output: 10
  ```

---

## 8\. **Partitioning + Aggregation**

You can use `Take()` and `Skip()` to partition a sequence and then apply aggregation to each partition.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
  var firstHalfSum = numbers.Take(numbers.Count / 2).Sum();
  var secondHalfSum = numbers.Skip(numbers.Count / 2).Sum();

  Console.WriteLine($"First Half Sum: {firstHalfSum}, Second Half Sum: {secondHalfSum}");
  // Output: First Half Sum: 6, Second Half Sum: 15
  ```

---
