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
