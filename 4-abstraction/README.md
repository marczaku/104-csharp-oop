# Exercises 4 - Abstraction

## 11 - Abstraction:
### Goal
- Make `Unit` to be a base-class, which we cannot instantiate by itself anymore.
### Instructions
- Continue working on the Project `RPG`
- Make the `Unit`-class `abstract` by using the `abstract`-keyword before the `class`-keyword
- Adjust the `SpawnNewUnit`-Method, so it can not spawn `Unit` anymore, only `Necromancer`

Need Help? [Here's The Slides!](slides/README.md#11-abstraction)

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

Need Help? [Here's The Slides!](slides/README.md#11-abstraction)

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

Need Help? [Here's The Slides!](slides/README.md#11-abstraction)

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

Need Help? [Here's The Slides!](slides/README.md#11-abstraction)

## 12 - Composition:
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

Need Help? [Here's The Slides!](slides/README.md#12-composition)

## 12.1 - Composition (BONUS):
### Goal
- Implement the `Bomb`'s Explosion Feature fully: When it explodes after 5 rounds, it deals 500 Damage to the Player.
### Instructions
- There is no instructions here, I am looking forward to see, what solutions you came up with :)

Need Help? [Here's The Slides!](slides/README.md#12-composition)


## 13 - Class Casting:
### Goal
- Have our `Hero` deal Bonus Damage against certain Units.
### Instructions
- Continue working on the Project `RPG`
- Make the `Attack`-Method on the `Unit`-class `virtual`, if you haven't done so already.
  - This allows us to `overload` this method for some Units, giving them special `Attack` abilities.
- `overload` the `Attack`-Method on the `Hero`-class.
  - First, call the `base`-`Attack`-Method.
  - Then, check, if the `target`-`Unit` `is` a `Skeleton`
    - If it is, then Deal 10 Damage extra.
    - Print `"The Hero deals 10 extra Damage against the Skeleton's weak Bones!"`

Need Help? [Here's The Slides!](slides/README.md#13-class-casting)

## 13.1 - Class Casting:
### Goal
- Introduce a new `Unit` called `Ghost` which scares our `Hero` to his bones 👻
### Instructions
- Continue working on the Project `RPG`
- Add a new `class` inheriting from `Unit` named `Ghost`
  - The `Ghost` has 200 Health
  - The `Ghost` uses `Haunt`, a `Weapon` with 10 Damage.
- Edit the `Hero`'s `Attack`-Method.
  - Check first, whether the `target`-`Unit` `is` a `Ghost`
    - If it is, then do a random-roll:
    - With a 55% Chance, the `Hero` is scared and will not attack this round.
    - Write `"The Hero is too scared to attack!"`
    - Make sure that the `Hero` does not get scared by other `Unit`s!

Need Help? [Here's The Slides!](slides/README.md#13-class-casting)

## 13.2 - Class Casting:
### Goal
- We will refactor our `TakeDamage`-Method to allow our `Hedgehog`'s `Spikes` to deal damage while he's in defense Mode.
### Instructions
- Change the `Unit`'s Method `TakeDamage` to take a second parameter named `attacker` of type `Unit`.
- Now, change the Code all over the place, where the `TageDamage`-Method is invoked and pass `this` as a second argument.
  - Like this: `unit.TakeDamage(30, this);`
- Now, in the `Hedgehog`'s `TakeDamage` Method, if he is in Defense-Mode, let the Hedgehog deal 5 Damage through its Spikes.
  - Print: `"The Hedgehog's Spikes are up and they hurt!"`
  - Call `TakeDamage` on the `attacker`-`Unit` and deal 5 Damage.
  - What Danger exists right now?
    - ||think about what might happen, if two Hedgehogs in Defense Mode fight each other.||

Need Help? [Here's The Slides!](slides/README.md#13-class-casting)

## 14 - Interfaces:

### Goal
- Create a System in which `Weapon`s can be equipped and unequipped.
- `IWeapon` is an `interface` for classes that can be Equipped
- `IHand` is an `interface` for classes that can equip items

### Instructions

#### Define IWeapon

- Create an `interface` named `IWeapon`.
  - it has a `Property` named `EquippedTo` of type `IHand` with only a getter: `{get;}` 
  - it has a `Method` named `EquipTo` with one parameter of type `IHand` named `Hand`.
  - it has a `Method` named `UnEquip` with no parameters.

#### Define IHand

- Create an `interface` named `IHand`.
  - it has a `Property` named `Weapon` of type `IWeapon` with a getter and setter: `{get; set;}`

#### Implement IHand

- Have the `Unit` implement `IHand`
  - You just need to add the `Weapon`-Property with getter and setter

#### Implement IWeapon

- Have the `Weapon` implement `IWeapon`
  - You need to add the `EquippedTo`-Property with a getter and private setter
  - Implement the `UnEquip`-Method:
    - First, we remove ourselves from the `Hand` that we are equipped to:
    - On our own `EquippedTo`-Property, set the `Weapon`-Property to `null`
    - Then, we set our own `Hand` to `null`, since we are not equipped anymore:
    - Set our own `EquippedTo`-Property to `null`
  - Implement to `EquipTo`-Method:
    - First, if there already is a `Weapon` equipped to the `Hand`, we need to `UnEquip` it:
    - On the `Hand`-Parameter, we need to see, if `Weapon` is not `null`. And if it is not `null`, we need to call `UnEquip` on that `IWeapon`.
    - Then, we need to attach ourselves to the `Hand`:
    - On the `Hand`-Parameter, set the `Weapon`-Property to `this`
    - Then, we need to save the `Hand` in our own Property:
    - Assign the `Hand`-Parameter to our own `EquippedTo`-Property

#### Using Weapons

- Now, let's remove the `Weapon`-Property from the `Unit`
- And in the `Unit`'s Constructor, call `EquipTo` on the `weapon` passed as a constructor argument, and pass `this` as an argument to the Method.

Need Help? [Here's The Slides!](slides/README.md#14-interfaces)

## Done?
Return to the [Overview](../../../#5-game-on)
