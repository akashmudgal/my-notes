
## The Problem

Suppose you have client code, where you need to to create multiple variant object of a class descending from a same abstract class. So, in your client class, you define some conditional statements - perhaps calling appropriate constructor based on a `type` input to a method in the client class.

This results code which is tightly coupled with the concrete classes - so let's say every time there is a change to the classes, or you need to add some more variants of the classes-  the object creation code also needs to be modified, which becomes a problem as your codebase grows.
## Example

This problem is can be solved by implementing a *Factory Method* pattern. Here is how it works - in your abstract class implementation, you create a so called abstract *factory method* - a single method which is responsible for instantiating your classes, so all your object creation logic is implemented in a single method - which makes it flexible to modify. Also, since the method is abstract, it allows subclasses to control how objects are created as well.

The Factory Method Pattern defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

To summarise, here is what you need to implement the Factory method pattern:

- Creator Classes: These abstract classes contain a special factory method, responsible to create objects. These classes will expose a method to be used for creating objects - the *factory method*.
- Product Classes: The classes which are meant to be instantiated - these are concrete implementations. You optionally may have an abstract Product implementation as well, followed by more concrete Product classes - based on a scenario.

Instead of letting subclasses decide, another approach is a parameterized factory method - where the factory method is given a parameter, based on which it decides what class to instantiate.
## Approach

Here is an example code:

- We define the Product interface and the concrete Product classes:
```Java
// Product Interface
interface Biryani {
    void prepare();
    void cook();
    void pack();
}

// Concrete Products
class ChickenBiryani implements Biryani {
    @Override
    public void prepare() {
        System.out.println("Preparing Chicken Biryani ingredients...");
    }
    @Override
    public void cook() {
        System.out.println("Cooking Chicken Biryani...");
    }
    @Override
    public void pack() {
        System.out.println("Packing Chicken Biryani...");
    }
}

class MuttonBiryani implements Biryani {
    @Override
    public void prepare() {
        System.out.println("Preparing Mutton Biryani ingredients...");
    }
    @Override
    public void cook() {
        System.out.println("Cooking Mutton Biryani...");
    }
    @Override
    public void pack() {
        System.out.println("Packing Mutton Biryani...");
    }
}

class VegBiryani implements Biryani {
    @Override
    public void prepare() {
        System.out.println("Preparing Veg Biryani ingredients...");
    }
    @Override
    public void cook() {
        System.out.println("Cooking Veg Biryani...");
    }
    @Override
    public void pack() {
        System.out.println("Packing Veg Biryani...");
    }
}
```

- Let's say we have stores which specialize in each type of Biryani - We define an abstract `BiryaniStore`, which has an abstract factory method called `createBiryani`. The method is 
```Java
// Creator Abstract Class
abstract class BiryaniStore {
    // Factory Method
    abstract Biryani createBiryani();

    // Common Workflow
    public void serveBiryani() {
        Biryani biryani = createBiryani();
        biryani.prepare();
        biryani.cook();
        biryani.pack();
        System.out.println("Biryani is ready to serve!\n");
    }
}

// Concrete Creators
class ChickenBiryaniStore extends BiryaniStore {
    @Override
    Biryani createBiryani() {
        return new ChickenBiryani();
    }
}

class MuttonBiryaniStore extends BiryaniStore {
    @Override
    Biryani createBiryani() {
        return new MuttonBiryani();
    }
}

class VegBiryaniStore extends BiryaniStore {
    @Override
    Biryani createBiryani() {
        return new VegBiryani();
    }
}
```

- Finally, we have the client code - which utilizes these classes.
```Java
// Client Code
public class BiryaniFactoryMethodExample {
    public static void main(String[] args) {
        BiryaniStore chickenStore = new ChickenBiryaniStore();
        BiryaniStore muttonStore = new MuttonBiryaniStore();
        BiryaniStore vegStore = new VegBiryaniStore();

        System.out.println("Welcome to the Biryani Store!");
        chickenStore.serveBiryani();
        muttonStore.serveBiryani();
        vegStore.serveBiryani();
    }
}

```
