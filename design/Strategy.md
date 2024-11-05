# **Strategy**

Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

## Usage examples:
The Strategy pattern is very common in C# code. It’s often used in various frameworks to provide users a way to change the behavior of a class without extending it.

## Identification:
Strategy pattern can be recognized by a method that lets a nested object do the actual work, as well as a setter that allows replacing that object with a different one.

> This pattern enables selecting an algorithm's behavior at runtime, allowing for flexibility and the ability to change the behavior of an object.

## Scenario
We'll create a simple payment processing system where we can switch between different payment strategies, such as credit card and PayPal.

```cs
namespace StrategyPattern
{
    public interface IPaymentStrategy
    {
        void Pay(decimal amount);
    }

    public class CreditCardPayment : IPaymentStrategy
    {
        public void Pay(decimal amount)
        {
            Console.WriteLine($"Paid {amount:C} using Credit Card.");
        }
    }

    public class PayPalPayment : IPaymentStrategy
    {
        public void Pay(decimal amount)
        {
            Console.WriteLine($"Paid {amount:C} using PayPal.");
        }
    }

    public class ShoppingCart
    {
        private IPaymentStrategy _paymentStrategy;

        public void SetPaymentStrategy(IPaymentStrategy paymentStrategy)
        {
            _paymentStrategy = paymentStrategy;
        }

        public void Checkout(decimal amount)
        {
            _paymentStrategy.Pay(amount);
        }
    }

    public class Program
    {
        static void Main(string[] args)
        {
            var cart = new ShoppingCart();

            // Pay using Credit Card
            cart.SetPaymentStrategy(new CreditCardPayment());
            cart.Checkout(100.00m); // Output: Paid $100.00 using Credit Card.

            // Pay using PayPal
            cart.SetPaymentStrategy(new PayPalPayment());
            cart.Checkout(150.00m); // Output: Paid $150.00 using PayPal.
        }
    }
}
```

## Summary
- **Strategy Interface:** IPaymentStrategy defines the interface for payment strategies.
- **Concrete Strategies:** CreditCardPayment and PayPalPayment implement the payment strategies.
- **Context Class:** ShoppingCart uses a payment strategy and can switch between different strategies at runtime.
- **Client Code:** Demonstrates how to set the payment strategy and perform checkout.