# Slides 2 - Encapsulation

Encapsulation allows us to restrict access to internal data (fields) and functions (methods) of classes and how this feature enables us to validate input, ensure the integrity of the internals of our class and even do complex calculations every time an information is requested.

## 7. Access Modifiers

### Public

```cs
public class Person
{
  public string name;
  public void Greet()
  {
    Console.WriteLine($"Hello, my name is {name}.");
  }
}
```

So far, we have used the `public` access modifier a lot for:
- classes
- methods
- fields

But why?

```cs
public class Person {}
```

We made classes public, so it would be accessible from other files within our project.

```cs
Person person = new Person();
person.name = "Hans";
Console.WriteLine(person.name);
public class Person {
  public string name;
}
```

We made fields public, so we'd be able to access the fields from outside the class' body. Both for
- assigning a value
- reading the value

```cs
Person person = new Person();
person.SayHello();
public class Person {
  public void SayHello(){
    Console.WriteLine("Hello.");
  }
}
```

And we made methods public so we'd be able to invoke them from outside the class.

### Private

```cs
Person person = new Person();
person.name = "Hans"; // ERROR: Cannot access private field
Console.WriteLine(person.name); // ERROR: Cannot access private field
public class Person{
  private string name;
}
```

If we use the `private` Keyword on a field, it means that it cannot be accessed outside the class anymore for
- reading the value
- assigning a value

```cs
public class Person{
  private int comboCounter;
  private void Attack(Enemy enemy){
    enemy.TakeDamage(1);
    comboCounter++;
  }
}
```

But we can still access the field from other members within the class.

### Getter & Setter

```cs
Person person = new Person();
person.SetName("Hans");
Console.WriteLine(person.GetName());
public class Person{
  private string name;
  public void GetName(){
    return name;
  }
  public void SetName(string value){
    name = value;
  }
}
```

If you want to allow other classes to be able to 
- read the value of a private field
- assign a value to the private field

You can create so-called Getter and Setter Methods, which are usually named
- `Get{FieldName}`
- `Set{FieldName}`
- where `{FieldName}` is replaced with the name of the field

But why would you do that?

### Data Validation

```cs
Person person = new Person();
person.SetName("Hans");
person.SetName("");
Console.WriteLine(person.GetName());
public class Person{
  private string name;
  public void GetName(){
    return name;
  }
  public void SetName(string value){
    if(string.IsNullOrEmpty(value)){
      Console.WriteLine("Error. Can not assign an invalid name to Person.");
      return;
    }
    name = value;
    Console.WriteLine($"Successfully changed the name to: {value}.");
  }
}
```

Output:
```
Successfully changed the name to: Hans.
Error. Can not assign an invalid name to Person.
Hans
```

You can use it to validate the value that the user of a class wants to assign to your Field.\
This is important to avoid users from introducing bugs by accident and only noticing it when it's too late.

### Data Calculation

```cs
Person person = new Person();
person.SetName("Hans");
Console.WriteLine(person.GetName());
person.Promote();
Console.WriteLine(person.GetName());
person.SetName("Will");
Console.WriteLine(person.GetName());
public class Person{
  private string name;
  private bool isPromoted;

  public void GetName(){
    if(isPromoted){
      return "Officer " + name;
    }
    return name;
  }

  public void SetName(string value){
    if(string.IsNullOrEmpty(value)){
      Console.WriteLine("Error. Can not assign an invalid name to Person.");
      return;
    }
    name = value;
    Console.WriteLine($"Successfully changed the name to: {value}.");
  }

  public void Promote(){
    isPromoted = true;
    Console.WriteLine($"Promoted {Hans}.")
  }
}
```

Output:
```
Successfully changed the name to: Hans.
Hans
Promoted Hans.
Officer Hans
Successfully changed the name to: Will.
Officer Will
```

You can calculate the actual value of a Property whenever it is accessed.\
In above example, the actual displayed name is calculated when `GetName` is invoked:
- Officer Hans, if Hans got promoted.
- Hans, if Hans has not gotten promoted, yet.

