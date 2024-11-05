# **Memento**
> Also known as: Snapshot

Memento is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

## Usage examples:
The Memento’s principle can be achieved using serialization, which is quite common in C#. While it’s not the only and the most efficient way to make snapshots of an object’s state, it still allows storing state backups while protecting the originator’s structure from other objects.

> This pattern allows you to capture and externalize an object's internal state without violating encapsulation, enabling you to restore the object to this state later.

## Scenario
We'll create a text editor that can save and restore its content. The Editor class will maintain a state, and we'll use the Memento pattern to save and restore the content.

```cs
namespace Structural.MementoPattern
{
  public class EditorMemento
  {
      public string Content { get; }

      public EditorMemento(string content)
      {
          Content = content;
      }
  }

  	public class TextEditor
	{
		private string _content;

		public void SetContent(string content)
		{
			_content = content;
		}

		public string GetContent()
		{
			return _content;
		}

		// Create a memento to save the current state
		public EditorMemento Save()
		{
			return new EditorMemento(_content);
		}

		// Restore the state from a memento
		public void Restore(EditorMemento memento)
		{
			_content = memento.Content;
		}
	}

	public class Caretaker
	{
		private readonly List<EditorMemento> _mementos = new List<EditorMemento>();
		
		public void SaveMemento(EditorMemento memento)
		{
			_mementos.Add(memento);
		}

		public EditorMemento GetMemento(int index)
		{
			if (index < 0 || index >= _mementos.Count)
				throw new ArgumentOutOfRangeException("Invalid memento index.");
			
			return _mementos[index];
		}
	}

	class Program
	{
		static void Main(string[] args)
		{
			var textEditor = new TextEditor();
			var caretaker = new Caretaker();

			// Edit content and save states
			textEditor.SetContent("Version 1");
			caretaker.SaveMemento(textEditor.Save());

			textEditor.SetContent("Version 2");
			caretaker.SaveMemento(textEditor.Save());

			textEditor.SetContent("Version 3");
			caretaker.SaveMemento(textEditor.Save());

			// Restore to previous versions
			textEditor.Restore(caretaker.GetMemento(1));
			Console.WriteLine("Current Content: " + textEditor.GetContent()); // Output: Version 2

			textEditor.Restore(caretaker.GetMemento(0));
			Console.WriteLine("Current Content: " + textEditor.GetContent()); // Output: Version 1
		}
	}
}
```

## Summary
- Memento Class: EditorMemento holds the internal state of the TextEditor.
- Originator Class: TextEditor manages its content and can create and restore its memento.
- Caretaker Class: Caretaker keeps track of the mementos and provides access to them.
- Client Code: Demonstrates saving different versions of the content and restoring previous states.