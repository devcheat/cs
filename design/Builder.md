# **Builder**

Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

> Unlike other creational patterns, Builder doesn’t require products to have a common interface. That makes it possible to produce different products using the same construction process.



## Usage:
 The Builder pattern is a well-known pattern in C# world. It’s especially useful when you need to create an object with lots of possible configuration options.

## Identification:
The Builder pattern can be recognized in a class, which has a single creation method and several methods to configure the resulting object. Builder methods often support chaining (for example, someBuilder.setValueA(1).setValueB(2).create()).

The Builder interface specifies methods for creating the different parts of the Product objects.

## Example:
We'll use a Pizza class to demonstrate how to build a pizza with various optional ingredients.
```cs
namespace CreationalPatterns.BuilderPattern
{

  public class Pizza
  {
      public string Size { get; set; }
      public bool HasCheese { get; set; }
      public bool HasPepperoni { get; set; }
      public bool HasMushrooms { get; set; }

      public override string ToString()
      {
          return $"Pizza Size: {Size}, Cheese: {HasCheese}, Pepperoni: {HasPepperoni}, Mushrooms: {HasMushrooms}";
      }
  }

  public class PizzaBuilder
  {
      private Pizza _pizza = new Pizza();

      public PizzaBuilder SetSize(string size)
      {
          _pizza.Size = size;
          return this;
      }

      public PizzaBuilder AddCheese()
      {
          _pizza.HasCheese = true;
          return this;
      }

      public PizzaBuilder AddPepperoni()
      {
          _pizza.HasPepperoni = true;
          return this;
      }

      public PizzaBuilder AddMushrooms()
      {
          _pizza.HasMushrooms = true;
          return this;
      }

      public Pizza Build()
      {
          return _pizza;
      }
  }

  class Program
  {
      static void Main(string[] args)
      {
          var pizza = new PizzaBuilder()
              .SetSize("Large")
              .AddCheese()
              .AddPepperoni()
              .Build();

          Console.WriteLine(pizza);
      }
  }
}
```
## Summary
- **Pizza Class**: Represents the product being built.
- **PizzaBuilder Class**: Provides methods to configure the pizza and a Build method to create the final object.
- **Client Code**: Demonstrates how to use the builder to create a customized pizza.