### Hiding Complexity

```
Next
NextBytes
NextDouble
NextInt64
NextSingle
```

Also, you want to hide unnecessary complexity. Above, you can see all the Methods that are publicly accessible in System.Random class.

```
MBIG
MSEED
MZ
inext
inextp
SeedArray
Sample
InternalSample
GetSampleForLargeRange
```

And above are a few more members which are accessible only within the class.

It's much nicer and easier to use, if we have less options. This makes a good and clean so-called interface of a class.

- think about Bluetooth Headphones
  - do you understand, how they work internally?
  - or are you happy, that you only need a Power and a Connect Button to use them?

### protected and internal

These modifiers are not so important at this point and are intentionally left out.

### Which one to use?
- Make as much use of `private` as possible
  - It offers less confusion to users of your class, if they can only change values that make sense
  - It makes your code more easy to change, because other classes can not rely on `private` fields / methods
  - It prevents other classes to „break“ your classes through invalid changes
  
- Use `public` for everything you want to make available
  - It should be your conscious intention to make sth. public

- In the beginning:
  - The easiest is, to just make everything `public`, you have to put less thought into it, then.

---


## 8. Properties

Properties are here to replace Getter and Setter Methods in C#.\
Let's look at an example:

### Problem: Fields allow for no control

```cs
public class Unit {
   public int health;
}
```

I can assign any value to the field `health` now. The class has no control over it:

```cs
Unit unit = new Unit();
unit.health = 5;
unit.health = -100;
Console.WriteLine(unit.health); // -100
```

Output:
```
-100
```

### Solution: Getter and Setter Methods

This solves the problem using Getter and Setter methods:
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
```

Now, I can not assign values to the field directly anymore:

```cs
Unit unit = new Unit();
// unit.health = -100; ERROR, this is private
```

Instead, I need to use `SetHealth` and `GetHealth`.
- `SetHealth` corrects the input and ensures that `health` can never fall under 0

```cs
Unit unit = new Unit();
unit.SetHealth(5);
unit.SetHealth(-100);
Console.WriteLine(unit.GetHealth()); // 0
```

Output:
```
0
```

### Problem: Readability

The problem is, that the Code can become quite unreadable:

Before:

```cs
Unit unit = new Unit();
unit.health += 20;
```

Now:

```cs
Unit unit = new Unit();
unit.SetHealth(unit.GetHealth() + 20);
```

### Solution: Using Properties

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
```

We still can not access the private field `health`:

```cs
Unit unit = new Unit();
// unit.health = -100; ERROR, this is private
```

But we can use the `get` and `set` methods of the `public` `Health` Property:

```cs
Unit unit = new Unit();
unit.Health = 5;
unit.Health = -100;
Console.WriteLine(unit.Health); // Output: 0
```

Whenever you assign to the Unit's Health Property:

```cs
unit.Health = -100;
```

The Unit's `set` Method gets invoked, passing the value of the right side of the assignment (`-100`) to the Parameter named `value`:

```cs
   public int Health {
      set {
         health = Math.Max(0, value);
      }
      // ...
```

Actually, this:

```cs
   public int Health {
      set {
         health = Math.Max(0, value);
      }
   }
```

Gets compiled to this:

```cs
   public void SetHealth(int value){
     health = Math.Max(0, value);
   }
```

But the advantage is that Properties can be used like fields, so we can write this:

```cs
unit.Health += 20;
```

Instead of:

```cs
unit.SetHealth(unit.GetHealth() + 20);
```

### Definition

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

### Samples

Here, the Circle Class uses the `Radius` Property in order to update both the `radius` and the `area` of the Circle whenever a new value is assigned to the `Radius` Property:

```cs
public class Circle {
  private float radius;
  private float area;

  public float Radius {
    get{
      return radius;
    }
    set{
      radius = value;
      area = value * value * MathF.PI;
    }
  }

  public float Area {
    get{
      return area;
    }
  }
}
```

