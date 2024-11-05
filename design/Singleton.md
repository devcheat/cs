# **Singleton**

> Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

Singleton has almost the same pros and cons as global variables. Although they’re super-handy, they break the modularity of your code.

## Usage examples:
A lot of developers consider the Singleton pattern an antipattern. That’s why its usage is on the decline in C# code.

## Identification:
Singleton can be recognized by a static creation method, which returns the same cached object.

This Singleton implementation is called "double check lock". It is safe
in multithreaded environment and provides lazy initialization for the
Singleton object.

> A Singleton ensures that a class has only one instance and provides a global point of access to it.

## Example
We'll use the double-checked locking method to ensure thread safety and performance.

```cs
namespace SingletonPattern
{
    public class Singleton
    {
        private static Singleton _instance;
        private static readonly object _lock = new object();

        // Private constructor to prevent instantiation from outside
        private Singleton() { }

        public static Singleton Instance
        {
            get
            {
                // First check (no locking)
                if (_instance == null)
                {
                    lock (_lock) // Locking to ensure thread safety
                    {
                        // Second check (with locking)
                        if (_instance == null)
                        {
                            _instance = new Singleton();
                        }
                    }
                }
                return _instance;
            }
        }

        public void SomeBusinessLogic()
        {
            // Example business logic
            Console.WriteLine("Executing some business logic.");
        }
    }

    public class Program
    {
        static void Main(string[] args)
        {
            // Accessing the Singleton instance
            Singleton instance1 = Singleton.Instance;
            instance1.SomeBusinessLogic();

            Singleton instance2 = Singleton.Instance;
            instance2.SomeBusinessLogic();

            // Check if both instances are the same
            Console.WriteLine($"Are both instances the same? {instance1 == instance2}");
        }
    }
}
```
## Summary
- **Singleton Class**: Singleton contains a private static instance and a private constructor to prevent external instantiation.
- **Thread Safety**: The Instance property uses double-checked locking to ensure that only one instance is created, even when accessed from multiple threads.
- **Client Code**: Demonstrates how to access the Singleton instance and ensure that multiple accesses yield the same instance.