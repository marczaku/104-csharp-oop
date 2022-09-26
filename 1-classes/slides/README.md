# Slides 1 - Classes

## Goal
We'll learn the basics of Object-Oriented Programming and how to define and instantiate a class and how to customize it with Fields and Methods.

## 1. Object Oriented Programming

<img src="https://user-images.githubusercontent.com/7360266/136045813-68ca1a5b-287f-44a1-a328-55449dfd62de.png" width="500" height="250">
---

### 1.1. Structured Programming
- Programs are divided into functions and data
- Functions
  - Require Data as Input
  - Return Data as Output
- Data
  - Only stores information
- Separation of Data and Functions

```cs
Player player;
player.health = 10;
player.gold = 17;

void BuyPotion(ref Potion potion)
{
    if (player.gold < potion.costs) return;

    player.gold -= potion.costs;
    potion.ownedAmount++;
}

void SellPotion(ref Potion potion)
{
    if (potion.ownedAmount < 1) return;

    potion.ownedAmount--;
    player.gold += potion.costs / 2;
}

void UsePotion(ref Potion potion)
{
    if (potion.ownedAmount < 1) return;

    potion.ownedAmount--;
    player.health += potion.healAmount;
}

Potion smallPotion;
smallPotion.costs = 10;
smallPotion.healAmount = 20;
smallPotion.ownedAmount = 0;

BuyPotion(ref smallPotion);
BuyPotion(ref smallPotion);
SellPotion(ref smallPotion);
BuyPotion(ref smallPotion);
UsePotion(ref smallPotion);

Potion freePotion;
freePotion.costs = 0;
freePotion.healAmount = 1;
freePotion.ownedAmount = 0;

BuyPotion(ref smallPotion);
UsePotion(ref smallPotion);

struct Potion
{
    public int costs;
    public int healAmount;
    public int ownedAmount;
}

struct Player
{
    public int health;
    public int gold;
}
```



---

### 1.2. Object-Oriented

- Programs are divided into objects
- Objects contain functions and data

```cs

Player player = new Player();
player.gold = 15;
player.health = 10;

Potion smallPotion = new Potion();
smallPotion.costs = 10;
smallPotion.healAmount = 20;
smallPotion.ownedAmount = 0;



player.Buy(smallPotion);
player.Buy(smallPotion);
player.Sell(smallPotion);
player.Buy(smallPotion);
player.Use(smallPotion);

Potion freePotion = new Potion();
freePotion.costs = 0;
freePotion.healAmount = 1;
freePotion.ownedAmount = 0;

player.Buy(freePotion);
player.Use(freePotion);


class Potion{
    public int costs;
    public int healAmount;
    public int ownedAmount;
}

class Player {

    public int health;
    public int gold;

    public void Buy(Potion potion){
        if(gold < potion.costs) return;
        gold -= potion.costs;
        potion.ownedAmount++;
    }

    public void Sell(Potion potion){
        if(potion.ownedAmount < 1) return;
        potion.ownedAmount--;
        gold += potion.costs/2;
    }

    public void Use(Potion potion){
        if(potion.ownedAmount < 1) return;
        potion.ownedAmount--;
        health += potion.healAmount;
    }
}

```


---

## 2. Classes & Objects

Note: You will see us use the Keyword `public` all over the place in the next few slides.

### Definition

Defining a class means defining a Template for Objects. e.g. a Template for a House can be used to later represent multiple houses of a neighborhood. Or a template of a monster can later be used to spawn multiple monsters in the world.

You can define a class using this syntax:
```cs
public class ClassName {
  /* class body */
}
```

Note: The `public` Keyword is important in order to allow the use of this class outside the file in which it was created.

```cs
public class Animal {
}
```

### Type Use

Classes are Types. So they can be used as such, for example to define a variable:

```cs
Animal animal;
```

Or as a function Parameter:

```cs
void Pet(Animal target){
  Console.WriteLine($"The player pets {target}.");
}
```

Or a function Return Type:

```cs
Animal Adopt(){
  return new Animal();
}
```

### Null

```cs
Animal animal = null;
```

The default value for classes is `null`. Classes are Reference Types, that's why their default value is `null`. More on that later.

### Instantiation

Objects are instances of classes. They define the actual instance. e.g. a House-class defines, what houses generally look like. A House-Object (or House-Instance) is one instance of a house that you can interact with. e.g. your own house.

- Create instances using the `new` keyword
- Then store the returned value in a variable
- Or pass it on to another method

