## The Problem

As classes grow in size, constructors to initialize instances become even more complex, since as the number of properties increases, the number of constructors parameters also increases - which results in messy constructor calls , and consequently, to unreadable code.

### The Solution

We can solve this problem in object oriented code by utilizing a design pattern called a *Builder Pattern* - where, instead of calling the "Class" Constructor directly, we call a "ClassBuilder" constructor, and use it to initialize the original class and any properties that are needed.

## Example

Below are the steps to implement the *Builder Pattern* for a class:
- Inside the class, create a nested static builder class. A common convention is to name the builder class is to suffix the original name of the class by *Builder*, e.g., PersonBuilder.
- Make the original class constructor private - it should not be accessible directly. And, modify the original class constructor to take a single parameter - the Builder instance.
- Declare all the original class properties inside the builder class.
- Inside the builder class, define a constructor, that takes only the mandatory properties as parameters - and provide methods to initialize the optional properties.
- Provide a build method, which calls the original class constructor and returns the instance of original class.

For example, consider the code below:

```Java
public class StoreItem
{

    private final String name;
    private final Double price;
    private final String shortDescription;
    private final String longDescription;
    private final Integer stockAvailable;
    private final String packagingType;

    public StoreItem(String name, Double price, String shortDescription, String longDescription, Integer stockAvailable, String packagingType) {

        this.name = name;
        this.price = price;
        this.shortDescription = shortDescription;
        this.longDescription = longDescription;
        this.stockAvailable = stockAvailable;
        this.packagingType = packagingType;
        if (name == null || price == null) {
            throw new IllegalArgumentException("Name and price must not be null");
        }
    }
}
```

Here is the Builder pattern implemented for the above class:

```Java
public class StoreItem
{
    private final String name;
    private final Double price;
    private final String shortDescription;
    private final String longDescription;
    private final Integer stockAvailable;
    private final String packagingType;

    private StoreItem(StoreItemBuilder storeItemBuilder) {
		if (storeItemBuilder.name == null || storeItemBuilder.price == null) {
            throw new IllegalArgumentException("Name and price must not be null");
        }
        this.name = storeItemBuilder.name;
        this.price = storeItemBuilder.price;
        this.shortDescription = storeItemBuilder.shortDescription;
        this.longDescription = storeItemBuilder.longDescription;
        this.stockAvailable = storeItemBuilder.stockAvailable;
        this.packagingType = storeItemBuilder.packagingType;
        
    }
	public static class StoreItemBuilder {
		private final String name;
	    private final Double price;
	    private final String shortDescription;
	    private final String longDescription;
	    private final Integer stockAvailable;
	    private final String packagingType;
	    
	    public StoreItembuilder(String name, Double price){
		    this.name= name;
		    this.price= price
	    }
	    
	    public StoreItemBuilder shortDescription(String shortDescription){
		    this.shortDescription= shortDescription;
		    return this;
	    }
	    
	    public StoreItemBuilder longDescription(String longDescription){
		    this.longDescription= longDescription;
		    return this;
	    }
	    
	    public StoreItemBuilder stockAvailable(String stockAvailable){
		    this.stockAvailable= stockAvailable;
		    return this;
	    }
	    
	    public StoreItemBuilder packagingType(String packagingType){
		    this.packagingType= packagingType;
		    return this;
	    }
	    
	    public StoreItem build(){
			return new StoreItem(this);
	    }
	}
}
```

Comparing instantiation, before, and after implementing the pattern:

**Before**:

```Java
var item1 = new StoreItem(

                "Pretzel", 2.0, "A tasty snack", null, 7, null);

  

        var item2 = new StoreItem(

                "Soup", 1.5, null,

                "A meal that you can warm up at home. It can come in different flavours including tomato, chicken, and vegetable",

                null, "Can");
```

**After**:

```Java
var item1 = new StoreItem.StoreItemBuilder("Pretzel",2.0)
	.shortDescription("A tasty snack)
	.stockAvailable(7)
	.build();

var item2 = new StoreItem.StoreItemBuilder("Soup",1.5)
	.longDescription("A meal that you can warm up at home. It can come in different flavours including tomato, chicken, and vegetable")
	.packagingType("Can")
	.build();
```

The code is much more structured, and readable.