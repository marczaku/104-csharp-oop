# Slides 4 - Abstraction

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

## 13. Class Casting
Class Casting describes the process in Polymorphism of changing the Shape of a class to either its base or its parent:

```cs
public class Animal { 
   public void Breathe() { } 
}
```
```cs
public class Dog : Animal { 
   public void Bark() { } 
}
```
```cs
public class Cat : Animal { 
   public void Meow() { } 
}
```
```cs
public class Teapot { }
```

### Down-Casting

It is very easy to Down-Cast.\
Down-Casting describes the process of casting a class to one of its base-classes.\
If you have a Dog:
```cs
Dog dog = new Dog();
dog.Breathe();
dog.Bark();
```

Then you can implicitly Down-Cast him to an Animal, without requiring any explicit code for casting:
```cs
Animal animal = dog;
animal.Breathe();
```

### Up-Casting

Up-Casting however, the Process of casting a base class to one of its child classes, brings a few more problems.\
If you need to Up-Cast from an `Animal` to a `Dog`, you need to specify it:
```cs
Animal animal = new Dog();
Dog dog = (Dog)animal;
dog.Bark();
```

Why is that? Well, because you need to be quite sure, that this Animal actually is a Dog!\
Every Dog is an Animal. But not every Animal is a Dog.\
In fact, if you try to cast a Cat to a Dog, bad things will happen...

```cs
Animal animal = new Cat();
Dog dog = (Dog)animal;
dog.Bark();
```

Result: `Unhandled exception. System.NullReferenceException: Object reference not set to an instance of an object.`

This means, when casting, you should always make sure, that the casting was successful:

```cs
Animal animal = new Cat();
Dog dog = (Dog)animal;
if(dog != null) {
   dog.Bark();
}
```

### Keywords for Casting

Since Casting and Type-Checking is awfully ugly, C# has introduced two new keywords.\
There is `is` for Type-Checking:

```cs
Animal animal = new Cat();
if(animal is Dog) {
   Console.WriteLine("I'll be damned, it's a dog!");
}
```

And `as` for Type-Casting:

```cs
Animal animal = new Cat();
Dog dog = animal as Dog;
if(dog != null) {
   dog.Bark();
}
```

They're quite nice in combination:

```cs
Animal animal = new Cat();
if(animal is Dog) {
   (animal as Dog).Bark();
}
```

But even better is the newest addition to the Cast-Club.\
This Syntax can do the Type-Check and Type-Cast in one go:

```cs
Animal animal = new Cat();
if(animal is Dog dog) {
   dog.Bark();
}
```

### Illegal Casting

By the way, all of these methods of Up-Casting do not allow you to even try to do impossible Up-Casts.\
You will not even be able to Compile / Run the following Code on your Machine:

```cs
Animal animal = new Animal();
Teapot teapot = animal as Teapot;
```

Error: Cannot convert `Animal` to `Teapot` via built-in conversion.

---

## 14. Interfaces

Interfaces are used in C# to make sure that we can communicate with classes whose exact implementation we don't know, yet.\
By defining an interface that we want to interact with and then allowing any class that implements that interface to interact with us.

This interface here:
```cs
public interface IConsumable {
   void Consume(Hero hero);
}
```

For example only defines that whatever class implements `IConsumable`, must have a `Consume` Method.\
Therefore, without knowing, what class might implement `IConsumable` or, what `Consume` will actually do, we can interact with that interface:

```cs
public class Hero {
   public int Health {get;set;}
   public int Mana {get;set;}
   public void Use(IConsumable consumable) {
      consumable.Consume(this);
   }
}
```

We were actually able to fully implement the `Hero` class without a single `IConsumable` existing, yet.

What will happen will depend on the class implementing that consumable interface:
```cs
public class HealthPotion : IConsumable {
   public void Consume(Hero hero) {
      hero.Health += 50;
   }
}
```

Or maybe:

```cs
public class ManaPotion : IConsumable {
   public void Consume(Hero hero) {
      hero.Mana += 50;
   }
}
```

### Interface vs Abstract Class

What is the difference to an abstract base class, though?
```cs
public abstract class Consumable {
   public abstract void Consume(Hero hero);
}
```

Well not, much, actually. This is, how it would be used:
```cs
public class Hero {
   public void Use(Consumable consumable) {
      consumable.Consume(this);
   }
}
```

And this is, how it would be implemented:
```cs
public class HealthPotion : Consumable {
   public override void Consume(Hero hero) {
      hero.Health += 50;
   }
}
```

