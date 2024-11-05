# **Prototype** 
> *also known as* `Clone`

Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

## Usage examples:
The Prototype pattern is available in C# out of the box with a ICloneable interface.

## Identification:
The prototype can be easily recognized by a clone or copy methods, etc.

All prototype classes should have a common interface that makes it possible to copy objects even if their concrete classes are unknown. Prototype objects can produce full copies since objects of the same class can access each other’s private fields.

> This pattern allows you to create new objects by copying an existing object, known as the prototype, rather than creating a new instance from scratch.

## Scenario
We'll create a scenario with a Shape class that can be cloned. This class will have different shapes, such as Circle and Rectangle, and we will use the prototype pattern to create new shapes based on existing ones.

```cs
namespace PrototypePattern
{
    public interface IShape
    {
        IShape Clone();
        void Draw();
    }

    public class Circle : IShape
    {
        public string Color { get; set; }

        public Circle(string color)
        {
            Color = color;
        }

        public IShape Clone()
        {
            return new Circle(Color);
        }

        public void Draw()
        {
            Console.WriteLine($"Drawing a {Color} circle.");
        }
    }

    public class Rectangle : IShape
    {
        public string Color { get; set; }

        public Rectangle(string color)
        {
            Color = color;
        }

        public IShape Clone()
        {
            return new Rectangle(Color);
        }

        public void Draw()
        {
            Console.WriteLine($"Drawing a {Color} rectangle.");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Create original shapes
            IShape circle = new Circle("Red");
            IShape rectangle = new Rectangle("Blue");

            // Clone shapes
            IShape clonedCircle = circle.Clone();
            IShape clonedRectangle = rectangle.Clone();

            // Draw original and cloned shapes
            circle.Draw();          // Output: Drawing a Red circle.
            clonedCircle.Draw();    // Output: Drawing a Red circle.

            rectangle.Draw();       // Output: Drawing a Blue rectangle.
            clonedRectangle.Draw();  // Output: Drawing a Blue rectangle.
        }
    }



}
```

## Summary
- **Prototype Interface**: IShape defines the Clone method for cloning objects.
- **Concrete Prototypes**: Circle and Rectangle implement the IShape interface, allowing cloning and drawing.
- **Client Code**: Demonstrates how to create original shapes, clone them, and draw both the original and cloned shapes.

---
---
