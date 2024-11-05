# **Command**
Also known as: Action, Transaction
 
Command is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

## Usage examples:
The Command pattern is pretty common in C# code. Most often it’s used as an alternative for callbacks to parameterizing UI elements with actions. It’s also used for queueing tasks, tracking operations history, etc.

## Identification:
The Command pattern is recognizable by behavioral methods in an abstract/interface type (sender) which invokes a method in an implementation of a different abstract/interface type (receiver) which has been encapsulated by the command implementation during its creation. Command classes are usually limited to specific actions.

> This pattern encapsulates a request as an object, allowing you to parameterize clients with queues, requests, and operations.

## Scenario
We'll create a simple remote control that can execute different commands, such as turning on and off a light.

```cs
namespace CommandPattern
{
    public interface ICommand
    {
        void Execute();
    }

    public class Light
    {
        public void TurnOn()
        {
            Console.WriteLine("The light is ON.");
        }

        public void TurnOff()
        {
            Console.WriteLine("The light is OFF.");
        }
    }

    public class TurnOnCommand : ICommand
    {
        private readonly Light _light;

        public TurnOnCommand(Light light)
        {
            _light = light;
        }

        public void Execute()
        {
            _light.TurnOn();
        }
    }

    public class TurnOffCommand : ICommand
    {
        private readonly Light _light;

        public TurnOffCommand(Light light)
        {
            _light = light;
        }

        public void Execute()
        {
            _light.TurnOff();
        }
    }

    public class RemoteControl
    {
        private ICommand _command;

        public void SetCommand(ICommand command)
        {
            _command = command;
        }

        public void PressButton()
        {
            _command.Execute();
        }
    }

    public class Program
    {
        static void Main(string[] args)
        {
            Light light = new Light();
            ICommand turnOn = new TurnOnCommand(light);
            ICommand turnOff = new TurnOffCommand(light);

            RemoteControl remote = new RemoteControl();

            remote.SetCommand(turnOn);
            remote.PressButton(); // Output: The light is ON.

            remote.SetCommand(turnOff);
            remote.PressButton(); // Output: The light is OFF.
        }
    }
}
```

## Summary
- Command Interface: ICommand defines the Execute method.
- Concrete Commands: TurnOnCommand and TurnOffCommand implement the command interface.
- Receiver: The Light class represents the object that will be manipulated.
- Invoker: The RemoteControl class holds the command and invokes it.
- Client Code: Sets up the commands and uses the remote control to execute them.