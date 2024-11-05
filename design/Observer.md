# **Observer**
> Also known as: **Event-Subscriber**, **Listener**

## Usage examples:
The Observer pattern is pretty common in C# code, especially in the GUI components. It provides a way to react to events happening in other objects without coupling to their classes.

## Identification:
The pattern can be recognized by subscription methods, that store objects in a list and by calls to the update method issued to objects in that list.

Observer is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

> This pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.


```cs
namespace ObserverPattern
{
    public interface IObserver
    {
        void Update(float temperature);
    }

    public interface ISubject
    {
        void RegisterObserver(IObserver observer);
        void RemoveObserver(IObserver observer);
        void NotifyObservers();
    }

    public class WeatherStation : ISubject
    {
        private readonly List<IObserver> _observers = new List<IObserver>();
        private float _temperature;

        public void RegisterObserver(IObserver observer)
        {
            _observers.Add(observer);
        }

        public void RemoveObserver(IObserver observer)
        {
            _observers.Remove(observer);
        }

        public void NotifyObservers()
        {
            foreach (var observer in _observers)
            {
                observer.Update(_temperature);
            }
        }

        public void SetTemperature(float temperature)
        {
            _temperature = temperature;
            NotifyObservers();
        }
    }

    public class PhoneDisplay : IObserver
    {
        public void Update(float temperature)
        {
            Console.WriteLine($"Phone display updated: Temperature is {temperature}°C.");
        }
    }

    public class DesktopDisplay : IObserver
    {
        public void Update(float temperature)
        {
            Console.WriteLine($"Desktop display updated: Temperature is {temperature}°C.");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            var weatherStation = new WeatherStation();

            var phoneDisplay = new PhoneDisplay();
            var desktopDisplay = new DesktopDisplay();

            weatherStation.RegisterObserver(phoneDisplay);
            weatherStation.RegisterObserver(desktopDisplay);

            // Change temperature and notify observers
            weatherStation.SetTemperature(25.0f);
            weatherStation.SetTemperature(30.0f);
        }
    }
}
```

## Summary
- Observer Interface: IObserver defines the Update method for observers.
- Subject Interface: ISubject defines methods for registering, removing, and notifying observers.
- Concrete Subject: WeatherStation implements the subject interface, maintaining a list of observers and notifying them of changes.
- Concrete Observers: PhoneDisplay and DesktopDisplay implement the observer interface and update their displays based on temperature changes.
- Client Code: Demonstrates how to register observers and notify them of changes in the subject's state.- 