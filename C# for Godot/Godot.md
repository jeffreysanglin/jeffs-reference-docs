These are my notes of items that I've learned using Godot and its tutorials.

## Tutorials
- [All Godot tutorials](https://github.com/godotengine/godot-demo-projects/tree/master)
- [C# Godot Tutorials](https://github.com/godotengine/godot-demo-projects/tree/master/mono) (not all tutorials are in C#)

## Creating a new signal inside Godot
1. Use the following code template inside your class:
```
	[Signal]
	public delegate void FlimFlammedEventHandler();

	public void Floors()
	{
		EmitSignal(SignalName.FlimFlammed);
		QueueFree();
	}
```
Replace "FlimFlammed" in the delegate name and after `SignalName`. It will appear in your "Node > Signal" tab after rebuilding.
