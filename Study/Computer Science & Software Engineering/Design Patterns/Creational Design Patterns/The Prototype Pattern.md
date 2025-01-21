## The Problem

Let's think of a scenario where we need create a lot of similar objects. If we have a concrete definition of a class, you would have to repetitively follow it up with a lot of `new Class()..` calls.

If you have to create an exact copy of an object, you can create a new object of that class and copy all of the properties, and their values into the new object - which is not always possible, because some of the object's properties may be private - and not visible from outside of the object.

Another problem with the direct approach, is that you have to know the class of an object as well - and your code has an extra dependency now. Sometimes, you only know the interface, and not the concrete implementation.

## The Solution

The *Prototype* pattern solves exactly this problem - it delegates the cloning process to the actual object which is being cloned. 

You start off with a common interface for all of the objects that need to support cloning - which more often, would contain just a single clone method. This clone method, when called, would return a new copy of the object, with all the properties and their values from the previous instance.

Here, we call the object that is being cloned a *Prototype*. When your objects have a significant amount of fields, and multiple configurations - cloning them might serve as an alternative to subclassing.
## Example

We start by defining a prototype interface:

```Java
public interface Shape {
	
	Shape clone();
	
	void draw();
}
```

We then implement the interface with a Concrete prototype definition:

```Java
public class Circle extends Shape {
	private String color;

	public Circle(String color){
		this.color = color;
	}
	
	@Override
	public Shape clone() {
		return new Circle(this.color);
	}

	@Override
	public void draw() {
		System.out.println("Drawing a " + this.color + " circle.");
	}
}
```

Here's how we would use this:

```Java
public class PrototypeExample {
    public static void main(String[] args) {
    
        // Create a concrete prototype (a red circle).
        Shape circlePrototype = new Circle("red");

        // Use the prototype to create a new shape (a red circle).
        Shape redCircle = circlePrototype.clone();

        // Draw the newly created red circle.
	    Shape anotherRedCircle =  circlePrototype.clone();
    }
}
```