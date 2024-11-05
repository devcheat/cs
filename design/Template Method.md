# **Template Method**

Template Method is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

## Usage examples:
The Template Method pattern is quite common in C# frameworks. Developers often use it to provide framework users with a simple means of extending standard functionality using inheritance.

## Identification:
Template Method can be recognized if you see a method in base class that calls a bunch of other methods that are either abstract or empty.

> This pattern defines the skeleton of an algorithm in a base class but allows subclasses to override specific steps without changing the algorithm's structure.

## Scenario
We'll create a class that prepares a drink. The process of making a drink will involve steps that are common (like boiling water), but the actual drink type (like tea or coffee) will differ.

```cs
namespace TemplatePattern
{

    public abstract class DrinkTemplate
    {
        // Template method
        public void PrepareDrink()
        {
            BoilWater();
            AddMainIngredient();
            PourInCup();
            AddCondiments();
        }

        private void BoilWater()
        {
            Console.WriteLine("Boiling water...");
        }

        private void PourInCup()
        {
            Console.WriteLine("Pouring into cup...");
        }

        // Abstract methods to be implemented by subclasses
        protected abstract void AddMainIngredient();
        protected abstract void AddCondiments();
    }

    public class Tea : DrinkTemplate
    {
        protected override void AddMainIngredient()
        {
            Console.WriteLine("Adding tea leaves.");
        }

        protected override void AddCondiments()
        {
            Console.WriteLine("Adding lemon.");
        }
    }

    public class Coffee : DrinkTemplate
    {
        protected override void AddMainIngredient()
        {
            Console.WriteLine("Adding coffee grounds.");
        }

        protected override void AddCondiments()
        {
            Console.WriteLine("Adding sugar and milk.");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            DrinkTemplate tea = new Tea();
            tea.PrepareDrink();
            Console.WriteLine();

            DrinkTemplate coffee = new Coffee();
            coffee.PrepareDrink();
        }
    }
}
```
## Summary
Abstract Class: DrinkTemplate defines the template method PrepareDrink, which outlines the steps to prepare a drink.
Concrete Classes: Tea and Coffee implement the specific steps for their respective drinks.
Client Code: Demonstrates how to prepare different drinks using the template method.