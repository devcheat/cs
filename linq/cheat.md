# LINQ

LINQ (Language-Integrated Query) performs various types of operations to manipulate and query data. These operations can be categorized as follows:


### [Filtering](#filtering-1)
- [1. TakeWhile()](#1-takewhile)
- [2. SkipWhile()](#2-skipwhile)
- [3. OfType<T>()](#3-oftypet)
- [4. First() and FirstOrDefault() with Predicate](#4-first-and-firstordefault-with-predicate)
- [5. Single() and SingleOrDefault() with Predicate](#5-single-and-singleordefault-with-predicate)
- [6. All()](#6-all)
- [Use Cases](#use-cases)
### [Projection](#projection-1)
- [1. Query Syntax](#1-query-syntax)
- [2. Anonymous Types](#2-anonymous-types)
- [3. Cast<T>()](#3-castt)
- [4. Zip()](#4-zip)
- [5. Custom Projection Logic](#5-custom-projection-logic)
- [6. Conversion Methods](#6-conversion-methods)
### [Sorting](#sorting-1)
- [1. Reverse()](#1-reverse)
- [2. ThenBy() and ThenByDescending()](#2-thenby-and-thenbydescending)
- [3. Manual Sorting using List.Sort()](#3-manual-sorting-using-listsort)
- [4. Using LINQ Query Syntax](#4-using-linq-query-syntax)
- [5. Sorting with Comparer.Default](#5-sorting-with-comparerdefault)
- [6. Custom Comparers (IComparer<T>)](#6-custom-comparers-icomparert)
- [7. GroupBy() for Categorized Sorting](#7-groupby-for-categorized-sorting)
- [8. Zip() for Pairwise Sorting](#8-zip-for-pairwise-sorting)
### [Grouping](#grouping-1)
- [1. ToLookup()](#1-tolookup)
- [2. Using Query Syntax](#2-using-query-syntax)
- [3. Custom Dictionary-Based Grouping](#3-custom-dictionary-based-grouping)
- [4. Grouping with LINQ Extensions](#4-grouping-with-linq-extensions)
- [5. Partitioning Instead of Grouping](#5-partitioning-instead-of-grouping)
- [6. Using Multiple LINQ Operations](#6-using-multiple-linq-operations)
### [Joining](#joining-1)
- [1. GroupJoin()](#1-groupjoin)
- [2. Zip()](#2-zip)
- [3. SelectMany()](#3-selectmany)
- [4. Manual Loops](#4-manual-loops)
- [5. Using LINQ Query Syntax](#5-using-linq-query-syntax)
- [6. Intersect() for Matching Data](#6-intersect-for-matching-data)
### [Set Operations](#set-operations-1)
- [1. Except()](#1-except)
- [2. Concat()](#2-concat)
- [3. Using HashSet for Manual Set Operations](#3-using-hashset-for-manual-set-operations)
- [4. GroupBy() for Custom Grouped Set Operations](#4-groupby-for-custom-grouped-set-operations)
- [5. Manual Loops for Custom Set Operations](#5-manual-loops-for-custom-set-operations)
- [6. DistinctBy() (Available in .NET 6 and above)](#6-distinctby-available-in-net-6-and-above)
- [7. Aggregate() for Set-Like Custom Operations](#7-aggregate-for-set-like-custom-operations)
### [Element Operations](#element-operations-1)
- [1. Single() and SingleOrDefault()](#1-single-and-singleordefault)
- [2. Take()](#2-take)
- [3. Skip()](#3-skip)
- [4. DefaultIfEmpty()](#4-defaultifempty)
- [5. LastOrDefault()](#5-lastordefault)
- [6. Where() with Element Filtering](#6-where-with-element-filtering)
- [7. Query Syntax](#7-query-syntax)
- [8. Aggregate()](#8-aggregate)
- [9. Any() with a Predicate](#9-any-with-a-predicate)
## [Aggregation](#aggregation-1)
- [1. Aggregate()](#1-aggregate)
- [2. GroupBy() for Aggregation](#2-groupby-for-aggregation)
- [3. Using Query Syntax](#3-using-query-syntax)
- [4. Loop-Based Aggregation](#4-loop-based-aggregation)
- [5. Distinct() and Custom Aggregation](#5-distinct-and-custom-aggregation)
- [6. MinBy() and MaxBy() (Available in .NET 6 and Above)](#6-minby-and-maxby-available-in-net-6-and-above)
- [7. Custom Extensions](#7-custom-extensions)
- [8. Partitioning + Aggregation](#8-partitioning--aggregation)
### [Quantifiers](#quantifiers-1)
- [1. Count()](#1-count)
- [2. Where()](#2-where)
- [3. FirstOrDefault() / SingleOrDefault()](#3-firstordefault--singleordefault)
- [4. Aggregate()](#4-aggregate)
- [5. Manual Loops](#5-manual-loops)
- [6. TakeWhile() and SkipWhile()](#6-takewhile-and-skipwhile)
- [7. Any() with Negation](#7-any-with-negation)
- [8. Contains()](#8-contains)
### [Partitioning](#partitioning-1)
- [1. TakeWhile()](#1-takewhile-1)
- [2. SkipWhile()](#2-skipwhile-1)
- [3. Partitioning with Where()](#3-partitioning-with-where)
- [4. Manual Looping](#4-manual-looping)
- [5. Using GroupBy()](#5-using-groupby)
- [6. Chunk() (Available in .NET 6 and Above)](#6-chunk-available-in-net-6-and-above)
- [7. Using LINQ Query Syntax](#7-using-linq-query-syntax)
- [8. Custom Extension Methods](#8-custom-extension-methods)
### [Conversion](#conversion-1)
- [1. ToArray()](#1-toarray)
- [2. ToHashSet() (Available in .NET Core 2.0 and later)](#2-tohashset-available-in-net-core-20-and-later)
- [3. Cast<T>()](#3-castt-1)
- [4. OfType<T>()](#4-oftypet)
- [5. ToLookup()](#5-tolookup)
- [6. AsEnumerable()](#6-asenumerable)
- [7. ToListAsync() (Available in Entity Framework Core)](#7-tolistasync-available-in-entity-framework-core)
- [8. AsQueryable()](#8-asqueryable)
- [9. ToConcurrentBag() (Using `ConcurrentBag<T>` from System.Collections.Concurrent)](#9-toconcurrentbag-using-concurrentbagt-from-systemcollectionsconcurrent)
- [10. Custom Conversion](#10-custom-conversion)

---

# **Filtering**

These operations are used to filter data based on specific conditions.

* **Example Methods**:

  * `Where()`: Filters elements based on a condition.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0); // 2, 4
```

While `Where()` is the primary method in LINQ for filtering data, there are some alternative methods that can also be used for filtering based on specific scenarios:

---

## 1\. **TakeWhile()**

This method takes elements from a sequence as long as a specified condition is true. It stops processing as soon as the condition becomes false.

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

This method skips elements in the sequence as long as a condition is true, and then returns the remaining elements.

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

## 3\. **OfType<T>()**

This method filters elements in a sequence based on their type, making it useful for filtering collections containing mixed types.

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

## 4\. **First() and FirstOrDefault() with Predicate**

Although these methods are typically used to retrieve a single element, they can act as a filter when used with a predicate.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4, 5 };
  var firstEven = numbers.First(n => n % 2 == 0); // Retrieves the first even number: 2
  ```

---

## 5\. **Single() and SingleOrDefault() with Predicate**

These methods work similarly to `First()` but enforce that there is only one matching element in the sequence.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3 };
  var uniqueEven = numbers.Single(n => n % 2 == 0); // Output: 2
  ```

---

## 6\. **All()**

This method doesn't filter but checks whether all elements satisfy a condition. It can be combined with other LINQ operations for effective filtering.

* **Example**:

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

# **Projection**

Projection transforms data into a new form.

**Example Methods**:

  * `Select()`: Projects each element into a new form.

```csharp
       var numbers = new List<int> { 1, 2, 3 };
       var squares = numbers.Select(n => n \* n); // 1, 4, 9
```

  * `SelectMany()`: Projects elements and flattens them into a single collection.

```csharp
       var lists = new List<List<int>> { new List<int>{1, 2}, new List<int>{3, 4} };
       var flatList = lists.SelectMany(l => l); // 1, 2, 3, 4
```

---

While `Select()` and `SelectMany()` are the primary LINQ methods for projection, there are some alternative techniques and methods that can achieve similar results depending on the scenario. Here are a few options:

---

## 1\. **Query Syntax**

Instead of using `Select()` or `SelectMany()` with method syntax, you can use LINQ's query syntax for projection. It’s a declarative way to specify projections.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3 };
  var squares = from number in numbers
                select number \* number;

  foreach (var square in squares)
  {
      Console.WriteLine(square); // Outputs: 1, 4, 9
  }
  ```

Here, `select` in query syntax serves the same purpose as `Select()` in method syntax.

---

## 2\. **Anonymous Types**

If you want to create projections in the form of custom objects, you can use anonymous types in combination with query syntax.

* **Example**:

```csharp
  var people = new List<string> { "Alice", "Bob", "Charlie" };
  var projections = from person in people
                    select new { Name = person, NameLength = person.Length };

  foreach (var projection in projections)
  {
      Console.WriteLine($"{projection.Name}: {projection.NameLength}");
  }
  // Outputs: 
  // Alice: 5
  // Bob: 3
  // Charlie: 7
  ```

Here, the query creates projections that include the name and its length using an anonymous type.

---

## 3\. **Cast<T>()**

The `Cast<T>()` method can be used when projecting elements into a specific type. This is especially useful for type conversion in collections.

* **Example**:

```csharp
  var objects = new ArrayList { 1, 2, 3 };
  var numbers = objects.Cast<int>(); // Projects elements as integers

  foreach (var num in numbers)
  {
      Console.WriteLine(num); // Outputs: 1, 2, 3
  }
  ```

---

## 4\. **Zip()**

`Zip()` can be used to project data by combining two collections into one. It produces a new sequence by applying a function to corresponding elements of two input sequences.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3 };
  var words = new List<string> { "one", "two", "three" };
  
  var projections = numbers.Zip(words, (num, word) => $"{num}: {word}");

  foreach (var item in projections)
  {
      Console.WriteLine(item); 
  }
  // Outputs:
  // 1: one
  // 2: two
  // 3: three
  ```

---

## 5\. **Custom Projection Logic**

Sometimes you can use other LINQ methods like `Aggregate()`, `GroupBy()`, or loops to create custom projections tailored to your specific needs.

* **Example using `GroupBy()`**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var groupedProjections = words.GroupBy(w => w\[0]);

  foreach (var group in groupedProjections)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 6\. **Conversion Methods**

You can use methods like `ToDictionary()`, `ToArray()`, or `ToList()` for creating projections into specific data structures.

* **Example with `ToDictionary()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3 };
  var dictionary = numbers.ToDictionary(n => n, n => n \* n);

  foreach (var kvp in dictionary)
  {
      Console.WriteLine($"{kvp.Key}: {kvp.Value}");
  }
  // Outputs:
  // 1: 1
  // 2: 4
  // 3: 9
  ```



---

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





# **Grouping**

Groups data based on a key.

* **Example Method**:

  * `GroupBy()`: Groups elements based on a key.

```csharp
  var words = new List<string> { "apple", "banana", "avocado" };
  var grouped = words.GroupBy(w => w\[0]);
  foreach (var group in grouped) {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Output:
  // a: apple, avocado
  // b: banana
```

---

Although `GroupBy()` is the go-to LINQ method for grouping data, there are alternative ways to achieve similar functionality, depending on the context. Here are some approaches:

---

## 1\. **ToLookup()**

`ToLookup()` is similar to `GroupBy()` but creates a one-to-many lookup structure. It allows indexed access to grouped elements and is immutable (unlike `GroupBy()`).

* **Example**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var lookup = words.ToLookup(w => w\[0]);

  foreach (var group in lookup)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 2\. **Using Query Syntax**

Instead of `GroupBy()` with method syntax, LINQ query syntax can be used for grouping. It’s more declarative and may improve readability in some scenarios.

* **Example**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var grouped = from word in words
                group word by word\[0] into g
                select g;

  foreach (var group in grouped)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 3\. **Custom Dictionary-Based Grouping**

You can manually group elements using a `Dictionary<TKey, List<T>>`. This approach provides more control over the grouping process and customization.

* **Example**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var groups = new Dictionary<char, List<string>>();

  foreach (var word in words)
  {
      char key = word\[0];
      if (!groups.ContainsKey(key))
      {
          groups\[key] = new List<string>();
      }
      groups\[key].Add(word);
  }

  foreach (var group in groups)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group.Value)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 4\. **Grouping with LINQ Extensions**

You can use extensions like `Aggregate()` to build custom groupings without relying on `GroupBy()`.

* **Example**:

```csharp
  var words = new List<string> { "apple", "apricot", "banana", "blueberry" };
  var grouped = words.Aggregate(
      new Dictionary<char, List<string>>(),
      (acc, word) =>
      {
          char key = word\[0];
          if (!acc.ContainsKey(key))
          {
              acc\[key] = new List<string>();
          }
          acc\[key].Add(word);
          return acc;
      });

  foreach (var group in grouped)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group.Value)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---

## 5\. **Partitioning Instead of Grouping**

If you don't need full grouping but just want to split items into categories, `Where()` and `Select()` can be used for filtering categories separately.

* **Example**:

```csharp
  var words = new List<string> { "apple", "banana", "cherry" };
  var aWords = words.Where(w => w.StartsWith("a"));
  var bWords = words.Where(w => w.StartsWith("b"));

  Console.WriteLine($"a: {string.Join(", ", aWords)}"); // Outputs: apple
  Console.WriteLine($"b: {string.Join(", ", bWords)}"); // Outputs: banana
  ```

---

## 6\. **Using Multiple LINQ Operations**

Combining LINQ operations like `SelectMany()` or `GroupBy()` alternatives can create complex grouping logic.

* **Example with `SelectMany()`**:

```csharp
  var words = new List<List<string>>
  {
      new List<string> { "apple", "apricot" },
      new List<string> { "banana", "blueberry" }
  };

  var flattenedGroups = words.SelectMany(group => group.GroupBy(w => w\[0]));

  foreach (var group in flattenedGroups)
  {
      Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
  }
  // Outputs:
  // a: apple, apricot
  // b: banana, blueberry
  ```

---





# **Joining**

Combines two data sources based on a common key.

* **Example Method**:

  * `Join()`: Joins two collections based on matching keys.

```csharp
       var students = new\[] { new { ID = 1, Name = "John" } };
       var scores = new\[] { new { StudentID = 1, Score = 90 } };

       var result = students.Join(
           scores,
           student => student.ID,
           score => score.StudentID,
           (student, score) => new { student.Name, score.Score }
       );

       foreach (var item in result)
       {
           Console.WriteLine($"{item.Name}: {item.Score}"); // John: 90
       }
```

---

While `Join()` is the standard LINQ method for combining two sequences based on a matching key, there are several alternatives you can use for joining data, depending on your specific needs. Here's a list of these methods with examples:

---

## 1\. **GroupJoin()**

`GroupJoin()` is used for a hierarchical or group-based join. It produces a collection of groups, where each group consists of elements from the second collection that match a key from the first collection.

* **Example**:

```csharp
  var departments = new\[] { new { DeptID = 1, DeptName = "HR" }, new { DeptID = 2, DeptName = "IT" } };
  var employees = new\[] { new { EmpName = "Alice", DeptID = 1 }, new { EmpName = "Bob", DeptID = 2 } };

  var result = departments.GroupJoin(
      employees,
      dept => dept.DeptID,
      emp => emp.DeptID,
      (dept, empGroup) => new { dept.DeptName, Employees = empGroup }
  );

  foreach (var group in result)
  {
      Console.WriteLine($"{group.DeptName}: {string.Join(", ", group.Employees.Select(e => e.EmpName))}");
  }
  // Output:
  // HR: Alice
  // IT: Bob
  ```

---

## 2\. **Zip()**

`Zip()` combines two sequences into one by applying a function to corresponding elements. While it is not a direct replacement for `Join()` (as it requires both sequences to have the same number of elements), it can be used for pairwise joining.

* **Example**:

```csharp
  var names = new\[] { "Alice", "Bob" };
  var scores = new\[] { 85, 90 };

  var result = names.Zip(scores, (name, score) => $"{name}: {score}");

  foreach (var entry in result)
  {
      Console.WriteLine(entry);
  }
  // Output:
  // Alice: 85
  // Bob: 90
  ```

---

## 3\. **SelectMany()**

`SelectMany()` can be used to create custom joins by flattening one-to-many relationships.

* **Example**:

```csharp
  var students = new\[] { new { ID = 1, Name = "John" }, new { ID = 2, Name = "Jane" } };
  var courses = new\[] { new { StudentID = 1, Course = "Math" }, new { StudentID = 2, Course = "Science" } };

  var result = students.SelectMany(
      student => courses.Where(course => course.StudentID == student.ID),
      (student, course) => new { student.Name, course.Course }
  );

  foreach (var item in result)
  {
      Console.WriteLine($"{item.Name}: {item.Course}");
  }
  // Output:
  // John: Math
  // Jane: Science
  ```

---

## 4\. **Manual Loops**

A traditional alternative to `Join()` is using nested loops to manually join two collections. This approach gives you full control over the logic but can be less efficient for large datasets.

* **Example**:

```csharp
  var students = new\[] { new { ID = 1, Name = "John" }, new { ID = 2, Name = "Jane" } };
  var scores = new\[] { new { StudentID = 1, Score = 85 }, new { StudentID = 2, Score = 90 } };

  var result = new List<object>();
  foreach (var student in students)
  {
      foreach (var score in scores)
      {
          if (student.ID == score.StudentID)
          {
              result.Add(new { student.Name, score.Score });
          }
      }
  }

  foreach (var item in result)
  {
      Console.WriteLine($"{item.Name}: {item.Score}");
  }
  // Output:
  // John: 85
  // Jane: 90
  ```

---

## 5\. **Using LINQ Query Syntax**

Instead of using `Join()` in method syntax, you can achieve similar results using LINQ query syntax for joins, which can sometimes be more intuitive to read.

* **Example**:

```csharp
  var students = new\[] { new { ID = 1, Name = "John" }, new { ID = 2, Name = "Jane" } };
  var scores = new\[] { new { StudentID = 1, Score = 85 }, new { StudentID = 2, Score = 90 } };

  var result = from student in students
               join score in scores on student.ID equals score.StudentID
               select new { student.Name, score.Score };

  foreach (var item in result)
  {
      Console.WriteLine($"{item.Name}: {item.Score}");
  }
  // Output:
  // John: 85
  // Jane: 90
  ```

---

## 6\. **Intersect() for Matching Data**

If you only need to find matching elements (intersection) based on common data, `Intersect()` can be an alternative, though it has limited use compared to `Join()`.

* **Example**:

```csharp
  var list1 = new\[] { 1, 2, 3 };
  var list2 = new\[] { 2, 3, 4 };

  var result = list1.Intersect(list2);

  foreach (var item in result)
  {
      Console.WriteLine(item);
  }
  // Output:
  // 2
  // 3
  ```

---



# **Set Operations**

These operations are used to perform set-based operations like union or intersection.

* **Example Methods**:

  * `Distinct()`: Removes duplicates.
  * `Union()`: Combines two collections, removing duplicates.
  * `Intersect()`: Returns common elements.

```csharp
     var list1 = new List<int> { 1, 2, 3 };
     var list2 = new List<int> { 3, 4, 5 };
     var union = list1.Union(list2); // 1, 2, 3, 4, 5
     var intersect = list1.Intersect(list2); // 3
```

---

There are several alternatives and techniques you can use instead of `Distinct()`, `Union()`, and `Intersect()` for performing set operations in LINQ, depending on your requirements. Here's a detailed list of options, along with examples:

---

## 1\. **Except()**

This method returns the difference between two sequences, i.e., elements present in the first sequence but not in the second.

* **Example**:

```csharp
  var list1 = new\[] { 1, 2, 3, 4 };
  var list2 = new\[] { 3, 4, 5, 6 };

  var result = list1.Except(list2);

  foreach (var item in result)
  {
      Console.WriteLine(item); // Outputs: 1, 2
  }
  ```

---

## 2\. **Concat()**

`Concat()` combines two sequences by appending the elements of the second sequence to the first. Unlike `Union()`, it does not remove duplicates.

* **Example**:

```csharp
  var list1 = new\[] { 1, 2, 3 };
  var list2 = new\[] { 3, 4, 5 };

  var result = list1.Concat(list2);

  foreach (var item in result)
  {
      Console.WriteLine(item); // Outputs: 1, 2, 3, 3, 4, 5
  }
  ```

---

## 3\. **Using HashSet for Manual Set Operations**

You can leverage the `HashSet<T>` class to manually perform operations like union, intersection, and difference.

* **Example for Union**:

```csharp
  var list1 = new\[] { 1, 2, 3 };
  var list2 = new\[] { 3, 4, 5 };
  var set = new HashSet<int>(list1);
  set.UnionWith(list2);

  foreach (var item in set)
  {
      Console.WriteLine(item); // Outputs: 1, 2, 3, 4, 5
  }
  ```

* **Example for Intersection**:

```csharp
  var list1 = new\[] { 1, 2, 3 };
  var list2 = new\[] { 3, 4, 5 };
  var set = new HashSet<int>(list1);
  set.IntersectWith(list2);

  foreach (var item in set)
  {
      Console.WriteLine(item); // Outputs: 3
  }
  ```

---

## 4\. **GroupBy() for Custom Grouped Set Operations**

You can use `GroupBy()` to group elements by their key and create custom set operations.

* **Example**:

```csharp
  var list1 = new\[] { 1, 2, 3, 4 };
  var list2 = new\[] { 3, 4, 5, 6 };

  var grouped = list1.Concat(list2)
                     .GroupBy(x => x)
                     .Where(g => g.Count() == 1) // Exclude duplicates
                     .Select(g => g.Key);

  foreach (var item in grouped)
  {
      Console.WriteLine(item); // Outputs: 1, 2, 5, 6
  }
  ```

---

## 5\. **Manual Loops for Custom Set Operations**

Sometimes, manual loops are useful for specific or highly customized operations.

* **Example for Intersection**:

```csharp
  var list1 = new\[] { 1, 2, 3, 4 };
  var list2 = new\[] { 3, 4, 5, 6 };

  var result = new List<int>();
  foreach (var item in list1)
  {
      if (list2.Contains(item))
      {
          result.Add(item);
      }
  }

  foreach (var item in result)
  {
      Console.WriteLine(item); // Outputs: 3, 4
  }
  ```

---

## 6\. **DistinctBy()** (Available in .NET 6 and above)

`DistinctBy()` allows you to remove duplicates based on a specific property of the elements in a collection. This is an extension of `Distinct()`.

* **Example**:

```csharp
  var products = new\[]
  {
      new { ID = 1, Name = "Apple" },
      new { ID = 2, Name = "Banana" },
      new { ID = 1, Name = "Apple" }
  };

  var distinctProducts = products.DistinctBy(p => p.ID);

  foreach (var product in distinctProducts)
  {
      Console.WriteLine($"{product.ID}: {product.Name}");
  }
  // Outputs:
  // 1: Apple
  // 2: Banana
  ```

---

## 7\. **Aggregate() for Set-Like Custom Operations**

`Aggregate()` is a powerful LINQ method that can be used to implement custom set operations.

* **Example for Union**:

```csharp
  var list1 = new\[] { 1, 2, 3 };
  var list2 = new\[] { 3, 4, 5 };

  var union = list1.Aggregate(
      list2.ToList(),
      (acc, item) =>
      {
          if (!acc.Contains(item))
          {
              acc.Add(item);
          }
          return acc;
      });

  foreach (var item in union)
  {
      Console.WriteLine(item); // Outputs: 3, 4, 5, 1, 2
  }
  ```

---



# **Element Operations**

Used to retrieve single elements from a sequence.

* **Example Methods**:

  * `First()`, `FirstOrDefault()`: Returns the first element.
  * `Last()`, `LastOrDefault()`: Returns the last element.
  * `ElementAt()`, `ElementAtOrDefault()`: Retrieves an element at a specific index.

```csharp
       var numbers = new List<int> { 1, 2, 3 };
       var first = numbers.First(); // 1
```

---

While `First()`, `Last()`, and `ElementAt()` are commonly used in LINQ for retrieving specific elements, there are several alternatives you can use depending on your requirements. Here are the alternatives, with explanations and examples:

---

## 1\. **Single() and SingleOrDefault()**

`Single()` ensures that the sequence contains exactly one element matching a condition. If there's more than one match, it throws an exception. `SingleOrDefault()` works similarly but returns the default value (`null` for reference types) if no elements match.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var singleEven = numbers.Single(n => n == 2); // Output: 2
  ```

* **Example for `SingleOrDefault()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var nonExisting = numbers.SingleOrDefault(n => n == 5); // Output: null (or default value)
  ```

---

## 2\. **Take()**

`Take()` retrieves the first `n` elements from a sequence. Although this is not a direct replacement, it can be combined with other methods like `ToList()` or `First()` to extract specific elements.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var firstTwo = numbers.Take(2); // Outputs: 1, 2
  ```

---

## 3\. **Skip()**

`Skip()` bypasses a specified number of elements and returns the remaining ones. When used together with `Take()`, it can simulate retrieving elements at a specific position.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var elementAtPosition = numbers.Skip(2).First(); // Output: 3 (simulates ElementAt(2))
  ```

---

## 4\. **DefaultIfEmpty()**

`DefaultIfEmpty()` provides a default value if the sequence is empty. This method is useful when you expect the sequence might have no elements and don't want exceptions like `InvalidOperationException`.

* **Example**:

```csharp
  var numbers = new List<int>();
  var firstOrDefault = numbers.DefaultIfEmpty(-1).First(); // Output: -1 (default value)
  ```

---

## 5\. **LastOrDefault()**

Similar to `Last()`, this method returns the last element of a sequence or a default value if the sequence is empty.

* **Example**:

```csharp
  var numbers = new List<int>();
  var lastOrDefault = numbers.LastOrDefault(); // Output: default value (null or 0 depending on type)
  ```

---

## 6\. **Where() with Element Filtering**

`Where()` can be used to filter elements based on conditions, and combined with `First()` or `Last()` to retrieve the first or last matching element.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var firstEven = numbers.Where(n => n % 2 == 0).First(); // Output: 2
  ```

---

## 7\. **Query Syntax**

You can use LINQ query syntax as an alternative to these methods for clarity or more declarative operations.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var firstEven = (from n in numbers
                   where n % 2 == 0
                   select n).First(); // Output: 2
  ```

---

## 8\. **Aggregate()**

`Aggregate()` allows you to perform custom operations to find specific elements in a sequence. It can be used to simulate operations like finding the first or last element based on conditions.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  var lastNumber = numbers.Aggregate((acc, n) => n); // Output: 4 (simulates Last())
  ```

---

## 9\. **Any() with a Predicate**

`Any()` checks whether a sequence contains elements that match a condition. Though it does not return an element, it can be used for validation or to combine with other LINQ methods.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = numbers.Any(n => n % 2 == 0); // Output: true
  ```

---

## Comparison of Usage:

| \*\*Method\*\*   | \*\*Primary Use Case\*\* |
|--|--|
| `Single()`, `SingleOrDefault()` | Ensure exactly one element matches a condition or handle empty sequences gracefully.  |
| `Take()` and `Skip()`           | Retrieve elements by position or simulate custom retrieval logic.                 |
| `DefaultIfEmpty()`              | Provide default values if the sequence is empty.                                  |
| `Aggregate()`                   | Implement custom logic for retrieving elements.                                   |

---



# **Aggregation**

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



# **Quantifiers**

Determines whether any or all elements satisfy a condition.

* **Example Methods**:

  * `Any()`: Checks if any element matches a condition.
  * `All()`: Checks if all elements match a condition.

```csharp
       var numbers = new List<int> { 1, 2, 3 };
       var hasEven = numbers.Any(n => n % 2 == 0); // true
```

---

`Any()` and `All()` are widely used LINQ quantifier methods to evaluate conditions on elements in a collection. If you're looking for alternatives or methods to achieve similar functionality, here are some options with examples:

---

## 1\. **Count()**

`Count()` can be used to evaluate conditions by checking the number of matching elements. While it's not a direct replacement for `Any()` or `All()`, it can mimic their behavior.

* **Alternative for `Any()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = numbers.Count(n => n % 2 == 0) > 0; // True if at least one even number exists
  ```

* **Alternative for `All()`**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = numbers.Count(n => n % 2 != 0) == 0; // True if all numbers are even
  ```

---

## 2\. **Where()**

`Where()` is a filtering method that can be combined with checks like `FirstOrDefault()` or `Any()` to achieve similar results.

* **Alternative for `Any()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = numbers.Where(n => n % 2 == 0).Any(); // True if at least one even number exists
  ```

* **Alternative for `All()`**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = !numbers.Where(n => n % 2 != 0).Any(); // True if all numbers are even
  ```

---

## 3\. **FirstOrDefault() / SingleOrDefault()**

You can use `FirstOrDefault()` or `SingleOrDefault()` with a predicate to check for specific elements. This works as an alternative for `Any()` in some cases.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = numbers.FirstOrDefault(n => n % 2 == 0) != 0; // True if at least one even number exists
  ```

---

## 4\. **Aggregate()**

`Aggregate()` allows custom logic to evaluate conditions across the sequence, making it a versatile tool.

* **Alternative for `Any()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = numbers.Aggregate(false, (acc, n) => acc || n % 2 == 0); // True if at least one even number exists
  ```

* **Alternative for `All()`**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = numbers.Aggregate(true, (acc, n) => acc \&\& n % 2 == 0); // True if all numbers are even
  ```

---

## 5\. **Manual Loops**

Manual iteration allows complete control and can replace both `Any()` and `All()` in highly customized scenarios.

* **Alternative for `Any()`**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasEven = false;
  foreach (var n in numbers)
  {
      if (n % 2 == 0)
      {
          hasEven = true;
          break;
      }
  }
  Console.WriteLine(hasEven); // True if at least one even number exists
  ```

* **Alternative for `All()`**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = true;
  foreach (var n in numbers)
  {
      if (n % 2 != 0)
      {
          allEven = false;
          break;
      }
  }
  Console.WriteLine(allEven); // True if all numbers are even
  ```

---

## 6\. **TakeWhile() and SkipWhile()**

These methods can be used to evaluate conditions by processing elements until a condition is no longer met.

* **Alternative for `All()`**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = numbers.TakeWhile(n => n % 2 == 0).Count() == numbers.Count; // True if all numbers are even
  ```

---

## 7\. **Any() with Negation**

To replace `All()`, you can sometimes combine `Any()` with negation to determine if any element violates the condition.

* **Example**:

```csharp
  var numbers = new List<int> { 2, 4, 6 };
  bool allEven = !numbers.Any(n => n % 2 != 0); // True if all numbers are even
  ```

---

## 8\. **Contains()**

`Contains()` checks if a specific value exists in the collection. While not a direct replacement for `Any()`, it can help in simpler scenarios.

* **Example**:

```csharp
  var numbers = new List<int> { 1, 2, 3, 4 };
  bool hasNumberTwo = numbers.Contains(2); // True if the number 2 exists in the sequence
  ```

---

## Comparison of Methods:

| \*\*Alternative\*\*   | \*\*Use Case\*\*    |
|-------------------------|-------------------------------------------------|
| `Count()`               | When you want to evaluate the count of matching elements. |
| `FirstOrDefault()`      | To check if a specific element exists.          |
| `Aggregate()`           | For custom logic across the sequence.           |
| `Where()`               | Combined with other methods for filtering checks.|
| `TakeWhile()`/`SkipWhile()` | For evaluating conditions until they're broken. |
| `Contains()`            | For simple membership checks.                   |

---

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
==============================================
---

