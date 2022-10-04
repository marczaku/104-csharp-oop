# Slides 2 - Encapsulation

## 7. Access Modifiers
```cs
public class Animal {
// Private is only accessible by this class
// Everything is private per default
  Color color;
// public is accessible by everyone
  public string name;
  // protected is only accessible by this class and inheritors
  protected int age;
  
  public Animal(int id) {
    this.id = id;
  }

  public void SetAge(int value) {
    this.age = value;
  }
}
static void Main(string[]args) {
var animal = new Animal(5);
  // ERROR: Cannot access private field
  animal.id = 3;
  // Valid: name is a public field
  animal.name = "Adam";
  // ERROR: cannot access protected field
  animal.age = 5;
  // Valid: SetAge is a public method
  animal.SetAge(5);
}
```

Access modifiers are used to limit access to class-Members.\
Sometimes, you don't want other classes to fiddle around with your internal values.\
- e.g. you don't want a class to just set the player's weapon to `null`
- or you don't want other classes to read your application's Security Token

Also, you want to make sure, that other classes only see, what they need to see about your class.
- it makes your classes appear more simple, than they actually are
- which makes it easier to use it
- think about Bluetooth Headphones
  - do you understand, how they work internally?
  - or are you happy, that you only need a Power and a Connect Button to use them?

You can use access modifiers on classes, fields, method, properties.\
Just put the access modified before it.
- `private` (default): available to owning class only
- `protected`: available to owning class and inheriting classes (more on that later)
- `public`: available to all classes
- `internal`: same as `public`, but only within the same Project
  - We will see later, that we can actually have Solutions consisting of multiple Projects!

Let's see, how they each work:

This code sample is invalid, because the default access modifier is `private`\
And we cannot access implicitly `private` Members from outside the class:

```cs
public class Person {
   string name;
}

static void Main() {
   Person max = new Person();
   max.name = "Max";
}
```

This code sample is invalid.\
We cannot access explicitly `private` Members from outside the class:

```cs
public class Person {
   private string name;
}

static void Main() {
   Person max = new Person();
   max.name = "Max";
}
```

This code sample is valid.\
We can access `public` Members from outside the class:

```cs
public class Person {
   public string name;
}

static void Main() {
   Person max = new Person();
   max.name = "Max";
}
```

Let's have a real-world code-sample for Access Modifiers:

```cs
public class Circle {
  // radius is private to avoid other classes of assigning invalid values
  double radius;
  
  // area is private to avoid other classes of assigning other values (which are not really the area)
  double area;
  
  // SetRadius is a public method that you can use to set the radius
  // It validates the radius and updates the area as well
  public void SetRadius(double radius) {
    if (radius <= 0)
      return;
    this.radius = radius;
    this.area = Math.PI * radius * radius;
  }
  
  // GetRadius is a public method to read the radius
  public double GetRadius() {
    return this.radius;
  }
  
  // GetArea is a public method to read the area
  public double GetArea() {
    return this.area;
  }
}
```

Which one to use?
- Make as much use of `private` as possible
  - It offers less confusion to users of your class, if they can only change values that make sense
  - It makes your code more easy to change, because other classes can not rely on `private` fields / methods
  - It prevents other classes to „break“ your classes through invalid changes
  
- Use `protected` if you use inheritance
  - And only, if you expect inheriting classes to need access to a certain field / method
  
- Use `public` for everything you want to make available
  - It should be your conscious intention to make sth. public

- In the beginning:
  - The easiest is, to just make everything `public`, you have to put less thought into it, then.

---


## 8. Properties


Properties are used in `C#` to Replace Getter and Setter Method.\
They are basically just syntactic sugar!\
You could say, that they are Methods, which look like Fields.
- With Methods, you can use Code for Validation, Processing, etc.
- You can define a getter and a setter separately

