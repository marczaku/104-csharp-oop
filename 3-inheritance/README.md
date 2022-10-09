# Exercises 3 - Inheritance

Inheritance allow us to share code between classes by putting it into a common base class. It allows for very useful code principles as polymorphism, which ensure that we can write code for all classes that share a common ancestor.

## Some Pre-Work

### Goal
- Prepare our Game for Enemy Attacks by changing the Game 
  - From setting the Health directly from 100 to 80
  - To instead say, that we want to deal 20 damage
### Instructions
- Continue working on the Project `RPG`
- Make the `set` on `Health` private, so no other class can adjust the value directly anymore
  - NOTE: Since `Health` does not have a `get` anymore, we actually CAN (and NEED TO) make the whole `Health`-Property `private` now.
  - Having a `public` Property with only a `private` `set` and no `get` would make no sense, or?
- Introduce a `public` method named `Damage`
  - It returns nothing
  - It takes a parameter named `value` of type `int`
  - It then subtracts `value` from our current `Health` and assigns it to `Health`
- Change the Game Loop, so it does
  - Not ask anymore, what we want Leet's `Health` to be
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

## 9 - Inheritance:
[Read the Slides on Inheritance](../slides/003.5-classes-2.md#2-inheritance)
### Goal
- Introduce a new Unit: The Necromancer
  - His name is `"Necromancer"`
  - His `maxHealth` is 200
  - He has a special ability: The first time he dies, he resurrects and his `Health` is set back to 50% of his `maxHealth` again!
### Instructions
- Continue working on the Project `RPG`
- Introduce a new class, called `Necromancer` that inherits from `Unit`
  - Add a parameter-less constructor that calls the base-constructor with the proper values
  - Introduce a `bool`-field named `hasResurrected`, so the can keep track on whether the `Necromancer` has died already.
  - Make the `Damage`-Method of the `Unit`-class `virtual`
  - `override` the `Damage`-Method in the `Necromancer` class
    - Invoke the `base`-Method, to make sure, that the regular damage-dealing still works
    - After that, check, whether the `Necromancer`'s `Health` is zero or less and that he has NOT resurrected, yet
    - If so: flag `hasResurrected` to be` true`, and set his Health to 50% of `maxHealth` again.
- Update the Game Loop, so a `Necromancer` is spawned instead of `Leet`

### Sample
```
Output:Unit #3: Necromancer - 200/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:199
Output:Unit #3: Necromancer - 1/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:2
Output:Unit #3: Necromancer - 0/200 Health
Output:Unit #3: Necromancer - 100/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:100
Output:Unit #3: Necromancer - 0/200 Health
```

## 9.1 - Inheritance:
### Goal
- Clean Up our Code by moving the `Resurrect`-Logic to its own Method.
### Instructions
- Continue working on the Project `RPG`
- Take the code that ressurects the `Necromancer` (set the health to 50% and change the flag)
  - And move it to its own `private` Method named `Resurrect`
  - Call this Method from within the `Necromancer`'s `Damage`-Method instead of the two lines of code that currently do so

## 9.2 - Inheritance:
### Goal
- Clean Up our Code by renaming the `Damage`-Method
### Instructions
- Continue working on the Project `RPG`
- Rename the `Damage`-Method to `TakeDamage`.
- Methods should always describe an Action (something that can be done)
- And `Damage` can either be a Verb (to damage) or a Noun (the damage)
- `TakeDamage` more specifically describes, what the method does

## 9.3 - Inheritance:
### Goal
- Clean Up our Code by using another Property instead of checking for the `Health` directly in `NecroMancers`'s `TakeDamage`-Method.
### Instructions
- Continue working on the Project `RPG`
- Introduce a new Property `IsDead` of type `bool` to the `Unit`
  - Have it return `true`, if the `Unit` is not `IsAlive` and `false`, if the unit `IsAlive`

## 10 - PolyMorphism:
[Read the Slides on Polymorphism](../slides/003.5-classes-2.md#3-polymorphism)
### Goal
- Make random Units spawn 3 times in a row.
### Instructions
- Continue working on the Project `RPG`
- Update the Game Loop, so randomly, either a `Unit` or a `Necromancer` is spawned.
- After the `Unit` or `Necromancer` dies, a new one is spawned.
- Until a total of three monsters have been killed.
- Tip: This should be much easier, if you replace your code, whereever it says:
```cs
Necromancer necromancer = new Necromancer();
```
with:
```cs
Unit unit = SpawnNewUnit();
```

And then have a method like this:
```cs
static Unit SpawnNewUnit(){
   // get random number between 0 and 1
   if(number == 0){
      return new Necromancer();
   }
   if(number == 1){
      return new...
   }
}
```

### Sample
```
Output:Unit #3: Necromancer - 200/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:199
Output:Unit #3: Necromancer - 1/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:2
Output:Unit #3: Necromancer - 0/200 Health
Output:Unit #3: Necromancer - 100/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:100
Output:Unit #3: Necromancer - 0/200 Health
Output:Unit #4: Unit - 250/250 Health
Output:How much damage do you want to deal to Unit?
Input:250
Output:Unit #4: Unit - 0/250 Health
Output:Unit #5: Unit - 250/250 Health
Output:How much damage do you want to deal to Unit?
Input:250
Output:Unit #5: Unit - 0/250 Health
Output:Game Over.
```

## 10.1 - PolyMorphism:
### Goal
- Make the game more expressive by adding additional Messages to the Console.
### Instructions
- Continue working on the Project `RPG`
- Write `Unit #3: Necromancer has spawned!` when a new Unit is spawned.
- Write `Unit #3: Necromancer has come back from the dead!` when a Necromancer has resurrected.
- Write `Unit #3: Necromancer has died!` when a new Unit is killed.

### Sample
```
Output:Unit #3: Necromancer has spawned!
Output:Unit #3: Necromancer - 200/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:199
Output:Unit #3: Necromancer - 1/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:2
Output:Unit #3: Necromancer - 0/200 Health
Output:Unit #3: Necromancer has died!
Output:Unit #3: Necromancer Has come back from the dead!
Output:Unit #3: Necromancer - 100/200 Health
Output:How much damage do you want to deal to Necromancer?
Input:100
Output:Unit #3: Necromancer - 0/200 Health
Output:Unit #3: Necromancer has died!
Output:Unit #4: Unit has spawned!
Output:Unit #4: Unit - 250/250 Health
Output:How much damage do you want to deal to Unit?
Input:250
Output:Unit #4: Unit - 0/250 Health
Output:Unit #4: Unit has died!
Output:Unit #5: Unit has spawned!
Output:Unit #5: Unit - 250/250 Health
Output:How much damage do you want to deal to Unit?
Input:250
Output:Unit #5: Unit - 0/250 Health
Output:Unit #5: Unit has died!
Output:Game Over.
```

## 11 - Abstraction:
[Read the Slides on Abstraction](../slides/003.5-classes-2.md#4-abstraction)
### Goal
- Make `Unit` to be a base-class, which we cannot instantiate by itself anymore.
### Instructions
- Continue working on the Project `RPG`
- Make the `Unit`-class `abstract` by using the `abstract`-keyword before the `class`-keyword
- Adjust the `SpawnNewUnit`-Method, so it can not spawn `Unit` anymore, only `Necromancer`

## 11.1 - Abstraction:
### Goal
- Implement more enemies: `Skeleton`, `Hedgehog`, `Bomb`
### Instructions
- Continue working on the Project `RPG`
- The `Skeleton` inherits from `Unit`, has 250 Health, the name `"Skeleton"`
- The `Hedgehog` inherits from `Unit`, has 200 Health, the name `Hedgehog` and this Special ability:
  - When hit, he goes into Defense Mode for 2 Rounds
    - Print `Unit #3: Hedgehog went into Defense Mode!`
  - After 2 rounds, he stops being in Defense Mode
    - Print `Unit #3: Hedgehog stopped being into Defense Mode!`
  - While in Defense Mode, the `Hedgehog` does not take any Damage from the Player
    - Print `Unit #3: Hedgehog was hit while in Defense Mode!`
- The `Bomb` inherits from `Unit`, has 500 Health, the name `Bomb` and this Special Ability:
  - It explodes after 5 Rounds
  - When exploding, the `Bomb`'s `Health` is set to 0
    - Print `Unit #3: Bomb has exploded!`
- Add these three new monsters to your `SpawnNewUnit`-Method
  - extend the Random Number to give results between 0 and 2
  - And map each of these numbers to a `Unit` that is then created

## 11.2 - Abstraction:
### Goal
- Implement the `Hero` as a class, so the Player can play (and die) himself, too!
### Instructions
- Continue working on the Project `RPG`
- The `Hero` inherits from `Unit`, has 1000 Health and the name `Hero`
- Create a `Hero` when the Game Starts, before the first Monster is spawned.
- The `Hero` takes 89 Damage each round.
- When the `Hero` `IsDead`, the Game is Over.
  - Print `Game Over. You Lose.`
- If the three monsters were killed before the `Hero` `IsDead`
  - Print `Game Over. You Win.`

## 11.3 - Abstraction:
### Goal
- Implement real Attacks!
### Instructions
- Continue working on the Project `RPG`
First, let's make sure that every unit has a `Power`-Property, so we know, how much damage they can deal:
- Add a read-only (only `get;`) Property `Power` of type `int` to the `Unit`-class.
- Require a new parameter in the `Unit`'s constructor: `power` of type `int`
  - Assign the `power`-argument's value to the `Unit`s `Power`-Property
Now, let's make sure, that this `power`-argument is provided with all `Unit`s:
- The `Hero` has 66 `power`
- The `Skeleton` has 46 `power`
- The `Necromancer` has 32 `power`
- The `Bomb` has 0 `power`
- The `Hedgehog` has 27 `power`
Now, let's add an `Attack`-Method to the `Unit`, which allows a `Unit` to Attack any other `Unit`
- Add a new Method with no return type to the `Unit` class named `Attack`
  - `Attack` takes one parameter named `target` of type `Unit` - this will be the `Unit` that we want to attack.
  - Within the `Attack`-Method, we should call `TakeDamage` on the `target`-`Unit` and pass our own `Power` as an argument.
Now, let's first Remove the Part in our Game Loop, where we ask the Player, how much Damage he wants to deal and replace it with the following Code-Sample:
```cs
Console.WriteLine("The fight continues... (Press any key.)");
Console.ReadKey();
hero.Attack(monster);
monster.Attack(hero);
```

## 12 - Composition:
[Read the Slides on Composition](../slides/003.5-classes-2.md#5-composition)
### Goal
- Remove the `Power`-Property from the `Unit`-Class and replace it with a `Weapon`-Property.
### Instructions
- Continue working on the Project `RPG`
- Introduce a new `abstract` class `Weapon`
  - Weapon has a read-only Propery named `Power` of type `int`
  - And a Constructor that requires a Parameter named `power` of type `int`, which is then assigned to the `Power`-Property
- Introduce the following Weapons:
  - `TrainingWeapon` with 66 `power`
  - `BoneSword` with 46 `power`
  - `CursedStaff` with 32 `power`
  - `Unarmed` with 0 `power`
  - `Spike` with 27 `power`
- Change the `Unit`-constructor to not require `power` of type `int` anymore
- But instead require a `weapon` of type `Weapon`
- Also, remove the `Power`-Property of type `int` from the `Unit`
- And add a `Weapon`-Property of type `Weapon` to the `Unit` instead
- Assign the `weapon` passed to the `Unit`-constructor to the `Unit`'s `Weapon`-Property
- Assign the following `Weapon`s in the different classes' constructors:
  - `Hero`: `new TrainingWeapon()`
  - `Skeleton`: `new BoneSword()`
  - `Necromancer`: `new CursedStaff()`
  - `Bomb`: `new Unarmed()`
  - `Hedgehog`: `new Spike()`
- Modify the `Attack`-Method, so it does not pass `Power` into the `TakeDamage`-Method, but `Weapon.Power` instead.
- Add a new Message to Attacks that looks like this:
`Unit #3: Hero uses TrainingWeapon to attack Unit #4: Necromancer for 66 Damage.`

## 12.1 - Composition (BONUS):
### Goal
- Implement the `Bomb`'s Explosion Feature fully: When it explodes after 5 rounds, it deals 500 Damage to the Player.
### Instructions
- There is no instructions here, I am looking forward to see, what solutions you came up with :)
