# **Facade** 
Facade is a structural design pattern that provides a simplified (but limited) interface to a complex system of classes, library or framework.

While Facade decreases the overall complexity of the application, it also helps to move unwanted dependencies to one place.

## Usage examples:
The Facade pattern is commonly used in apps written in C#. It’s especially handy when working with complex libraries and APIs.

## Identification:
Facade can be recognized in a class that has a simple interface, but delegates most of the work to other classes. Usually, facades manage the full life cycle of objects they use.

> This pattern provides a simplified interface to a complex subsystem, making it easier to use.

## Scenario
Imagine a home theater system with several components: Amplifier, DVDPlayer, and Projector. We'll create a HomeTheaterFacade to simplify the interaction with these components.

```cs
namespace FacadePattern
{
	public class Amplifier
	{
		public void On() => Console.WriteLine("Amplifier is on.");
		public void SetVolume(int level) => Console.WriteLine($"Volume set to {level}.");
		public void Off() => Console.WriteLine("Amplifier is off.");
	}

	public class DVDPlayer
	{
		public void On() => Console.WriteLine("DVD Player is on.");
		public void Play(string movie) => Console.WriteLine($"Playing '{movie}'.");
		public void Stop() => Console.WriteLine("DVD Player stopped.");
		public void Off() => Console.WriteLine("DVD Player is off.");
	}

	public class Projector
	{
		public void On() => Console.WriteLine("Projector is on.");
		public void SetInput(string input) => Console.WriteLine($"Projector input set to {input}.");
		public void Off() => Console.WriteLine("Projector is off.");
	}

	public class HomeTheaterFacade
	{
		private readonly Amplifier _amplifier;
		private readonly DVDPlayer _dvdPlayer;
		private readonly Projector _projector;

		public HomeTheaterFacade(Amplifier amplifier, DVDPlayer dvdPlayer, Projector projector)
		{
			_amplifier = amplifier;
			_dvdPlayer = dvdPlayer;
			_projector = projector;
		}

		public void WatchMovie(string movie)
		{
			Console.WriteLine("Get ready to watch a movie...");
			_projector.On();
			_projector.SetInput("DVD");
			_amplifier.On();
			_amplifier.SetVolume(5);
			_dvdPlayer.On();
			_dvdPlayer.Play(movie);
		}

		public void EndMovie()
		{
			Console.WriteLine("Shutting movie theater down...");
			_dvdPlayer.Stop();
			_dvdPlayer.Off();
			_amplifier.Off();
			_projector.Off();
		}
	}
	
	public class Program
	{
		static void Main(string[] args)
		{
			Amplifier amplifier = new Amplifier();
			DVDPlayer dvdPlayer = new DVDPlayer();
			Projector projector = new Projector();

			HomeTheaterFacade homeTheater = new HomeTheaterFacade(amplifier, dvdPlayer, projector);

			homeTheater.WatchMovie("Inception"); // Starting the movie
			homeTheater.EndMovie();               // Ending the movie
		}
	}

}
```

## Summary
- **Subsystem Classes**: Amplifier, DVDPlayer, and Projector represent the complex parts of the system.
- **Facade Class**: HomeTheaterFacade provides a simplified interface to the complex subsystems.
- **Client Code**: Demonstrates how to use the facade to manage the home theater system easily.