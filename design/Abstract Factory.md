# **Abstract Factory**
> Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

The Abstract Factory Pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. 
It is particularly useful when your code needs to work with multiple types of products that share a common interface.

## Scenario
Let’s say we are building a UI toolkit that can produce different themes: Dark and Light. Each theme has a corresponding set of UI components, like Button and TextBox.

## Usage examples:
The Adapter pattern is pretty common in C# code. It’s very often used in systems based on some legacy code. In such cases, Adapters make legacy code work with modern classes.

## Identification:
Adapter is recognizable by a constructor which takes an instance of a different abstract/interface type. When the adapter receives a call to any of its methods, it translates parameters to the appropriate format and then directs the call to one or several methods of the wrapped object.

The Abstract Factory interface declares a set of methods that return
different abstract products. These products are called a family and are
related by a high-level theme or concept. Products of one family are
usually able to collaborate among themselves. A family of products may
have several variants, but the products of one variant are incompatible
with products of another.

```cs
namespace AbstractFactoryPattern 
{

    //define interfaces
    public interface IButton
    {
        void Render();
    }

    public interface ITextBox
    {
        void Render();
    }

    //dark theme
    public class DarkButton : IButton
    {
        public void Render()
        {
            Console.WriteLine("Rendering a dark button.");
        }
    }

    public class DarkTextBox : ITextBox
    {
        public void Render()
        {
            Console.WriteLine("Rendering a dark text box.");
        }
    }

    //light theme
    public class LightButton : IButton
    {
        public void Render()
        {
            Console.WriteLine("Rendering a light button.");
        }
    }

    public class LightTextBox : ITextBox
    {
        public void Render()
        {
            Console.WriteLine("Rendering a light text box.");
        }
    }


    //define abstract factory interface
    public interface IUIFactory
    {
        IButton CreateButton();
        ITextBox CreateTextBox();
    }

    public class DarkThemeFactory : IUIFactory
    {
        public IButton CreateButton()
        {
            return new DarkButton();
        }

        public ITextBox CreateTextBox()
        {
            return new DarkTextBox();
        }
    }

    public class LightThemeFactory : IUIFactory
    {
        public IButton CreateButton()
        {
            return new LightButton();
        }

        public ITextBox CreateTextBox()
        {
            return new LightTextBox();
        }
    }


    class Program
    {
        static void Main(string[] args)
        {
            IUIFactory factory;

            // Uncomment one of the lines below to choose the theme
            // factory = new DarkThemeFactory();
            factory = new LightThemeFactory();

            IButton button = factory.CreateButton();
            ITextBox textBox = factory.CreateTextBox();

            button.Render();   // Output will depend on the selected theme
            textBox.Render();  // Output will depend on the selected theme
        }
    }
    
}
```
## Summary

- **Shape Interface**: IShape defines the method for drawing shapes.
- **Concrete Shapes**: ColorCircle, ColorSquare, BWCirlce, and BWSquare implement the IShape interface.
- **Abstract Factory Interface:** IShapeFactory defines methods for creating shapes.
- **Concrete Factories**: ColorShapeFactory and BWShapeFactory create color and black-and-white shapes, respectively.
- **Client Code**: Demonstrates how to use the factory to create shapes without knowing their concrete classes.