```cs
Animal pet = new Animal();
```

Here's some sample code to demonstrate the difference:

```cs
Animal animal = null;
Console.WriteLine(animal);
animal = new Animal();
Console.WriteLine(animal);
```

Output:
```

Animal
```

So, `Console.WriteLine` will print nothing when a variable is `null` (no instance) and the class name, when an instance of the class, an object, has been assigned to it.

### Destruction

When you no longer need an object instance, it's enough to "forget about it" i.e. lose the reference to that instance.

There's multiple ways of this happening:

#### Assigning a new instance to a variable:

```cs
Animal animal = new Animal(); //Object #1
animal = new Animal(); // Object #2 (End of Object #1)
```

#### Assigning null to a variable:

```cs
Animal animal = new Animal(); // Object #1
animal = null; // (End of Object #1)
```

#### Leaving a variable's scope

```cs
{
  Animal animal = new Animal(); // Object #1
} // (End of Object #1)
```

#### new, Heap and the Garbage Collector

We will learn about the details of these technical terms later, but here's an overview of what happens in the background:

```cs
Animal animal = null;
```

An `Animal` variable is defined and the value #0000 is assigned to it. This tells the Runtime Environment, that there is no Animal.

```cs
animal = new Animal();
```

Here, an instance of the Type `Animal` gets created in some magic wonderland named the Heap. And not the actual `Animal` gets assigned to the variable, but its reference. You can think of that as its ID or Personnummer. Let's say #AF61.

```cs
animal = null;
```

Now, when you assign null (#0000) to the variable again, there is no variable that holds the original reference (ID #AF61) anymore. But the actual `Animal` instance still exists in the Heap!

```cs
// A few moments later...
```

At some point later, it's not deterministic when exactly (depends on your system's memory and your GC settings), the Garbage Collector will look at all object instances in the Heap and find out, whether any variable still references this object. Should it find out, that the `Animal` instance with ID #AF61 is no longer referenced...

```cs
~Animal()
```

It invokes the so-called Finalizer (more on that later) and destroys the object instance, which in reality just means that it marks the address range in the RAM as Available and no longer in use.

P.S: Deleting in Computer Science usually only means "let's forget that there's important information there, it'll get overridden at some later point. Which is, why Safe-Deleting software exists, which continuously writes some randomly scrambled data over the Memory in question, as good Hackers (and even Script-Kiddos) could else recover deleted files easily.

---

## 3. Class Members

Members define the Properties and Methods of a class.
- Properties define, what a class IS
- Methods define, what it DOES

### Fields

Fields are the the traditional way of defining the Properties of a class. Properties define, what a class IS. e.g.
- `Animal` is a class. it defines, what Properties Animals generally have.
- `string name` is a field. it defines a property that animals share: they have a name.
- `"Bello"` might be the value of the field on one class instance. Another instance of the class might be named `"Rex"`.

Notes:
- Fields are still the main way of defining Properties in Unity (due to the way its serialization works). 
- In modern C# Code, the use of Fields is more or less discouraged (it is recommended to use Properties instead whenever you can, we'll look into that later)

Syntax: 
```cs
public FieldType fieldName;
```

Note: This time, we need to write `public` to allow access to this field from outside the class. More on that later.

Looks just like a variable, right? It kind of is!\
Field are basically variables that belong to a class instance:

```cs
public class Animal {
  public string name;
}
```

We can use the field on instances of that class:

```cs
Animal dog = new Animal();
dog.name = "Bello"; // Assigning a value to the field
Console.WriteLine(dog.name); // Reading the value of the field
```

Output:
```
Bello
```

You can also have multiple instances of the class:

```cs
Animal myDog = new Animal();
Animal yourDog = new Animal();
myDog.name = "Bello";
yourDog.name = "Rex";
Console.WriteLine(myDog.name);
Console.WriteLine(yourDog.name);
```

Output:
```
Bello
Rex
```

Whenever you assign a new instance of a class to a variable, you will reset all of its fields:

```cs
Animal myDog = new Animal();
myDog.name = "Rex";
myDog = new Animal();
Console.WriteLine(myDog.name);
```

### Default Values

You might be surprised, but Fields do not need to be initialized. Their default values are `false`, `0` or `null` respectively (which are all represented under the hood as binary `0000 0000`):

```cs
Animal pet = new Animal();
Console.WriteLine(pet.likesTuna);
Console.WriteLine(pet.age);
Console.WriteLine(player.name);

public class Animal{
  public bool likesTuna;
  public int age;
  public string name;
}
```

