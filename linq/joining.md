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
