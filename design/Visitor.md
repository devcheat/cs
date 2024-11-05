# **Visitor**

> Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate which allows adding new behaviors to existing class hierarchy without altering any existing code.

This pattern allows you to separate an algorithm from the objects on which it operates, making it easier to add new operations without modifying the objects.

## Scenario
We'll create a structure to represent different types of shapes (like Circle and Rectangle) and use a visitor to calculate the area of these shapes.

```cs
namespace VisitorPattern
{

    public interface IShape
    {
        void Accept(IShapeVisitor visitor);
    }

    public class Circle : IShape
    {
        public double Radius { get; }

        public Circle(double radius)
        {
            Radius = radius;
        }

        public void Accept(IShapeVisitor visitor)
        {
            visitor.Visit(this);
        }
    }

    public class Rectangle : IShape
    {
        public double Width { get; }
        public double Height { get; }

        public Rectangle(double width, double height)
        {
            Width = width;
            Height = height;
        }

        public void Accept(IShapeVisitor visitor)
        {
            visitor.Visit(this);
        }
    }

    public interface IShapeVisitor
    {
        void Visit(Circle circle);
        void Visit(Rectangle rectangle);
    }

    public class AreaCalculator : IShapeVisitor
    {
        public double TotalArea { get; private set; }

        public void Visit(Circle circle)
        {
            TotalArea += Math.PI * circle.Radius * circle.Radius;
        }

        public void Visit(Rectangle rectangle)
        {
            TotalArea += rectangle.Width * rectangle.Height;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            IShape[] shapes = new IShape[]
            {
                new Circle(5),
                new Rectangle(4, 6)
            };

            AreaCalculator areaCalculator = new AreaCalculator();

            foreach (var shape in shapes)
            {
                shape.Accept(areaCalculator);
            }

            Console.WriteLine($"Total Area: {areaCalculator.TotalArea}");
        }
    }

}
```

## Summary
- **Element Interface:** IShape defines the Accept method for the visitor.
- **Concrete Elements:** Circle and Rectangle implement the IShape interface.
- **Visitor Interface:** IShapeVisitor defines methods for visiting different shapes.
- **Concrete Visitor:** AreaCalculator implements the visitor to calculate the area.
- **Client Code:** Demonstrates how to use the visitor pattern to calculate the total area of multiple shapes.