Output:
```
False
0

```


### Methods

Member methods are functions that belong to class instances:

```cs
public class Animal {
  public void Pet() {
    Console.WriteLine($"The player pets the animal.");
  }
}
```

Note: Again, we're using the `public` Keyword. This time, to allow us to use this method from outside the class.

We can then invoke that Method on instances of the class:

```cs
static void Main(string[] args) {
  Animal dog = new Animal();
  dog.Pet();
}
```

Output:
```
The player pets the animal.
```

Note: Now, you probably have a better idea of what we were doing earlier, when we wrote:

```cs
Random random = new Random();
int roll = random.Next(6);
```

We can also combine member methods and fields:
- This allows us to access an instance's fields from within an instance's method:

```cs
public class Animal {
  public string name;
  public void Pet() {
    // here, we can access this animal's name-field:
    Console.WriteLine($"The player pets {name}.");
  }
}
```

```cs
Animal bello = new Animal();
bello.name = "Bello";
Animal rex = new Animal();
rex.name = "Rex";
bello.Pet();
rex.pet();
```

Output:
```
The player pets Bello.
The player pets Rex.
```

---

## 4. Static Class Members

- Static class members are part of the class-type
- And not part of the instances
- To access them, you use the class‘s type name instead of an instance
  - Static Field: `public static FieldType fieldName;`
  - Static Method: `public static ReturnType MethodName() {}`

```cs
public class Animal {
  public string name;
  // This is a static field
  public static int count;
  
  public void MakeSound() {
    Console.WriteLine(name + ":...");
  }
  
  // This is a static method
  static static void PrintCount() {
    Console.WriteLine("Animal-Count: " + count);
  }
}

static void Main(string[] args) {
  Animal adam = new Animal();
  adam.name = "Adam";
  // Static fields are accessed using the type name
  Animal.Count++;
  
  Animal eva = new Animal();
  eva.name = "Eva";
  Animal.Count++;
  
  // Static methods are accessed using the type name
  
  Animal.PrintCount(); // Output: "Animal-Count: 2"
  
  // Non-static mathods require an instance to be called
  eva.MakeSound(); // Output: "Eva:..."
}
```


- You can even mark a complete class as `static`
  - Only, if all Members are marked `static`, too!
  - This will force you to make all members `static`, too!


```cs
// This is a static class:
public static class Math{
  public static int Add(int a, int b) {
    return a + b;
  }
}
```

```cs
// ERROR: A static class cannot have instance members:
public static class Math{
  public int Add(int a, int b) {
    return a + b;
  }
}
```

It will disable using the `new` keyword to create instances of this class

```cs
public static void Main() {
  // ERROR: This is not possible:
  Math math = new Math();
}
```

Once more, to compare, for a class named `Tree`:

-| static | instance
 :--: | :--: | :--:
keyword | `static` | none (default)
define a field | `public static int count;` | `public int height;`
write to a field | `Tree.count = 5;` | `Tree tree = new Tree(); tree.height = 5;`
read a field | `int i = Tree.count;` | `Tree tree = new Tree(); int i = tree.height;`
define a method | `public static string PrintDefinition(){}` | `public void DoPhotosynthesis(){}`
use a method | `Tree.PrintDefinition();` | `Tree tree = new Tree(); tree.DoPhotosynthesis();`

Now, you probably wonder:\
When to use `static` and when not to?

The question you need to ask for Fields:
Is the Field, that you want to add, something you want every instance to own by itself? e.g.:
- a house's color (one Houe might be red, another one blue)
- a person's name (one person is called Max, another one Anna)
- a unit's health (one unit might have full health, the other one might be almost dead)
Then make it an intance field.

Is the Field something that is shared across all instances of a class? e.g.:
- a counter for how many enemies have been spawned
- a global buff that is applied to all units (like 10% extra damage for everyone)
Then make it a static field.

Do you want to easily access this information globally, because you need it only in one place, e.g.:
- a timer, how long the game still lasts
- the current score of the player
- the information, whether the game is paused
Then you can also make it a static field (at least for now, I will later strongly discourage you of using static fields)

The question you can ask for Methods:
Does the Method require any information from a class's instance? e.g.:
- The `TakeDamage`-Method needs to access a spawned unit's `health`
- The `Attack`-Method needs sto know, who is attacking, and how much damage the attacker can deal
- The `SetPosition`-Method needs to set a certain GameObject's position
Then you want to use an instance Method