So, to compare:\
This is a field:
```cs
public class Unit {
   public int health;
}

Unit unit = new Unit();
unit.health = 5;
// I can assign any value to it and it will have that value.
unit.health = -100;
Console.WriteLine(unit.health); // Output: -100
```

This is Getter and Setter Methods:
```cs
public class Unit {
   private int health;
   public void SetHealth(int value) {
      health = Math.Max(0, value);
   }
   public int GetHealth() {
      return health;
   }
}

Unit unit = new Unit();
unit.SetHealth(5);
// Now, when I assign invalid values, they can be fixed:
unit.SetHealth(-100);
Console.WriteLine(unit.GetHealth()); // Output: 0
// I cannot assign `health` directly:
// unit.health = -100;
```

And now, using Properties:
```cs
public class Unit {
   private int health;
   public int Health {
      set {
         health = Math.Max(0, value);
      }
      get {
         return health;
      }
   }
}

Unit unit = new Unit();
unit.Health = 5;
// Now, when I assign invalid values, they can be fixed:
unit.Health = -100;
Console.WriteLine(unit.Health); // Output: 0
// I cannot assign `health` directly:
// unit.health = -100;
```

Do you see, how one part behaves like Methods:
```cs
set {
   health = Math.Max(0, value);
}
```

While the other behaves like Fields:
```cs
unit.Health = -100;
Console.WriteLine(unit.Health);
```


Syntax:
```cs
public PropertyType PropertyName {
     get { 
        // a function that returns PropertyType 
     }
     set { 
        // a function that receives a local variable named `value` of type PropertyType 
     }
}
```
                
```cs
public class Circle {
  // a private field, so it's inaccessible
  int radius;
  
  // a traditional Setter-Method
  public void SetRadius(int value) {
    this.radius = value;
  }
  // a traditional Getter-method
  public int GetRadius() {
    return this.radius;
  }
  
  // a property with the same functionality
  public int Radius {
    get {
      // The getter method has to return to an int
      return this.radius;
    }
    set {
      // The setter method gets a local int "value"
      this.radius = value;
    }
  }
}
```

- Since Properties act as two separe methods…
- You may have only a Getter, or only a Setter
- Or your getter may be public
- And your setter private at the same time
- There‘s also a nice expression-body syntax:
  -  get => `referenceToValue;`
  -  set => `referenceToValue = value;`

```cs
public class Circle {
   int radius;
   
   // properties may have only a getter
   public int Radius {
      get {
         return this.radius;
      }
   }
   
   // or only a setter
   public int Radius1 {
      set {
         this.radius = value;
     }
   }
   
   // or different access modifiers:
   public int Radius2 {
     get {
       return.this.radius;
     }
     private set {
       this.radius = value;
     }
   }

   // and you can use expression-bodies:
   public int Radius3 {
     get => this.radius;
     set => this.radius = value;
  }
}
```


- Want to save even more space?
- Use Auto-Properties:
  - They have a hidden field for the value
  - It‘s popular to name a backing field, that‘s supposed to only be changed through a Property with Underscore: _variableName 
  - It signalizes, that it should not be accessed directly; But it does not prevent it (within the class)

```cs
public class Circle {
  // This is an auto-property
  // it stores the value in a hidden field that you cannot access in any other way
  
  public int Radius {
    get;
    private set;
  }
}
// the above class is the same as:
public class AnotherCircle {
  int _radius;
  public int Radius {
    get {
      return this._radius;
    }
    private set {
      this._radius = value;
    }
  }
}
```

- Also Useful: Read-Only Properties.
- They only have a `get`
- But they can be assigned within the Constructor

```cs
public class Person {
  // Read-Only Property
  public string Name {
    get;
  }
  
  public Person(string name) {
    // we can still assign to the name within the constructor:
    this.Name = name;
  }
  
  public void ChangeName(string newName) {
    // But this is not possible anymore outside the constructor:
    // this.Name = name;
  }
}
```

---

