# **Iterator**

Iterator is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

## Usage examples:
The pattern is very common in C# code. Many frameworks and libraries use it to provide a standard way for traversing their collections.


## Identification:
Iterator is easy to recognize by the navigation methods (such as next, previous and others). Client code that uses iterators might not have direct access to the collection being traversed.

> This pattern provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

## Scenario
We'll create a custom collection of Book objects and provide an iterator to traverse the collection.

```cs
namespace Iterator
{

  public class Book
  {
      public string Title { get; }

      public Book(string title)
      {
          Title = title;
      }
  }

  public interface IBookIterator
  {
      bool HasNext();
      Book Next();
  }

  public interface IBookCollection
  {
      IBookIterator CreateIterator();
  }

  using System.Collections.Generic;

  public class BookCollection : IBookCollection
  {
      private readonly List<Book> _books = new List<Book>();

      public void AddBook(Book book)
      {
          _books.Add(book);
      }

      public IBookIterator CreateIterator()
      {
          return new BookIterator(this);
      }

      public int Count => _books.Count;

      public Book GetBook(int index)
      {
          return _books[index];
      }
  }

  public class BookIterator : IBookIterator
  {
      private readonly BookCollection _collection;
      private int _current = 0;

      public BookIterator(BookCollection collection)
      {
          _collection = collection;
      }

      public bool HasNext()
      {
          return _current < _collection.Count;
      }

      public Book Next()
      {
          return _collection.GetBook(_current++);
      }
  }

  class Program
  {
      static void Main(string[] args)
      {
          var bookCollection = new BookCollection();
          bookCollection.AddBook(new Book("The Great Gatsby"));
          bookCollection.AddBook(new Book("1984"));
          bookCollection.AddBook(new Book("To Kill a Mockingbird"));

          var iterator = bookCollection.CreateIterator();

          while (iterator.HasNext())
          {
              var book = iterator.Next();
              Console.WriteLine(book.Title);
          }
      }
  }

}
```
## Summary
- **Book Class**: Represents the items in the collection.
Iterator Interface: IBookIterator defines methods for traversing the collection.
- **Aggregate Interface**: IBookCollection defines a method for creating an iterator.
- **Concrete Collection**: BookCollection implements the aggregate interface and holds a list of books.
- **Concrete Iterator**: BookIterator implements the iterator interface to traverse the book collection.
- **Client Code**: Demonstrates how to use the iterator to access the book titles.