Is the Method a very Generic Method, that works independent of an instance of a class? e.g.:
- Any `Math`-function; Mathematical functions don't need to know, who wants to add two numbers, the result will be the same
- A Method that can calculate a path between to locations in an RTS
Then make the method static. Usually, static functions are really used for Calculations and Formulas only


---

## 5. Constructor

Constructors describe, how a new class instance (object) can be created (constructed).\
They are needed in order to be able to instantiate a class using the `new` keyword.\
They offer us more control over:
- what information is needed to create a class instance
  - e.g. you are not allowed to create a Hero without providing a `name`
- what initial values a class should have
  - e.g. a player's initial health should be `100` and not `0`

```cs
public class Cat 
{

}
```
Constructors are called when a class is constructed through the `new` key-word\
There‘s an invisible Parameterless Default-Constructor for every class doing nothing\
Which means, that above class in reality looks like this:
```cs
public class Cat 
{
  // the invisible, parameter-less constructor looks like this:
  public Cat() 
  { 
  }
}
```
This constructor is called, as soon as you create a new instnce of the class with the `new` keyword\
Note, how we call the Constructor using `()` (a list of zero parameters), just like a parameter-less method:
```cs
Cat meows = new Cat();
```

We can actually change the class to proof, that this is happening:
```cs
public class Cat 
{
  // the invisible, parameter-less constructor looks like this:
  public Cat() 
  { 
     Console.WriteLine("And thus, a new Cat was born!");
  }
}

public class Program {
   static void Main() {
      Cat meows = new Cat();
   }
}
```

Output: `And thus, a new Cat was born!`

The syntax of a Constructor is:
```cs
public ClassName(parameter-list) {
   constructor-body
}
```

You can create your own constructor without parameters, to do some initialization:

```cs
public class Elephant {
  bool hasTrunk;

  // as soon as an elephant is created...
  publlic Elephant() {
    // set `hasTrunk` to true
    this.hasTrunk = true;
  }
}
```

You can also create a constructor with parameters, just like you can create a method with parameters:


```cs
public class Dog {
  string name;
  
  // require a parameter in order to construct a new Dog:
  public Dog(string name) {
    // assign the passed parameters to our own field
    // `this` helps us to access our own field `name` instead of the parameter with the same name:
    this.name = name;
  }
}
```

Now, we can create Dogs using the constructor with the correct parameter:
```cs
Dog woofs = new Dog("Woofs");
```

But the invisible default constructor has disappeared now, so we cannot use it anymore:
```cs
// Error: No parameter-less constructor exists!
Dog woofs = new Dog();
```

---

## 6. Finalizer

A Finalizer allows us to take control over what happens as soon as a class instance (object) is destroyed.\
Sometimes, we want to be notified, when something disappeared.\
Or we need to do some clean-up (e.g. remove some temporary files)



The syntax of a Finalizer is:
```cs
~ClassName() {
   finalizer-body
}
```

Let's have a look at the following class with a finalizer:

```cs
public class Animal {
  // Constructor
  public Animal() {
    Console.WriteLine("Animal created.");
  }
  
  // Finalizer
  ~Animal() {
    Console.WriteLine("Animal destroyed.");
  }
}
```

Now, let's create and Finalize a few animals:
```cs
static void Main(string[] args) {
  for ( var i = 0; i < 2; i++) {
    // This will call the Constructor
    var animal = new Animal(); // Output: Animal created.
    
    // At this point, the loop will start again
    // The old animal variable will then be out of scope
    // And therefore cleaned up by the garbage collector at some point
    // This will call the finalizer (whenever the GC decides to "wake up"
    // To speed this process up, since the garbage collector is slow and lazy:
    // We can call it manually:
    GC.Collect();
  }
}
```

Output:
```
Animal created.
Animal created.
Animal destroyed.
```

The output might confuse you. But it can be explained:
- First, an Animal is created.
- Then, we call the Garbage-Collector, but the animal still exists in the current scope.
- Then, the loop ends, the current scope ends, and the animal variable gets reset.
- Then, we create another Animal.
- Then, we call the Garbage-Collector again. This time, there is an old Animal ready to be cleaned up.
  - That's, why we now see, that one animal was destroyed.

To summarize the Finalizer:
- Opposite of a Constructor
- Defined through `~ClassName()` `{}` Method Signature
- Unreliable (You never know, when it‘s called)
- Dependant on when the Garbage Collector Collects the Object
- Better: IDisposable-Pattern

---