**Bridge**
===
Bridge is a structural design pattern that divides business logic or huge class into separate class hierarchies that can be developed independently.

One of these hierarchies (often called the Abstraction) will get a reference to an object of the second hierarchy (Implementation). The abstraction will be able to delegate some (sometimes, most) of its calls to the implementations object. Since all implementations will have a common interface, they’d be interchangeable inside the abstraction.

## Usage examples:
The Bridge pattern is especially useful when dealing with cross-platform apps, supporting multiple types of database servers or working with several API providers of a certain kind (for example, cloud platforms, social networks, etc.)

## Identification:
Bridge can be recognized by a clear distinction between some controlling entity and several different platforms that it relies on.

> The Bridge Design Pattern is used to separate an abstraction from its implementation, allowing them to vary independently. Here’s a simple example in C# that demonstrates how the Bridge pattern works.

## Scenario
Imagine we want to create a drawing application that can draw shapes using different rendering methods (e.g., Raster and Vector). The Shape will be the abstraction, and the rendering methods will be the implementations.

```cs
namespace StructuralPatterns.Bridge
{

   public interface IRenderer
   {
      void RenderCircle(float radius);
      void RenderSquare(float side);
   }

   public class RasterRenderer : IRenderer
   {
      public void RenderCircle(float radius) => 
         Console.WriteLine($"Raster Circle with radius {radius}.");
      
      public void RenderSquare(float side) => 
         Console.WriteLine($"Raster Square with side {side}.");
   }

   public class VectorRenderer : IRenderer
   {
      public void RenderCircle(float radius) => 
         Console.WriteLine($"Vector Circle with radius {radius}.");
      
      public void RenderSquare(float side) => 
         Console.WriteLine($"Vector Square with side {side}.");
   }

   public abstract class Shape
   {
      protected IRenderer Renderer;

      protected Shape(IRenderer renderer) => Renderer = renderer;

      public abstract void Draw();
   }

   public class Circle : Shape
   {
      private float _radius;

      public Circle(IRenderer renderer, float radius) : base(renderer) => 
         _radius = radius;

      public override void Draw() => Renderer.RenderCircle(_radius);
   }

   public class Square : Shape
   {
      private float _side;

      public Square(IRenderer renderer, float side) : base(renderer) => 
         _side = side;

      public override void Draw() => Renderer.RenderSquare(_side);
   }


   class Program
   {
      static void Main(string[] args)
      {
         IRenderer raster = new RasterRenderer();
         IRenderer vector = new VectorRenderer();

         Shape circle = new Circle(raster, 5);
         Shape square = new Square(vector, 4);

         circle.Draw(); // Raster Circle with radius 5.
         square.Draw(); // Vector Square with side 4.
      }
   }
}
```

## Summary
- Renderer Interface: IRenderer defines rendering methods.
- Concrete Renderers: RasterRenderer and VectorRenderer implement rendering.
- Shape Abstraction: Shape is the abstract class with a reference to IRenderer.
- Concrete Shapes: Circle and Square implement drawing.
- Client Code: Demonstrates how to draw shapes using different renderers.