```cs
Circle circle = new Circle();
circle.Radius = 12;
Console.WriteLine(circle.Radius); // 12
Console.WriteLine(circle.Area); // 152.38...
```

Output:
```
12
152.38
```

### Separate Getter and Setter

You might have spotted it already in above example, but:

```cs
  public float Area {
    get{
      return area;
    }
  }
```

The `Area` Property has no `set`-Accessor at all. This is possible with Properties.
- You can not use `Area` on the left side of an assignment!
- But you can use it on the right side of an assignment
- And to pass the value to Methods

```cs
Circle circle = new Circle();
// circle.Area = 12; // ERROR: Area has no set Accessor
Console.WriteLine(circle.Area);
float area = circle.Area;
```

You can also have a Property which only has a `set`-Accessor:

```cs
public class Login{
  private string password;
  public string Password{
    set{
      password = value;
    }
  }
}
```

In above example, I can assign a value to `Password`:

```cs
Login login = new Login();
login.Password = "12345";
```

But I can not read the value:

```cs
//Console.WriteLine(login.Password); // ERROR: Password has no get Accessor
```

### Separate Getter and Setter Accessibility

Either of your getter and setter can also have less accessibility than the Property:

```cs
public class Unit{
  private int health;
  public int Health{
    get{
      return health;
    }
    private set{
      health = Math.Max(0, value);
    }
  }
}
```

`Health` is a `public` Property and has a `get`-Accessor, so we can access it outside the class:

```cs
Unit unit = new Unit();
Console.WriteLine(unit.Health);
```

`Health` has a `set`-Accessor, but it is private. Which means that we cannot access it:

```cs
//unit.Health = 12; // ERROR: set is not accessible
```

### Expression Body Syntax

If either your getter or setter fit into one line of code, you can also use the shorter syntax:

Before:

```cs
public int Health{
  get{
    return health;
  }
  set{
    health = Math.Max(0, value);
  }
}
```

After:

```cs
public int Health{
  get => health;
  set => health = Math.Max(0, value);
}
```

### Expression Bodied Property

And if your Property only has a getter, it can even be further shortened:

```cs
public class Circle{
  float radius;
  public float Area{
    get{
      return radius * radius * MathF.PI;
    }
  }
}
```

Can be reduced to:

```cs
public class Circle{
  float radius;
  public float Area => radius * radius * MathF.PI;
}
```

Isn't that cool?

### Auto-Implemented Properties

If your Properties only forward the value of a field, you can have them be auto-implemented:

```cs
public class Circle{
  private float radius;
  public float Radius{
    get{
      return radius;
    }
    set{
      radius = value;
    }
  }
}
```

Can be reduced to:

```cs
public class Circle{
  public float Radius{get;set;}
}
```

And if you have different access modifiers, that's possible, too:

```cs
public class Player{
  private float health;
  public float Health{
    get{
      return health;
    }
    private set{
      health = value;
    }
  }
}
```

Can be reduced to:

```cs
public class Player{
  public float Health{ get;private set; }
}
```

### Read-Only Property

You can even have Properties that are Read-Only:

```cs
public class Player{
  public string Id {get;}
}
```

```cs
Player player = new Player();
// player.Id = "Player1"; // ERROR: Property `Id` has no set-Accessor.
Console.WriteLine(player.Id); // empty output
```

Output:
```

```

What's the use of a value that can never be assigned to? Won't it always have its default value (`null`, `0`, `false`)?

Actually, you can assign to it! But only in the class's constructor:

```cs
public class Player{
  public string Id {get;}

  public Player(string id){
    Id = id;
  }
}

That way, you can (and need to) pass the Id to the `Player` class and can never ever change it again:

```cs
Player player = new Player("Player1");
// player.Id = "Player2"; // ERROR: Property `Id` has no set-Accessor.
Console.WriteLine(player.Id); // Player1
```

Output:
```
Player1
```