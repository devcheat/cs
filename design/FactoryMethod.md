# **Factory Method**
> Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

This pattern defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.

## Scenario
We'll create a simple application for different types of shapes (e.g., Circle and Square) and use a factory method to create these shapes.


## Usage examples:
 The Factory Method pattern is widely used in C# code. It’s very useful when you need to provide a high level of flexibility for your code.

## Identification:
Factory methods can be recognized by creation methods that construct objects from concrete classes. While concrete classes are used during the object creation, the return type of the factory methods is usually declared as either an abstract class or an interface.


The Factory Method defines a method, which should be used for creating objects instead of using a direct constructor call (new operator). Subclasses can override this method to change the class of objects that will be created.

```cs
namespace FactoryPattern
{
    public interface IShape
    {
        void Draw();
    }

    public class Circle : IShape
    {
        public void Draw()
        {
            Console.WriteLine("Drawing a Circle.");
        }
    }

    public class Square : IShape
    {
        public void Draw()
        {
            Console.WriteLine("Drawing a Square.");
        }
    }

    public abstract class ShapeFactory
    {
        public abstract IShape CreateShape();
    }

    public class CircleFactory : ShapeFactory
    {
        public override IShape CreateShape()
        {
            return new Circle();
        }
    }

    public class SquareFactory : ShapeFactory
    {
        public override IShape CreateShape()
        {
            return new Square();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            ShapeFactory circleFactory = new CircleFactory();
            ShapeFactory squareFactory = new SquareFactory();

            IShape circle = circleFactory.CreateShape();
            IShape square = squareFactory.CreateShape();

            circle.Draw(); // Output: Drawing a Circle.
            square.Draw(); // Output: Drawing a Square.
        }
    }
}
```

## Summary
- **Shape Interface**: IShape defines a method for drawing shapes.
- **Concrete Shape Classes**: Circle and Square implement the IShape interface.
- **Creator Class**: ShapeFactory defines the factory method CreateShape.
- **Concrete Creator Classes**: CircleFactory and SquareFactory implement the factory method to create specific shape instances.
- **Client Code**: Demonstrates how to use the factory to create and draw shapes.
