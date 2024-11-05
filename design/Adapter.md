Adapter
===

Adapter is a structural design pattern, which allows incompatible objects to collaborate.

The Adapter acts as a wrapper between two objects. It catches calls for one object and transforms them to format and interface recognizable by the second object.

## Usage examples:
The Adapter pattern is pretty common in C# code. It’s very often used in systems based on some legacy code. In such cases, Adapters make legacy code work with modern classes.

## Identification:
Adapter is recognizable by a constructor which takes an instance of a different abstract/interface type. When the adapter receives a call to any of its methods, it translates parameters to the appropriate format and then directs the call to one or several methods of the wrapped object.

> The Adapter Design Pattern allows objects with incompatible interfaces to work together. 
 It acts as a bridge between two incompatible interfaces.

## Scenario:
Let's say you have a Car interface and a Truck class. You want to create an adapter that allows a Truck to be treated as a Car.

```cs
namespace AdapterPattern 
{
    public interface ICar
    {
        void Drive();
    }

    public class Truck
    {
        public void DriveTruck()
        {
            Console.WriteLine("The truck is driving.");
        }
    }

    //Create the Adapter
    public class TruckAdapter : ICar
    {
        private readonly Truck _truck;

        public TruckAdapter(Truck truck)
        {
            _truck = truck;
        }

        public void Drive()
        {
            // Adapting the method to call the truck's method
            _truck.DriveTruck();
        }
    }

    //Use the Adapter
    class Program
    {
        static void Main(string[] args)
        {
            Truck truck = new Truck();
            ICar truckAdapter = new TruckAdapter(truck);

            // Using the adapter to drive the truck
            truckAdapter.Drive(); // Output: The truck is driving.
        }
    }
}
```

## Summary
- **ICar**: Defines a common interface for cars.
- **Truck**: Represents a class with its own method that is incompatible with the ICar interface.
- **TruckAdapter**: Implements the ICar interface and adapts the Truck class so it can be used as a Car.
- **Program**: Demonstrates how to use the adapter to drive the truck via the ICar interface.

This pattern is useful when you want to integrate new functionality into an existing system without modifying the original code.