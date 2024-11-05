# **Composite**
Composite is a structural design pattern that allows composing objects into a tree-like structure and work with the it as if it was a singular object.

Composite became a pretty popular solution for the most problems that require building a tree structure. Composite’s great feature is the ability to run methods recursively over the whole tree structure and sum up the results.

## Usage examples:
The Composite pattern is pretty common in C# code. It’s often used to represent hierarchies of user interface components or the code that works with graphs.

## Identification:
If you have an object tree, and each object of a tree is a part of the same class hierarchy, this is most likely a composite. If methods of these classes delegate the work to child objects of the tree and do it via the base class/interface of the hierarchy, this is definitely a composite.

> This pattern allows you to treat individual objects and compositions of objects uniformly.

## Scenario
We’ll create a graphic drawing application where we can manage simple shapes (like Circle and Square) and groups of shapes.

```cs
namespace CompositePattern
{
    public interface IGraphic
    {
        void Draw();
    }

    public class Circle : IGraphic
    {
        public void Draw()
        {
            Console.WriteLine("Drawing a Circle.");
        }
    }

    public class Square : IGraphic
    {
        public void Draw()
        {
            Console.WriteLine("Drawing a Square.");
        }
    }

    public class CompositeGraphic : IGraphic
    {
        private readonly List<IGraphic> _graphics = new List<IGraphic>();

        public void Add(IGraphic graphic)
        {
            _graphics.Add(graphic);
        }

        public void Remove(IGraphic graphic)
        {
            _graphics.Remove(graphic);
        }

        public void Draw()
        {
            Console.WriteLine("Drawing Composite Graphic:");
            foreach (var graphic in _graphics)
            {
                graphic.Draw();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            IGraphic circle = new Circle();
            IGraphic square = new Square();

            CompositeGraphic composite = new CompositeGraphic();
            composite.Add(circle);
            composite.Add(square);

            composite.Draw(); // Output: Drawing Composite Graphic: Drawing a Circle. Drawing a Square.
        }
    }


}
```

## Summary
- **Component Interface**: IGraphic defines the common interface for all graphics.
- **Leaf Classes**: Circle and Square implement the IGraphic interface.
- **Composite Class**: CompositeGraphic can contain other IGraphic objects and implements the Draw method to draw them all.
- **Client Code**: Demonstrates how to create individual shapes and compose them into a composite.