In fact, the only difference is, that we need to use the `override` keyword.

### Why Interfaces?

Then why use interfaces? They have one big advantage:

```cs
public class LuckyCharm : IEquippable, IConsumable {
   public void Equip(Hero hero) {
      hero.Luck += 5;
   }
   
   public void Consume(Hero hero) {
      hero.Gold += 10;
   }
}
```

In other words: While Classes cannot inherit from multiple classes, they can implement multiple interfaces!

### Defining an Interface

So, let's compare it again. A class that has only abstract members...

```cs
public abstract class Animal {
   public abstract string FavoriteFood { get; }
   public abstract string Name { get; set; }
   public abstract void MakeSound();
   public abstract void Feed(string food);
}
```

... can also be defined as an `interface`
- interface Members are always `public`
  - unless explicitly implemented, more on that later.
- Common Scheme for naming interfaces: `IName`

```cs
public interface IAnimal {
   string FavoriteFood { get; }
   string Name { get; set; }
   void MakeSound();
   void Feed8string food);
}
```

### Implementing an Interface

Now, other classes can `implement` this interface:
- We do not say, `Dog` inherits from `IAnimal`, but `Dog` implements `IAnimal`
  - even though, they both use the `:` symbol!
- The class implementing the interface has to:
  - Implement ALL Properties defined in the interface
  - Implement ALL Methods defined in the interface

```cs
public class Dog : IAnimal {
   public string FavoriteFood => "Bones";
   public string Name { get; set; }
   public void MakeSound() {
      Console.WriteLine("Woof!");
   }
   public void Feed(string food) {
      Console.WriteLine("Dog eating " + food);
   }
}
```

### Implementing multiple Interfaces

If your class inherits multiple interfaces, you need to separate them by comma `,`:

```cs
public class Dog : IFeedable, IAdoptable {
   // ...
}
```

If your class inherits from a class and also implements an interface, the class needs to come first:

```cs
public abstract class Animal { }
public interface IFeedable {
  void Feed(string food);
}

public interface IAdoptable {
   void Adopt();
}

public interface ICarriable {
   void Carry();
}

public class Dog : Animal, IFeedable, IAdoptable {
   public void Feed(string food) { }
   public void Adopt() { }
}
```

In above sample, a Dog can be fed and adopted.\
In the example below, we can see that a Cat can be fed and adopted, too, but also carried:

```cs
public class Cat : Animal, IFeedable, IAdoptable, ICarriable {
   public void Feed(string food) { }
   public void Adopt() { }
   public void Carry() { }
}
```

And the poor Lion can neither be carried nor adopted (it's a really big cat), but you can feed it:
```cs
public class Lion : Animal, IFeedable {
   public void Feed(string food) { }
}
```

### Using Interfaces

So, what's the advantage of these interfaces, really?\
Well, imagine, we have a small Zoo, full of animals:

```cs
Animal[] animals = {
   new Lion(), new Dog(), new Cat(), 
   new Cat(), new Dog(), new Lion(), new Cat(),
};
```

Now, how can we Feed all the animals that can be fed?\
We could check, what Animal each of them is, and then feed them, if possible:

```cs
for(int i = 0; i < animals.Length; i++) {
   IAnimal animal = animals[i];
   
   if(animal is Cat cat) {
      cat.Feed();
   }
   
   if(animal is Dog dog) {
      dog.Feed();
   }
   
   // ...
}
```

But you can see, that this can get quite annoying, actually. Imagine that for 150 different animals in the Zoo...\
But if each animal, which can be fed, implements the same `IFeedable` interface, then we can do this:

```cs
for(int i = 0; i < animals.Length; i++) {
   IAnimal animal = animals[i];
   
   if(animal is IFeedable feedable) {
      feedable.Feed("SomeFood");
   }
}
```

The same, we can cast for other interfaces like `IAdoptable`, when someone comes to adopt an animal from the zoo:
```cs
for(int i = 0; i < animals.Length; i++) {
   IAnimal animal = animals[i];
   
   if (animal is IAdoptable adoptable) {
      adoptable.Adopt();
      break;
   }
}
```

And for `ICarriable` in case someone tries to lift our animals:
```cs
for(int i = 0; i < animals.Length; i++) {
   IAnimal animal = animals[i];
   
   if ( animal is ICarriable carriable ) {
      carriable.Carry();
   }
}
```
