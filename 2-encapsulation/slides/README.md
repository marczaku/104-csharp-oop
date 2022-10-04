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

