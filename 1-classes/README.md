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

## 4 - Classes

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

## 5 - Class Members:

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

## 6 - Static Class Members:
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

## 7 - Constructor:
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

## 8 - Finalizer:
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

## 9 - Access Modifiers:
[Read the Slides on Access Modifiers](../slides/003.5-classes.md#9-access-modifiers)
### Goal
- Add Health and MaxHealth to our Unit.
- Make sure, that Health can never be less than 0 or more than MaxHealth.
### Instructions
- Continue working on the Project `RPG`
- Add two new fields to the `Unit` Class. Both fields should NOT be accessible from outside the class!
  - `maxHealth` of type `int`
  - `health` of type `int`
- Require `maxHealth` as an argument in the constructor
  - This phrasing describes two steps:
  - Adding the parameter to the constructor's parameter list
  - Assigning the passed value to the class's field in the constructor body
- In the Constructor, set `health` to the same value as `maxHealth`, too
- Modify `ReportStatus`, to show not only the Id and Name, but also Health and MaxHealth:
  - `Unit #27: Zombie - 127/200 Health`
  - `27` is the `id`
  - `Zombie` is the `name`
  - `127` is the `health`
  - `200` is the `maxHealth`
- Create a public Method named `SetHealth` to the `Unit`-class.
  - The method takes one argument: `newHealth` of type `int`
  - It then sets the Unit's `health` to the value of `newHealth`
  - It also ensures, that the new `health` is not smaller than `0` or greater than `maxHealth`
  - It then calls `ReportStatus` to print the new Status to the Console
- Test the functionality by:
  - Creating a Unit with `name` `Leet` and `maxHealth` 1337
  - Asking the User 3 times, to what value he wants to change the Unit's health
  - Assign the given value each time to the Unit's `SetHealth`-Method:

### Sample
```
[...](previous Output)
Output:Unit #3: Leet - 1337/1337 Health
Output:What do you want Leet's Health to be?
Input:1234
Output:Unit #3: Leet - 1234/1337 Health
Output:What do you want Leet's Health to be?
Input:-100
Output:Unit #3: Leet - 0/1337 Health
Output:What do you want Leet's Health to be?
Input:2000
Output:Unit #3: Leet - 1337/1337 Health
```

## 11 - Properties:
[Read the Slides on Properties](../slides/003.5-classes.md#11-properties)
### Goal
- Improve our Code's Readability a bit.
- Change the Game to have a Game Loop that runs as long as the Spawned Unit is alive.
### Instructions
- Continue working on the Project `RPG`
- Replace the Method `SetHealth` with a public Property named `Health` of type `int`
  - The same logic that happened in `SetHealth` before, should now happen in that propperty's `set`
  - Add a `get` to the Property, which returns the current value of the `health`-field
- Change the Game-Loop:
  - Don't ask the user 3 times for Leet's new Health
  - But instead ask him for as long as Leet's Health is greater than zero

### Sample
```
Output:Unit #3: Leet - 1337/1337 Health
Output:What do you want Leet's Health to be?
Input:1234
Output:Unit #3: Leet - 1234/1337 Health
Output:What do you want Leet's Health to be?
Input:2000
Output:Unit #3: Leet - 1337/1337 Health
Output:What do you want Leet's Health to be?
Input:-100
Output:Unit #3: Leet - 0/1337 Health
```

## 11.1 - Properties:
### Goal
- Right now, you can change our Unit's `name` at any time.
- Try it: Try adding: `Unit test = new Unit("Abc", 100); test.name = "Def";`
- This is not behaviour that we want. Let's change that.
### Instructions
- Continue working on the Project `RPG`
- Change the field `name` to be a Property named `Name` which only has a `get`, but no `set`
  - You can still assign the value in the constructor, but nowhere else anymore

### Sample
Adding this code should not compile anymore:
```cs
Console.WriteLine("Temporary, please remove from final Game!");
Unit test = new Unit("Abc", 100); 
test.Name = "Def";
```
But this one should compile:
```cs
Console.WriteLine("Temporary, please remove from final Game!");
Unit test = new Unit("Abc", 100); 
Console.WriteLine(test.Name);
```

```
Output:Abc
```

## 11.2 - Properties:
### Goal
- Clean Up our Code a bit more, to improve Readability.
- When we are checking in the Game Loop, whether the Unit's health is still > 0
- Then, what are we actually checking?
- Whether the Unit is alive or dead
- Let's have our code express this in a nice way!
### Instructions
- Continue working on the Project `RPG`
- Add a property named `IsAlive` of type `bool` to the `Unit` Class
  - It should have a `get` that returns `true`, if `health` is greater than zero
  - And `false` in any other case
- In the Game Loop, use this property instead of reading the Health!
- You can Remove the `Health` Property's `get` now!

### Sample
Adding this code should not compile anymore:
```cs
Console.WriteLine("Temporary, please remove from final Game!");
Unit test = new Unit("Abc", 100); 
Console.WriteLine(test.Health);
```
But this one should compile:
```cs
Console.WriteLine("Temporary, please remove from final Game!");
Unit test = new Unit("Abc", 100); 
test.Health = 500;
```

## 11.3 - Properties:
### Goal
- Prepare our Game for Enemy Attacks by changing the Game 
  - From setting the Health directly from 100 to 80
  - To instead say, that we want to deal 20 damage
### Instructions
- Continue working on the Project `RPG`
- Make the `set` on `Health` private, so no other class can adjust the value directly anymore
- Introduce a `public` method named `Damage`
  - It returns nothing
  - It takes a parameter named `value` of type `int`
  - It then subtracts `value` from our current `Health` and assigns it to `Health`
- Change the Game Loop, so it does
  - Not ask anymore, what we want Leet's Health to be
  - But instead asks, how much damage we want to deal
  - And then calls the `Damage`-Method with that value

### Sample
```
Output:Unit #3: Leet - 1337/1337 Health
Output:How much damage do you want to deal to Leet?
Input:12
Output:Unit #3: Leet - 1325/1337 Health
Output:How much damage do you want to deal to Leet?
Input:-200
Output:Unit #3: Leet - 1337/1337 Health
Output:How much damage do you want to deal to Leet?
Input:2000
Output:Unit #3: Leet - 0/1337 Health
```

Continue with [003.5.2 Console Classes 2](003.5.2-console-classes-2.md)
