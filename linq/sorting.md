# **Sorting**

These methods help sort data in ascending or descending order.

* **Example Methods**:

  * `OrderBy()`: Sorts in ascending order.
  * `OrderByDescending()`: Sorts in descending order.

```csharp
  var numbers = new List<int> { 5, 1, 3 };
  var sorted = numbers.OrderBy(n => n); // 1, 3, 5
```

---

While `OrderBy()` and `OrderByDescending()` are the standard LINQ methods for sorting, there are some alternatives or complementary approaches you can use for sorting based on specific requirements. Here's a list of alternatives along with explanations and examples:

---

## 1\. **Reverse()**

The `Reverse()` method reverses the order of the elements in a sequence. It doesn't allow custom sorting logic but simply inverts the order of the items.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5 };
  var reversed = numbers.AsEnumerable().Reverse();

  foreach (var num in reversed)
  {
      Console.WriteLine(num); // Outputs: 5, 4, 3, 2, 1
  }
  ```

> Note: `Reverse()` works on the current order of the sequence and doesn't sort based on custom logic.

---

## 2\. **ThenBy() and ThenByDescending()**

While technically not alternatives but extensions of `OrderBy()`, these methods allow secondary sorting for more complex ordering.

* **Example**:

```csharp
  var people = new List<(string Name, int Age)>
  {
      ("Alice", 30), ("Bob", 25), ("Alice", 25)
  };
  
  var sorted = people
      .OrderBy(p => p.Name)
      .ThenByDescending(p => p.Age);

  foreach (var person in sorted)
  {
      Console.WriteLine($"{person.Name}: {person.Age}");
  }
  // Outputs:
  // Alice: 30
  // Alice: 25
  // Bob: 25
  ```

---

## 3\. **Manual Sorting using List.Sort()**

When working with lists, you can use the `Sort()` method, which allows you to specify a custom sorting logic using a comparer.

* **Example**:

```csharp
  var numbers = new List<int> { 5, 2, 8, 1, 3 };
  numbers.Sort((a, b) => b.CompareTo(a)); // Custom logic for descending sort

  foreach (var num in numbers)
  {
      Console.WriteLine(num); // Outputs: 8, 5, 3, 2, 1
  }
  ```

> This method directly modifies the original list.

---

## 4\. **Using LINQ Query Syntax**

Instead of using `OrderBy()` or `OrderByDescending()` with method syntax, you can achieve sorting through query syntax, which can be more readable in some cases.

* **Example**:

```csharp
  var numbers = new List<int> { 5, 2, 8, 1, 3 };
  var sortedNumbers = from n in numbers
                      orderby n descending
                      select n;

  foreach (var num in sortedNumbers)
  {
      Console.WriteLine(num); // Outputs: 8, 5, 3, 2, 1
  }
  ```

---

## 5\. **Sorting with Comparer.Default**

You can use built-in comparers like `Comparer<T>.Default` for default comparisons in custom sorting scenarios.

* **Example**:

```csharp
  var strings = new List<string> { "banana", "apple", "cherry" };
  strings.Sort(Comparer<string>.Default);

  foreach (var fruit in strings)
  {
      Console.WriteLine(fruit); // Outputs: apple, banana, cherry
  }
  ```

---

## 6\. **Custom Comparers (IComparer<T>)**

If you need highly customized sorting logic, you can implement the `IComparer<T>` interface.

* **Example**:

```csharp
  class CustomComparer : IComparer<string>
  {
      public int Compare(string x, string y)
      {
          // Sort by string length
          return x.Length.CompareTo(y.Length);
      }
  }

  var words = new List<string> { "apple", "banana", "pear" };
  words.Sort(new CustomComparer());

  foreach (var word in words)
  {
      Console.WriteLine(word); // Outputs: pear, apple, banana
  }
  ```

---

## 7\. **GroupBy() for Categorized Sorting**

Sometimes, instead of pure sorting, you may want to group items based on a key, which results in categorized sorting.

* **Example**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var grouped = words.GroupBy(w => w\[0]).OrderBy(g => g.Key);

  foreach (var group in grouped)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 8\. **Zip() for Pairwise Sorting**

In cases where you have two sequences, you can use `Zip()` to pair and sort items together.

* **Example**:

```csharp
  var numbers = new List<int> { 3, 1, 2 };
  var words = new List<string> { "three", "one", "two" };

  var combined = numbers.Zip(words, (n, w) => new { Number = n, Word = w })
                        .OrderBy(p => p.Number);

  foreach (var item in combined)
  {
      Console.WriteLine($"{item.Number}: {item.Word}");
  }
  // Outputs:
  // 1: one
  // 2: two
  // 3: three
  ```

---
