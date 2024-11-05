# **Chain of Responsibility**
> Also known as: CoR, Chain of Command

Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

## Usage examples:
The Chain of Responsibility is pretty common in C#. It’s mostly relevant when your code operates with chains of objects, such as filters, event chains, etc.

## Identification:
The pattern is recognizable by behavioral methods of one group of objects that indirectly call the same methods in other objects, while all the objects follow the common interface.

> This pattern allows a request to be handled by multiple handlers in a chain, and each handler decides whether to process the request or pass it to the next handler.

## Scenario
Let's say we have a system that handles support requests at different levels: TechnicalSupport, BillingSupport, and GeneralSupport.

```cs
namespace BehavioralPatterns.ChainOfResponsibility
{
  public interface ISupportHandler
  {
      void SetNext(ISupportHandler handler);
      void HandleRequest(string request);
  }

  public class TechnicalSupport : ISupportHandler
  {
      private ISupportHandler _nextHandler;

      public void SetNext(ISupportHandler handler)
      {
          _nextHandler = handler;
      }

      public void HandleRequest(string request)
      {
          if (request == "Technical")
          {
              Console.WriteLine("Technical Support: Handling technical request.");
          }
          else if (_nextHandler != null)
          {
              _nextHandler.HandleRequest(request);
          }
      }
  }

  public class BillingSupport : ISupportHandler
  {
      private ISupportHandler _nextHandler;

      public void SetNext(ISupportHandler handler)
      {
          _nextHandler = handler;
      }

      public void HandleRequest(string request)
      {
          if (request == "Billing")
          {
              Console.WriteLine("Billing Support: Handling billing request.");
          }
          else if (_nextHandler != null)
          {
              _nextHandler.HandleRequest(request);
          }
      }
  }

  public class GeneralSupport : ISupportHandler
  {
      private ISupportHandler _nextHandler;

      public void SetNext(ISupportHandler handler)
      {
          _nextHandler = handler;
      }

      public void HandleRequest(string request)
      {
          Console.WriteLine("General Support: Handling general request.");
      }
  }

  class Program
  {
      static void Main(string[] args)
      {
          ISupportHandler techSupport = new TechnicalSupport();
          ISupportHandler billingSupport = new BillingSupport();
          ISupportHandler generalSupport = new GeneralSupport();

          techSupport.SetNext(billingSupport);
          billingSupport.SetNext(generalSupport);

          // Test different requests
          techSupport.HandleRequest("Technical"); // Handled by Technical Support
          techSupport.HandleRequest("Billing");    // Handled by Billing Support
          techSupport.HandleRequest("Other");      // Handled by General Support
      }
  }
}
```

## Summary
- Handler Interface: ISupportHandler defines methods for setting the next handler and handling requests.
- Concrete Handlers: TechnicalSupport, BillingSupport, and GeneralSupport implement the interface and handle specific types of requests.
- Client Code: Sets up the chain and tests it with different requests.