## The Problem

Suppose you have a scenario where you have to create families of related objects - Say, a class which is dependent on many other related class - without specifying their concrete classes - a different object is created by grouping any combination of related objects. Managing these groups of classes which should be used together is a problem while ensuring that the client code is decoupled from the Concrete Classes.

## The Solution

The *Abstract Factory* design pattern can help here. Instead of creating the family of object with Concrete class constructor calls, you delegate this functionality to another class, a *Factory* class which will be used for producing these families of objects.

**Note**: It's easy to get confused between the *Factory* method pattern, and *Abstract* method pattern - since both serve similar purposes. However there are a few differences:
 - Firstly, the Factory Method is a *method*, while Abstract Factory is a *class*.
 - Factory Method pattern uses *inheritance* - to allow subclasses to handle the desired object instantiation - and the object . Abstract factory relies on composition - which means, that the client code creates a factory method - and then uses that factory object to create new objects.
 - Another important point to note here is that the convention is to use the factory method to use for creating *single* objects - while the Abstract Factory will be used to create *families* of related objects.
## Example

Suppose we have a scenario, where we have a user interface. The user interface is made of components, like Button and Scroll Bar. Now, additionally, there is a colour scheme functionality - so when a user selects Red colour for user interface - the UI Components must correspond to the selected colour scheme.

Here's an implementation of the above scenario:

- First, define abstract and concrete Product classes:
```Java
// Button and ScrollBar interfaces

public interface Button {
    String getColor();
}

public interface ScrollBar {
    String getColor();
}

// Concrete classes
public class RedButton implements Button {
    @Override
    public String getColor() {
        return "RED";
    }
}
public class RedScrollBar implements ScrollBar {
    @Override
    public String getColor() {
        return "RED";
    }
}

public class BlueButton implements Button {
    @Override
    public String getColor() {
        return "BLUE";
    }
}

public class BlueScrollBar implements ScrollBar {
    @Override
    public String getColor() {
        return "BLUE";
    }
}

// UserInterface class
public class UserInterface {
    private Button button;

    private ScrollBar scrollBar;

    public UserInterface(Button button, ScrollBar scrollBar) {
        this.button = button;
        this.scrollBar = scrollBar;
    }

    public Button getButton() {
        return button;
    }

    public ScrollBar getScrollBar() {
        return scrollBar;
    }

    @Override

    public String toString() {
        return "UserInterface{" +
                "button=" + button.getColor() +
                ", scrollBar=" + scrollBar.getColor() +

                '}';
    }

}
```

- Next, we define the `UserInterfaceFactory` interfaces:
```Java

public interface UserInterfaceFactory {
  public abstract Button createButton();

  public abstract ScrollBar createScrollBar();

  public abstract UserInterface createUserInterface();

}
// Concrete implementations of UserInterfaceFactory
public final class RedUserInterfaceFactory implements UserInterfaceFactory {

  public Button createButton(){
    return new RedButton();
  }

  public ScrollBar createScrollBar(){
    return new RedScrollBar();
  }

  public UserInterface createUserInterface() {
    return new UserInterface(createButton(),createScrollBar());
  }
}

public final class BlueUserInterfaceFactory implements UserInterfaceFactory {

  public Button createButton() {
    return new BlueButton();
  }

  public ScrollBar createScrollBar(){
    return new BlueScrollBar();
  }

  public UserInterface createUserInterface(){
    return new UserInterface(createButton(),createScrollBar());
  }
}
```

- Finally, A `UserInterfaceFactoryMaker` class - this returns the appropriate factory based on the selection from client code:
```Java
import java.util.*;
import java.util.function.*;

public class UserInterfaceFactoryMaker {

  private static Map<String, Supplier<UserInterfaceFactory>> factoryTypes = new HashMap<String, Supplier<UserInterfaceFactory>>();

  static {
    factoryTypes.put("RED", RedUserInterfaceFactory::new);
    factoryTypes.put("BLUE", BlueUserInterfaceFactory::new);
  }

  public UserInterfaceFactory createUserInterfaceFactory(String color) {
    if (factoryTypes.get(color) != null) {
      return factoryTypes.get(color).get();
    } else {
      return null;
    }
  }
}
```
- here is the client code - so neat and clean!!
```Java
public class App {
    public static void main(String[] args) {
        UserInterface roadUserInterface = createUserInterface("RED");
        UserInterface mountainUserInterface = createUserInterface("BLUE");
        
        System.out.println(roadUserInterface);
        System.out.println(mountainUserInterface);
    }

    private static UserInterface createUserInterface(String color) {
        UserInterfaceFactoryMaker factoryMaker = new UserInterfaceFactoryMaker();

        UserInterfaceFactory factory = factoryMaker.createUserInterfaceFactory(color);

        return factory.createUserInterface();
    }
}
```