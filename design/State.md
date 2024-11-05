# **State**

State is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

## Usage examples:
The State pattern is commonly used in C# to convert massive switch-base state machines into objects.

## Identification:
State pattern can be recognized by methods that change their behavior depending on the objects’ state, controlled externally.

> This pattern allows an object to change its behavior when its internal state changes, essentially allowing it to appear to change its class.

## Scenario
We'll create a simple context for a TrafficLight that can be in one of three states: Red, Yellow, and Green. Each state will define its own behavior for the Change method.

```cs
namespace StatePattern
{

  public interface ITrafficLightState
  {
    void Change(TrafficLight trafficLight);
  }

  public class RedState : ITrafficLightState
  {
      public void Change(TrafficLight trafficLight)
      {
          Console.WriteLine("Changing from Red to Green.");
          trafficLight.SetState(new GreenState());
      }
  }

  public class YellowState : ITrafficLightState
  {
      public void Change(TrafficLight trafficLight)
      {
          Console.WriteLine("Changing from Yellow to Red.");
          trafficLight.SetState(new RedState());
      }
  }

  public class GreenState : ITrafficLightState
  {
      public void Change(TrafficLight trafficLight)
      {
          Console.WriteLine("Changing from Green to Yellow.");
          trafficLight.SetState(new YellowState());
      }
  }

  public class TrafficLight
  {
      private ITrafficLightState _state;

      public TrafficLight()
      {
          _state = new RedState(); // Initial state
      }

      public void SetState(ITrafficLightState state)
      {
          _state = state;
      }

      public void Change()
      {
          _state.Change(this);
      }
  }

  public class Program
  {
      static void Main(string[] args)
      {
          var trafficLight = new TrafficLight();

          // Change the traffic light state
          trafficLight.Change(); // Red to Green
          trafficLight.Change(); // Green to Yellow
          trafficLight.Change(); // Yellow to Red
      }
  }

}
```

## Summary
- **State Interface**: ITrafficLightState defines the Change method for different traffic light states.
- **Concrete States**: RedState, YellowState, and GreenState implement the behavior for each state.
- **Context Class:** TrafficLight maintains the current state and allows state transitions.
- **Client Code**: Demonstrates how to change the traffic light state and see the transitions.