# Slides 3 - Inheritance

Inheritance allow us to share code between classes by putting it into a common base class. It allows for very useful code principles as polymorphism, which ensure that we can write code for all classes that share a common ancestor.

## 9. Inheritance 

### Example without Inheritance

Look at the following classes:

```cs
public class Dog {
  public int Age {get; set;}
  public Dog(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} walks.");}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

```cs
public class Fish {
  public int Age {get; set;}
  public Fish(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

```cs
public class Parrot {
  public int Age {get; set;}
  public Parrot(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Fly(){Console.WriteLine($"{this} age {Age} flys.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

```cs
public class Chicken {
  public int Age {get; set;}
  public Chicken(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

```cs
Dog dog = new Dog(5);
Fish fish = new Fish(2);
Parrot parrot = new Parrot(7);
Chicken chicken = new Chicken(3);

dog.Breathe();
dog.Walk();
dog.Swim();

fish.Breathe();
fish.Swim();

parrot.Breathe();
parrot.Fly();
parrot.Walk();

chicken.Breathe();
chicken.Fly();
chicken.Walk();
```

Output:
```
Dog age 5 brithes.
Dog age 5 walks.
Dog age 5 swims.
Fish age 2 brithes.
Fish age 2 swims.
Parrot age 7 brithes.
Parrot age 7 flys.
Parrot age 7 swims.
Chicken age 3 brithes.
Chicken age 3 flies.
Chicken age 3 swims.
```

### The Problem without Inheritance

That's a couple of classes and each of them has some similar and some different Methods. But we have a lot of duplication, don't we? It might not be so obvious, because each of these Methods is only one line long. But later, these Methods might grow into 20 or 70 or 300 lines of code each!

Have you noticed the awful spelling error? `brithes` instead of `breathes`?\
How many lines of code do we need to change that error? - 4. Once in every class. :(

Also, have you noticed, that in Parrot, it spells `flys` and in Chicken it spells `flies`?\
Which one is correct? In this case that's not the important part. The important part is that we have contradictory information in our code. One time, we spell it one way and the other time another.

### DRY Principle

Both above problems are the reason, why in Programming, we need to follow the DRY Principle actively!

- **D**on't **R**epeat **Y**ourself!

### Inheritance

Inheritance allows us to put Members that different classes have in common into a common base class. That base class needs to have a proper name.

e.g. in above example, all classes have a `Breathe()`-Method and an `Age`-Property. and all classes happen to be animals.

This allows us to introduce a base-class:

```cs
public class Animal{
  public int Age {get; set;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
}
```

And now, we can look at other classes, e.g.:

```cs
public class Fish {
  public int Age {get; set;}
  public Fish(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

And have them inherit from class `Animal` and thereby inheriting all of `Animal`'s Members (`Age` and `void Breathe()`):

```cs
public class Fish : Animal {
  public Fish(int age){Age = age;}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

The class `Fish` still has all of its Members:

```cs
Fish fish = new Fish(2);
fish.Breathe();
fish.Swim();
Console.WriteLine(fish.Age);
```

Output:
```
Fish age 2 brithes.
Fish age 2 swims.
2
```

Same goes for other classes, e.g. `Dog`. Before:

```cs
public class Dog {
  public int Age {get; set;}
  public Dog(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} walks.");}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

After:

```cs
public class Dog : Animal {
  public Dog(int age){Age = age;}
  public void Walk(){Console.WriteLine($"{this} age {Age} walks.");}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

We can take it even further: e.g. `Dog`, `Parrot` and `Chicken` are all terrestrial animals, which can `void Walk()`. And every terrestrial animal is an `Animal` as well:

```cs
public class Terrestrial : Animal {
  public void Walk(){Console.WriteLine($"{this} age {Age} walks.");}
}
```

This reduces e.g. the `Chicken` from:

```cs
public class Chicken {
  public int Age {get; set;}
  public Chicken(int age){Age = age;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

over

```cs
public class Chicken : Animal {
  public Chicken(int age){Age = age;}
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
  public void Walk(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

to:

```cs
public class Chicken : Terrestrial {
  public Chicken(int age){Age = age;}
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
}
```

And the `Dog` is now:

```cs
public class Dog : Terrestrial {
  public Dog(int age){Age = age;}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
```

And while we're at it, `Chicken` and `Parrot` are both birds, which are terrestrial animals, which are animals:

```cs
public class Bird : Terrestrial {
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
}
```

Which reduces the `Chicken` class further to:

```cs
public class Chicken : Bird {
  public Chicken(int age){Age = age;}
}
```

Final code sample:

```cs
public class Animal{
  public int Age {get; set;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} brithes.");}
}
public class Terrestrial : Animal {
  public void Walk(){Console.WriteLine($"{this} age {Age} walks.");}
}
public class Bird : Terrestrial {
  public void Fly(){Console.WriteLine($"{this} age {Age} flies.");}
}
public class Dog : Terrestrial {
  public Dog(int age){Age = age;}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
public class Fish : Animal {
  public Fish(int age){Age = age;}
  public void Swim(){Console.WriteLine($"{this} age {Age} swims.");}
}
public class Parrot : Bird {
  public Chicken(int age){Age = age;}
}
public class Chicken : Bird {
  public Chicken(int age){Age = age;}
}
```

Which makes it a lot easier to fix the spelling error in `void Breathe()`:

```cs
public class Animal{
  public int Age {get; set;}
  public void Breathe(){Console.WriteLine($"{this} age {Age} breathes.");} // not brithes!!
}
```

### Limitation of Inheritance

Problem is: Both the `Dog` and the `Fish` can Swim. But the `Dog` can walk, just like the `Bird`s, while the `Fish` can't...

So either the `Dog` inherits from `Terrestrial`, so he can share the `Walk()`-Method with the birds:

```cs
class Animal{} // breathe
class Fish : Animal {} // swim (breathe)
class Terrestrial : Animal{} // walk (breathe)
class Dog : Terrestrial{} // swim (walk, breathe)
class Bird : Terrestrial{} // fly (walk, breathe)
```

Or the `Dog` could inherit from some `Aquatic` class shared with the `Fish`, but then the `Dog` can't share the ability to `Walk()` with the `Bird`s anymore:

```cs
class Animal{} // breathe
class Aquatic : Animal {} // swim (breathe)
class Terrestrial : Animal{} // walk (breathe)
class Fish : Aquatic {} // -- (swim, breathe)
class Dog : Aquatic{} // walk (swim, breathe)
class Bird : Terrestrial{} // fly (walk, breathe)
```

The only chance to share the ability to `Walk()` with both the `Dog` and the `Bird`s would be by putting it in the `Animal`-class. But then, the `Fish` could suddenly walk :D

```cs
class Animal{} // walk, breathe
class Aquatic : Animal {} // swim (walk, breathe)
class Fish : Aquatic {} // -- (swim, walk, breathe)
class Dog : Aquatic{} // -- (swim, walk, breathe)
class Bird : Animal{} // fly (walk, breathe)
```

### Syntax

- Through Inheritance, it is possible to have a new class Inherit Members from an existing class
- The Base class (parent)
  - Is the class that is inherited FROM
- The Derived class (child)
  - Is the class that is inheriting

```cs
public class DerivedClass : BaseClass 
{
   // derived-class-body
}
```

### Inheriting Instance Members

- The derived class contains both its own Members (fields, methods, properties)
- As well as its parent‘s Members (fields, methods, properties)
- You say, that `Dog` inherits from `Animal`
- Or that `Dog` IS AN `Animal`

#### Methods

The derived class contains both its own methods as well as its parent's methods:

```cs
public class Animal {
   public void Breathe() {
      Console.WriteLine("I'm breathing.");
   }
}
```

```cs
public class Dog : Animal {
   public void Bark() {
      Console.WriteLine("Woof!");
   }
}
```

Now let's see the `Animal`-class in Action:
```cs
Animal animal = new Animal();
animal.Breathe(); // I'm breathing.

// animal.Bark(); ERROR: Animal does not contain a Member named `Bark`
```

And the `Dog`:
```cs
Dog dog = new Dog();
dog.bark(); // Woof!
dog.Breathe(); // I'm breathing.
```

It can even invoke it's parent's methods:

```cs
public class Dog : Animal {
   public void Bark() {
      // we need to breathe before we bark loudly
      Breathe();
      Console.WriteLine("Woof!");
   }
}
```

#### Properties (and Fields)

The inheriting class also inherits its parent's properties:

```cs
public class Animal {
   public int Age {get;set;}
}
```

```cs
public class Dog : Animal {
  public Dog(int age){
    Age = age;
  }

  public void Bark() {
    Console.WriteLine($"I'm {Age} years old, but I can bark!");
  }
}
```

```cs
Dog dog = new Dog(2);
dog.Bark();
Console.WriteLine(dog.Age);
```

Output:
```
I'm 2 years old, but I can bark!
2
```

### Inheriting Static Members

Derived classes inherit their parent's static members as well:

```cs
public class Employee {
  public static int TotalCount {get; set;}
  public static void Sign(){
    Console.WriteLine("Employee signed.");
  }
  public static void Quit(){
    Console.WriteLine("Employee Quit.");
  }
}
```

```cs
public class Programmer : Employee {}
```

You can access static members both through the derived class Name as well as the parent class name:

```cs
Employee.Sign(); // Employee signed.
Programmer.Sign(); // Employee signed.
```

Static Member Properties are shared between all classes inheriting from the base class. Including the base class itself:

```cs
Employee.Sign(); // Employee signed.
Console.WriteLine(Employee.Count); // 1
Console.WriteLine(Programmer.Count); // 1
Programmer.Sign(); // Employee signed.
Console.WriteLine(Employee.Count); // 2
Console.WriteLine(Programmer.Count); // 2
```

Output:
```
Employee signed.
1
1
Employee signed.
2
2
```

The inheriting classes can also access the static Members without prefixing the Class-Name:

```cs
public class Programmer {
  public Programmer(){
    Employee.Sign(); // works
    Programmer.Sign(); // works
    Sign(); // works, too!
  }
}
```

```cs
Programmer programmer = new Programmer();
```

Output:
```
Employee signed.
Employee signed.
Employee signed.
```


### Providing a Base-Constructor

Inheriting classes need to provide a compatible constructor to their base class.

```cs
public class Animal {
  public int Age {get; set;}
  public Animal(int age){
    Age = age;
  }
}
```

```cs
public class Dog : Animal {
  public Dog(int age) : base(age){

  }
}
```

```cs
Dog dog = new Dog(2);
Console.WriteLine(dog.Age);
```

Output:
```
2
```

The base class always gets constructed first:

```cs
public class Animal {
  public int Age {get; set;}
  public Animal(int age){
    Console.WriteLine("Animal gets constructed.");
    Age = age;
  }
}
```

```cs
public class Dog : Animal {
  public Dog(int age) : base(age){
    Console.WriteLine("Dog gets constructed.");
    Age = 5;
  }
}
```

```cs
Dog dog = new Dog(2);
Console.WriteLine(dog.Age);
```

Output:
```
Animal gets constructed.
Dog gets constructed.
5
```

If you don't provide a constructor, your code won't compile!

```cs
public class Animal {
  public int Age {get; set;}
  public Animal(int age){
    Age = age;
  }
}
```

```cs
public class Dog : Animal {
}
// ERROR: Base class 'Animal' does not contain parameter-less constructor
```


A parameter-less constructor does not need to be explicitly invoked:

```cs
public class Animal{
  public Animal(){
    Console.WriteLine("Animal got constructed.");
  }
}
```

```cs
public class Dog{
  public Dog(){
    Console.WriteLine("Dog got constructed.");
  }
}
```

```cs
Dog dog = new Dog();
```

Output:
```
Animal got constructed.
Dog got constructed.
```

### Encapsulation

Inheriting classes can not access the parent's class's private members:

```cs
public class Animal{
  private int Age{get;set;}
}
```

```cs
public class Dog : Animal {
  public Dog(int age){
    //Age = age; // ERROR: Cannot access private member
  }
}
```

#### Making it public?

You could fix this by making the parent Member public:

```cs
public class Animal{
  public int Age{get;set;}
}
```

```cs
public class Dog : Animal {
  public Dog(int age){
    Age = age;
  }
}
```

But that defeats the purpose of Encapsulation. Now, any class can change the `Age`-Property:

```cs
Dog dog = new Dog(3);
dog.Age = 5;
```

#### protected

By using `protected`, you can mark a Member to be inaccessible by other classes, but still accessible by all inheriting classes:

```cs
public class Animal{
  protected int Age{get;set;}
}
```

```cs
public class Dog : Animal {
  public Dog(int age){
    Age = age; // WORKS NOW!
  }
}
```

```cs
Dog dog = new Dog(3);
//dog.Age = 5; ERROR: Can not access protected Member.
//Console.WriteLine(dog.Age); ERROR: Can not access protected Member.
```

To make above example a bit more useful, we could change the Property to be `public` but have a `protected` setter:

```cs
public class Animal{
  public int Age{get; protected set;}
}
```

```cs
public class Dog : Animal {
  public Dog(int age){
    Age = age; // WORKS STILL!
  }
}
```

```cs
Dog dog = new Dog(3);
Console.WriteLine(dog.Age); // WORKS NOW!
//dog.Age = 5; ERROR: protected get Accessor is not accessible.
```

### sealed

Classes marked with the `sealed` Keyword can not be inherited from.
- this can improve performance slightly
- and can prevent misuse of your classes

```cs
public sealed class Animal {
  public void Live() {
    Console.WriteLine("I'm a living animal.");
  }
}
```

```cs
// ERROR: Cannot inherit from sealed class Animal
public class Dog : Animal { }
```

### No Multiple Inheritance

- By the way:
  - In C#, Classes can only inherit from one class at a time
  
  
```cs
// a class for swim and underwater breathing
public class AquaticAnimal { }
```

```cs
// a class for mammal breeding logic
public class Mammal { }
```

```cs
// ERROR: the dolphin cannot inherit from both classes at the same time :(
public class Dolphin : AquaticAnimal, Mammal { } 
```

---

## 10. Polymorphism 

```cs
public class Animal { }
public class Dog : Animal { }
```

- The derived class can be act as base class as well.
- In this case, a `Dog` can be used as an `Animal` as well.

Which means, that you can pass a `Dog` into a method that requires an `Animal` as an argument:
```cs
public void Pet(Animal animal) { }
```
```cs
Dog dog = new Dog();
Pet(dog);
```

And you can assign a `Dog` to a variable of type `Animal`
    - every `Dog` is an `Animal`...
```cs
Dog dog = new Dog();
Animal animal = dog;
```

But you cannot assign `Animal` to a variable of type `Dog` 
    - not every `Animal` is a `Dog`
    
```cs
Animal animal = new Dog();
Dog dog = animal;
```

You can cast an `Animal` to a `Dog`, though, but more on that later:
   - Note: you will get Errors like `null`, if you're trying to cast an `Animal` to a `Dog`, if it's actually a `Cat`...
```cs
Animal animal = new Dog();
Dog dog = animal as Dog;
```

Let's have a look at the previous example again:
```cs
public class Animal {
   public void Live() {
      Console.WriteLine("I'm a living animal.");
   }
}

// class Dog inherits from Animal - therefore, it also has a Live()-method
public class Dog : Animal {
   // Additionally, we define a Bark()-Method
   public void Bark() {
      Console.WriteLine("Woof!");
   }
}
```

How does this behave with Polymorphism?

```cs
// You can assign a class to it's base class
Animal dog2 = new Dog();
// outputs: "I'm a living animal."
dog2.Live();

// but now, it can not bark anymore
// as we only know for sure, that it's an Animal
dog2.Bark();
```

### Virtualization


If both `Animal` and `Dog` define the same method, it comes to weird behavior, though:

```cs
public class Animal {
  public void MakeSound() {
    Console.WriteLine("<AnimalSound>");
  }
}

public class Dog : Animal {
  // warning: the keyword "new" is required on MakeSound 
  // because it hides inherited method Animal.MakeSound()
  public void MakeSound() {
    Console.WriteLine("Woof!");
  }
}
```

```cs
static void Main(string[] args) {
  Animal animal = new Animal();
  // output: "<AnimalSound>"
  Dog dog = new Dog();
  // output: "Woof!"
  dog.MakeSound();
  // valid: a dog is an animal
  Animal.animalDog = dog;
  // output: "<AnimalSound>"
  animalDog.MakeSound();
}
```

- To avoid this weird behavior, you can mark Properties or Methods as `virtual`
- `virtual` methods can be overridden by derived classes using the `override` keyword
- You can call the `base`-Method from within an `override` by using the `base`-Keyword


```cs
public class Animal {
  public virtual void MakeSound() {
    Console.WriteLine("<AnimalSound>");
  }
}

public class Dog : Animal {
  public override void MakeSound() {
    Console.WriteLine("Woof!");
  }
}

public class Cat : Animal {
  public override void MakeSound() {
    base.MakeSound();
    Console.WriteLine("Meow!");
  }
}
```

```cs
static void Main(string[] args) {
  Animal[] animals = {new Animal(), new Dog(), new Cat()};
  // Output: 
  // Animal: "<AnimalSound>"
  // Dog: "Woof!"
  // Cat: "<AnimalSound>" & "Meow!"
  foreach ( Animal animal in animals) {
    animal.MakeSound();
  }
}
```

- How to work with `virtual` and `override` and `base`?
- Mark methods as virtual, if you expect inheriting classes to override them
- Virtual methods and properties have to be public or protected (if not, then the deriving class cannot see or use them)
- You should actually **ALWAYS** call the `base` Method as well
- But there is very rare occasions, where you do not want to
- _(But that is a HACK and might break at some point)

---

## 11. Abstraction

- Sometimes, you want to provide a common base class
- But that base class does not serve any purpose on its own
- You can mark the class as abstract
- This disables instantiation of the class itself using the new keyword
- Even, if there is a constructor

```cs
// if you define a class to be abstract
public abstract class Animal {
  string name;
  public Animal(string name) {
    this.name = name;
  }
  
  public virtual string FavoriteFood => "<AnimalFood>"
    
  public virtual void MakeSound() {
    Console.WriteLine("<AnimalSound>");
  }
}

// another class can still inherit from that class
public class Dog : Animal {
  public override string FavoriteFood => "Bones";
  public override void MakeSound() {
    Console.WriteLine("Woof!");
  }

  public Dog(string name) : base(name) {
  }
}
```
```cs
// ERROR: cannot create an instance of the abstract class "Animal"
Animal animal = new Animal("Ape");
// this is fine, dog is not an abstract class
Dog dog = new Dog("Bello");
// this is also fine, dog is not an abstract class
Animal animal2 = new D0g("Tina");
```

  
- **Abstract classes can now contain:**
  - Abstract Properties
  - Abstract Methods
  - (but no abstract fields)
- If the inheriting class is not abstract, it HAS TO implement abstract members
- You implement abstract members by overriding them
- If the inheriting class is abstract, it MAY implement abstract members
- That‘s a good way of ensuring, that every Animal implements a MakeSound-Method of their own
And that there is no „non-sense“ MakeSound-Method in the base-class "<AnimalSound>"
     
```cs
// if you define a class to be abstract, then...
public abstract class Animal {
  // you can also define properties to be abstract
  public abstract string FavoriteFood { get; }
  // and also methods can be abstract
  public abstract void MakeSound();
  // (fields can not be abstract, though)
  public abstract string name;
}
// a class inheriting an abstract class...
public class Dog : Animal {
  // needs to override all abstract properties
  public override string FavoriteFood => "Bones";
  public void GuardHouse() { }
  // and all abstract methods
  public override void MakeSound() {
    Console.WriteLine("Woof!");
  }
public class Cat : Animal {
  public override string FavoriteFood => "Mice";
  public void HuntMouse() { }
  public override void MakeSound() {
    Console.WriteLine("Meow!");
  }
}
// ERROR: abstract inherited Members "FavoriteFood" and "MakeSound" not implemented
public class Mouse : Animal { }
  ```

---

## 12. Composition

### Reminder: Inheritance

An abstract base class
```cs
public abstract class Animal { }
```

Example of inheritance: the Dog inherits from Animal

```cs
public class Dog : Animal { }
```


### What is Composition?

Composition is when one class references another class as a Member.

Here is a `Weapon` class which will later contain all the logic for attacking etc.:

```cs
public class Weapon {
   public bool IsBroken { get; }
}
```

Here, the class `Player` uses Composition in order to contain and use a `Weapon`:

```cs
public class Player {

   public Weapon Weapon { get; private set; }

   public void EquipWeapon(Weapon weapon) {
      if (!weapon.IsBroken)
         this.Weapon = weapon;
   }

   public void Attack() {
    if(this.weapon == null) {
        Console.WriteLine("No weapon!");
    } else {
        Console.WriteLine("Attack");
    }
}
```

### Definition

Composition is when Classes contain classes as Fields or Properties.
- Inheritance = **is** a
- Composition = **has** a
- A mouse **is** an animal
- A player **has** a weapon

### Why Composition?

#### Inheritance is not appropriate

Sometimes, Inheritance sounds weird:
- A _Weapon_ **IS NOT** a _Button_, just because it is displayed like one
  - Therefore, it should not inherit from _Button_, but instead **have** one


We want to have a weapon visible as a button.

A class for a button in the UI:

```cs
public class Button { }
```

Weird way: a weapon inheriting ("Being") a button

```cs
public class Weapon : Button{}
```

Better way: a weapon HAS a button

```cs
public class Weapon {
   public Button Button { get; }
}
```

#### Inheritance has limitations

Classes can only inherit from one class at a time
- A Bird **is** an Animal (AI), A Plane **is** a Vehicle (Can be entered)
- Both can Fly, though, how to share the code, if not through inheritance?
- They both **have** Wings, for example.

```cs
public class Vehicle{}
public class Animal{}
public class FlyingObject{}
```

A plane cannot inherit from ("BE") two classes:

```cs
public class Plane : Vehicle, FlyingObject{ }
```

But if we had a class for Wings:

```cs
public class Wing{}
```

Then `Plane` could use `Wings` in order to fly:

```cs
public class Plane2 : Vehicle {
   public Wing[] Wings { get; }
}
```

Advantage: A `Bird` could also use `Wings` to fly:

```cs
public class Bird : Animal{
   public Wing[] Wings { get; }
}
```

Even though these classes don't inherit from each other and they don't have a common parent class.

### Composition OVER Inheritance

***General guide:*** "Composition over Inheritance"
- Modern Engines have mastered this with Entity-Component-Frameworks
- Entities (GameObject) **HAVE** Components, that makes them very modular!

---
