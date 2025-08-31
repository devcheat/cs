# **Projection**

Projection transforms data into a new form.

* **Example Methods**:

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
