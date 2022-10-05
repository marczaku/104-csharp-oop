# Slides 3 - Inheritance

Inheritance allow us to share code between classes by putting it into a common base class. It allows for very useful code principles as polymorphism, which ensure that we can write code for all classes that share a common ancestor.

## 9. Inheritance 

Inheritance in C# allows us, to reuse code more efficiently by putting shared behaviour in base-classes and inheriting from them.\
Imagine for example, a class for `Animal`, which contains all logic for Animals, like needing to drink and eat, being able to move around, age, ...\
Then, there's a class for `Pet`, which is an `Animal` that can do all `Animal`-Things, but also can be tamed by the Player.\
Instead of writing all code from scratch again, we can have the `Pet` inherit from `Animal` and thereby have access to all `Animal`-functionality.\
Now, there might be different `Pet`: A `Parrot`, which can be tamed and Fly, and a `Dog`, which can be tamed and Sniff for enemies and Hidden Treasures.\
They may again inherit from `Pet`, which makes them `Animal` and `Pet`, too:

```cs
public class Animal { }
public class Pet : Animal { }
public class Parrot : Pet { }
public class Dog : Pet { }
```

- Through Inheritance, it is possible to have a new class Inherit Members from an existing class
- The Derived class (child / base)
  - Is the class that is inheriting
- The Base class (parent / base)
  - Is the class that is inherited FROM
**Syntax:**\ 
```cs
public class DerivedClass : BaseClass 
{
   // derived-class-body
}
```

- The derived class contains both its own Members (fields, methods, properties)
- As well as its‘ parent‘s Members (fields, methods, properties)
- You say, that `Dog` inherits from `Animal`
- Or that `Dog` IS AN `Animal`

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

Now let's see the `Animal`-class in Action:
```cs
Animal animal = new Animal();
// outputs: "I'm a living animal."
animal.Live();

// invalid, not every Animal can Bark
animal.Bark();
```

And the `Dog`:
```cs
Dog dog = new Dog();

// a Dog can Bark!
dog.bark();

// possible, a Dog is an Animal
dog.Live();
```

- Inheriting classes need to provide a compatible constructor
- Without, their base class can not be constructed
- If their base class does have a parameterless constructor, the derived class doesn‘t require a constructor

Syntax:
```cs
public DerivedClass(list-of-parameters) : base(list-of-base-constructor-arguments) {

}
```

```cs
public class LivingThing { }
public class Animal : LivingThing {
  string name;
  
  // this constructor does not need to call the base-constructor
  // it has a constructor without parameters
  public Animal(string name) {
    this.name = name;
  }
}

// ERROR: base class Animal does not contain parameterless constructor
public class Cat : Animal { }

public class Dog : Animal { 
  bool isChipped;
  
  // inheriting classes have to have a compatible constructor
  public Dog(string name) : base(name) { }
  
  // you can pass a constant into the base constructor
  public Dog() : base("Waldo") { }
  
  // but they may also have bigger constructors
  public Dog(string name, book isChipped) : base(name) {
    this.isChipped = isChipped;
  }
}

public class Pug : Dog {

   // they do not have to provide all constructor implementations
   // but at least 1!
   public Pug(string name) : base(name);
  }
}
```

- If you want to prevent another class from inheriting from your class…
- Use the sealed keyword
- It will prevent Inheritance

```cs
public sealed class Animal {
  public void Live() {
    Console.WriteLine("I'm a living animal.");
  }
}

// ERROR: Cannot inherit from sealed class Animal
public class Dog : Animal {
  public void Bark() {
    Console.WriteLine("Woof!");
  }
}
```

- By the way:
  - In C#, Classes can only inherit from one class at a time
  
  
```cs
// a class for swim and underwater breathing
public class AnimalInTheSea { }

// a class for mammal breeding logic
public class Mammal { }

// the dolphin cannot inherit from both classes at the same time :(
public class Dolphin : AnimalInTheSea, Mammal { } 
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


If both `Animal` and `Dog` define the same method, it comes to weird behaviour, though:

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

- To avoid this weird behaviour, you can mark Properties or Methods as `virtual`
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


- Another solution to inheritance?
- Composition!
- Classes can have classes as Fields or Properties!
- Inheritance = **is** a
- Composition = **has** a
- A mouse **is** an animal
- A player **has** a weapon
     
```cs
// an abstract base class
public abstract class Animal { }
// example of inheritance: 
// the Dog inherits from Animal
public class Dog : Animal { }
// a weapon class
public class Weapon {
   public bool IsBroken { get; }
}
// a Player class
public class Player {
   //example of composition:
   // the player "has a" weapon
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

- **Why Composition?**
- Sometimes, Inheritance sounds weird:
- A _Weapon_ **IS NOT** a _Button_, just because it is displayed like one
- Therefore, it should not inherit from _Button_, but instead **have** one
- Classes can only inherit from one class at a time
- A Bird **is** an Animal (AI), A Plane **is** a Vehicle (Can be entered)
- Both can Fly, though, how to share the code, if not through inheritance?
- They both **have** Wings, for example. 
- ***General guide:*** "Composition over Inheritance"
- Modern Engines have mastered this with Entity-Component-Frameworks
- Entities (GameObject) **HAVE** Components, that makes them very modular!
     
```cs
// a class for a button in the UI
public class Button { }
// we want to have a weapon visible as a button
// Weird way: a weapon inheriting ("Being") a button
public class Weapon1 : Button{}
// better way: a weapon HAS a button
public class Weapon2 {
   public Button Button { get; }
}

public class Vehicle{}
public class Animal{}
public class FlyingObject{}
// a plane cannot inherit frin ("BE") two classes:
public class Plane1 : Vehicle, FlyingObject{ }
public class Wing{}
// but it could HAVE wings for flying
public class Plane2 : Vehicle {
   public Wing[] Wings { get; }
}
// a bird could HAVE wings as well:
public class Bird : Animal{
   public Wing[] Wings { get; }
}
```

---
