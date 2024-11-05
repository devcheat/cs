# **Mediator**
> Also known as: Intermediary, Controller

Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

## Usage examples:
The most popular usage of the Mediator pattern in C# code is facilitating communications between GUI components of an app. The synonym of the Mediator is the Controller part of MVC pattern.

> This pattern defines an object that encapsulates how a set of objects interact, promoting loose coupling by preventing objects from referring to each other explicitly.

## Scenario
We'll create a chat room where different users can send messages to each other through a mediator (the chat room).

```cs
namespace MediatorPattern
{
	public interface IChatRoom
	{
		void Register(User user);
		void Send(string from, string to, string message);
	}

	public class ChatRoom : IChatRoom
	{
		private readonly Dictionary<string, User> _users = new Dictionary<string, User>();

		public void Register(User user)
		{
			if (!_users.ContainsKey(user.Name))
			{
				_users[user.Name] = user;
				user.SetChatRoom(this);
			}
		}

		public void Send(string from, string to, string message)
		{
			if (_users.ContainsKey(to))
			{
				_users[to].Receive(from, message);
			}
		}
	}

	public class User
	{
		public string Name { get; }
		private IChatRoom _chatRoom;

		public User(string name)
		{
			Name = name;
		}

		public void SetChatRoom(IChatRoom chatRoom)
		{
			_chatRoom = chatRoom;
		}

		public void Send(string to, string message)
		{
			Console.WriteLine($"{Name} sends to {to}: {message}");
			_chatRoom.Send(Name, to, message);
		}

		public void Receive(string from, string message)
		{
			Console.WriteLine($"{Name} received from {from}: {message}");
		}
	}

	class Program
	{
		static void Main(string[] args)
		{
			var chatRoom = new ChatRoom();

			var user1 = new User("Alice");
			var user2 = new User("Bob");
			var user3 = new User("Charlie");

			chatRoom.Register(user1);
			chatRoom.Register(user2);
			chatRoom.Register(user3);

			user1.Send("Bob", "Hello, Bob!");
			user2.Send("Alice", "Hi, Alice!");
			user3.Send("Alice", "Hey, Alice! How are you?");
		}
	}

}
```

## Summary
- **Mediator Interface**: IChatRoom defines methods for registering users and sending messages.
- **Concrete Mediator**: ChatRoom implements the mediator interface, managing user registrations and message passing.
- **Colleague Class**: User represents users in the chat room and uses the mediator to communicate.
- **Client Code**: Demonstrates how users can send and receive messages through the chat room.