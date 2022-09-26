# Exercises 1 - Classes

## Goal
We'll learn the basics of Object-Oriented Programming and how to define and instantiate a class and how to customize it with Fields and Methods.

## 1 - Object-Oriented Programming:

### Goal
```
Output:Give me a name.
Input:Mike
Output:Give me a name.
Input:Eva
Output:Give me a name.
Input:Max
Output:Mike,Eva,Max
```

### Instructions
- Create a Console Project named `P1ObjectOrientedProgramming`
- Small exercise for warm-up:
  - Create an array of three `strings`, named `names`
  - Ask the user for three names. 
    - Use a `for`-loop, to do so.
    - Put each name into one slot of the Array.
  - Now, print all three names to the console.
    - In one line.
    - Separated by comma.

Need Help? [Here's The Slides!](slides/README.md#1-object-oriented-programming)

## 2 - Classes

### Goal
```
Output:Person
Output:Animal
Output:Car
```
### Instructions
- Create a Console Project named `P2Classes`
- Create 3 Classes:
  - `Person`
  - `Animal`
  - `Car`
- For each of these classes:
  - Create one instance (object) of the class and assign each to a variable
  - Use `Console.WriteLine` and pass that instance (object) into the method


Need Help? [Here's The Slides!](slides/README.md#4-classes--objects)

## 3 - Class Members:

### Goal
```
[...](Output from Previous Exercises)
Output:Give me a name.
Input:Mike
Output:Give me a name.
Input:Eva
Output:Give me a name.
Input:Max
Output:Hello, my name is Mike
Output:Hello, my name is Eva
Output:Hello, my name is Max
```

### Instructions
- Continue Working on `P2Classes`
- Extend the class `Person`:
  - Give it a field of type `string` with the name `name`
  - Give it a method named `IntroduceYourself()`
    - The method should print `"Hello, my name is "` and the `name` of the Person to the console
- Create an Array of three `Person`, name it `persons`
- Ask the user for three names. 
  - Use a for-loop, to do so.
  - Put each name into one slot of the Person-Array's `name`
- Now, call `IntroduceYourself()` on each person of the Array

Need Help? [Here's The Slides!](slides/README.md#5-class-members)

## 4 - Static Class Members:
[Read the Slides on Static Class Members](../slides/003.5-classes.md#6-static-class-members)
### Instructions
- Create a Console Project named `P4StaticClassMembers`
- Create a `static` class named `SuperMath`
- In that claas, create a `static` method named `Lerp`
  - Tt takes three arguments:
    - `from` of type `float`
    - `to` of type `float`
    - `t` of type `float`
  - It returns one value of type `float`:
    - It returns the result of the formula `from + (to - from)*t`
- This is a very useful method to make things interpolate between two values
- Either over time, or in a certain amount
- You will use it a lot during game development!
- Check out the Samples to see it in action!

### Sample
```
Output:Lerp from 0 to 100 with t(0):0
Output:Lerp from 0 to 100 with t(0.1):10
Output:Lerp from 0 to 100 with t(0.3):30.000002
Output:Lerp from 0 to 100 with t(0.7):70
Output:Lerp from 0 to 100 with t(1.0):100
Output:Lerp from 0 to 100 with t(1.3):130
Output:Lerp from 100 to 200 with t(0):100
Output:Lerp from 100 to 200 with t(0.1):110
Output:Lerp from 100 to 200 with t(0.3):130
Output:Lerp from 100 to 200 with t(0.7):170
Output:Lerp from 100 to 200 with t(1.0):200
Output:Lerp from 100 to 200 with t(1.3):230
Output:Lerp from 200 to -100 with t(0):200
Output:Lerp from 200 to -100 with t(0.1):170
Output:Lerp from 200 to -100 with t(0.3):110
Output:Lerp from 200 to -100 with t(0.7):-10
Output:Lerp from 200 to -100 with t(1.0):-100
Output:Lerp from 200 to -100 with t(1.3):-190
```

## 5 - Constructor:
[Read the Slides on Constructor](../slides/003.5-classes.md#7-constructor)
### Goal
- To create a Unit class that has an ID and a Name.
- To create a Method that can give us details about a Unit's Status.
- Later, this Unit will serve as our Combat Target in our RPG.
### Instructions
- Create a Console Project named `RPG`
- Create a class named `Unit`
- Add a field to the Class of type `string` named `name`
- Add a Contructor to the Class
  - The Contructor requires an argument of type `string` named `name`
  - Assign the value of `name` to the field `name` (use `this.` to reference the field)  
- Add a field of type `int` named `id` to the Class
- Add a static field of type `int` named `nextId` to the Class
  - static fields are shared acros all Class instances
- In the Constructor, assign the vaalue of `nextId` to the field `id` of the class
- After that, increase `nextId` by 1
  - this ensures, that the next unit gets a higher id than the previous one
- Add a method named `ReportStatus` to the Class
  - It reports the unit's status to the User by printing:
  - `Unit #2: Zombie`
    - where 2 needs to be replaced by the unit's `id`
    - and Zombie needs to be replaced by the unit's `name`
- Create three instances of a `Unit` with names of your choice
- Call `ReportStatus` at the end of the Constructor!
  - Make sure, that this Method call stays at the bottom of the constructor at all times!
  - I think it's best to put a comment there right away: `// Make sure, that this is the last line of the constructor:`

### Sample
```
Output:Unit #0: Zombie
Output:Unit #1: Skeleton
Output:Unit #2: Necromancer
```

## 6 - Finalizer:
[Read the Slides on Finalizer](../slides/003.5-classes.md#8-finalizer)
### Goal
- Add a method to our Game that lets us know, if a Unit dies.
### Instructions
- Continue working on the Project `RPG`
- Add a Finalizer to the Unit Class
- Make it say print Unit #2: Zombie got finalized." to the Console.
- You can quickly test it using following code sample:
```cs
for(int i = 0; i < 2; i++){
  Unit unit = new Unit("LivingHand");
  GC.Collect();
}
```
- But please remove it again afterwards, we're trying to make a "real" game here.
- And this feature will pop back up again later.

### Sample
Note: actually, no new output appears, but while you put the sample code above in your code, this should pop up:
```
[...](previous Output)
Output:Unit #3: LivingHand
Output:Unit #4: LivingHand
Output:Unit #3: LivingHand got finalized.
```