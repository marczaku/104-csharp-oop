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

Need Help? [Here's The Slides!](slides/README.md#9-inheritance)

## 9.1 - Inheritance:
### Goal
- Clean Up our Code by moving the `Resurrect`-Logic to its own Method.

### Instructions
- Continue working on the Project `RPG`
- Take the code that ressurects the `Necromancer` (set the health to 50% and change the flag)
  - And move it to its own `private` Method named `Resurrect`
  - Call this Method from within the `Necromancer`'s `Damage`-Method instead of the two lines of code that currently do so


Need Help? [Here's The Slides!](slides/README.md#9-inheritance)

## 9.2 - Inheritance:
### Goal
- Clean Up our Code by renaming the `Damage`-Method

### Instructions
- Continue working on the Project `RPG`
- Rename the `Damage`-Method to `TakeDamage`.
- Methods should always describe an Action (something that can be done)
- And `Damage` can either be a Verb (to damage) or a Noun (the damage)
- `TakeDamage` more specifically describes, what the method does

Need Help? [Here's The Slides!](slides/README.md#9-inheritance)

## 9.3 - Inheritance:

### Goal
- Clean Up our Code by using another Property instead of checking for the `Health` directly in `NecroMancers`'s `TakeDamage`-Method.

### Instructions
- Continue working on the Project `RPG`
- Introduce a new Property `IsDead` of type `bool` to the `Unit`
  - Have it return `true`, if the `Unit` is not `IsAlive` and `false`, if the unit `IsAlive`

Need Help? [Here's The Slides!](slides/README.md#9-inheritance)

## 10 - PolyMorphism:

### Goal
- Make random Units spawn 3 times in a row.

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

Need Help? [Here's The Slides!](slides/README.md#10-polymorphism)

## 10.1 - PolyMorphism:

### Goal
- Make the game more expressive by adding additional Messages to the Console.

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

### Instructions
- Continue working on the Project `RPG`
- Write `Unit #3: Necromancer has spawned!` when a new Unit is spawned.
- Write `Unit #3: Necromancer has come back from the dead!` when a Necromancer has resurrected.
- Write `Unit #3: Necromancer has died!` when a new Unit is killed.

Need Help? [Here's The Slides!](slides/README.md#10-polymorphism)

## Done?
Return to the [Overview](../../../#4-abstraction)
