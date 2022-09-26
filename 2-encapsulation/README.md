# Exercises 2 - Encapsulation

## 7 - Access Modifiers:
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

## 8 - Properties:
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

## 8.1 - Properties:
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

## 8.2 - Properties:
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

## 8.3 - Properties:
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
