# **Decorator**
Decorator is a structural pattern that allows adding new behaviors to objects dynamically by placing them inside special wrapper objects, called decorators.

Using decorators you can wrap objects countless number of times since both target objects and decorators follow the same interface. The resulting object will get a stacking behavior of all wrappers.

## Usage examples:
The Decorator is pretty standard in C# code, especially in code related to streams.

## Identification:
Decorator can be recognized by creation methods or constructors that accept objects of the same class or interface as a current class.

This pattern allows you to dynamically add new behavior to objects by placing them inside special wrapper objects, called decorators.

## Scenario
We'll create a basic coffee order system where we can add various condiments (like milk and sugar) to a coffee.

```cs
namespace DecoratorPattern
{
    public interface ICoffee
    {
        string GetDescription();
        double GetCost();
    }

    public class SimpleCoffee : ICoffee
    {
        public string GetDescription()
        {
            return "Simple Coffee";
        }

        public double GetCost()
        {
            return 1.00; // base cost of simple coffee
        }
    }

    public abstract class CoffeeDecorator : ICoffee
    {
        protected ICoffee _coffee;

        protected CoffeeDecorator(ICoffee coffee)
        {
            _coffee = coffee;
        }

        public virtual string GetDescription()
        {
            return _coffee.GetDescription();
        }

        public virtual double GetCost()
        {
            return _coffee.GetCost();
        }
    }

    public class MilkDecorator : CoffeeDecorator
    {
        public MilkDecorator(ICoffee coffee) : base(coffee) { }

        public override string GetDescription()
        {
            return _coffee.GetDescription() + ", Milk";
        }

        public override double GetCost()
        {
            return _coffee.GetCost() + 0.50; // cost of milk
        }
    }

    public class SugarDecorator : CoffeeDecorator
    {
        public SugarDecorator(ICoffee coffee) : base(coffee) { }

        public override string GetDescription()
        {
            return _coffee.GetDescription() + ", Sugar";
        }

        public override double GetCost()
        {
            return _coffee.GetCost() + 0.25; // cost of sugar
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            ICoffee coffee = new SimpleCoffee();
            Console.WriteLine($"{coffee.GetDescription()} costs {coffee.GetCost()}");

            // Add milk
            coffee = new MilkDecorator(coffee);
            Console.WriteLine($"{coffee.GetDescription()} costs {coffee.GetCost()}");

            // Add sugar
            coffee = new SugarDecorator(coffee);
            Console.WriteLine($"{coffee.GetDescription()} costs {coffee.GetCost()}");
        }
    }
}
```

## Summary
- **Component Interface:** ICoffee defines the methods for coffee.
- **Concrete Component**: SimpleCoffee implements the base coffee.
- **Decorator Base Class**: CoffeeDecorator provides the basic functionality for decorators.
- **Concrete Decorators**: MilkDecorator and SugarDecorator add additional behavior (condiments).
- **Client Code**: Demonstrates how to create a coffee and add condiments dynamically.