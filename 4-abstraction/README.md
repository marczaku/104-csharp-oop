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
  - This allows us to `override` this method for some Units, giving them special `Attack` abilities.
- `override` the `Attack`-Method on the `Hero`-class.
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
  - it has a `Property` named `Power` of type `int` with only a `get`-Accessor.
    - We will use it to keep track of a `Weapon`'s damage in the `Attack` method.
  - it has a `Property` named `EquippedTo` of type `IHand` with only a `get`-Accessor
    - We will use it to keep track of what `Hand` the `Weapon` is currently equipped to
  - it has a `Method` named `EquipTo` with one parameter of type `IHand` named `hand`
    - We will use it to equip a Weapon to a new Hand
  - it has a `Method` named `UnEquip` with no parameters
    - We will use it to unequip a weapon from whatever Hand it is currently equipped to.

#### Define IHand

- Create an `interface` named `IHand`.
  - it has a `Property` named `Weapon` of type `IWeapon` with a `get` and a `set` accessor
    - We will use it to keep track of what `Weapon` is currently equipped to this `Hand`

#### Implement IWeapon

Have the class `Weapon` implement `IWeapon`

##### Power

The `Weapon` class already has a `Power` Property. Nice!

##### EquippedTo

- You need to add the `EquippedTo`-Property with a getter and private setter
  - The private `set` Accessor will be used to change the `Hand` that the `Weapon` is currently equipped to.
    - We want it to be `private` so it can only be chanced through the `EquipTo` and `UnEquip` methods.
  - The public `get` Accessor exists, so other classes can also see, what `Hand` a Weapon is currently equipped to.

##### UnEquip

Implement the `UnEquip`-Method:
- First, we remove ourselves from the `Hand` that we are equipped to:
  - On `EquippedTo`-Property, assign `null` to the `Weapon` Property
- Then, we can stop tracking the `Hand` that we are equipped to:
  - Assign `null` to the `EquippedTo` Property

Question: What happens, if you try to unequip a weapon that is currently not equipped?

```cs
IWeapon banana = new Banana();
banana.UnEquip();
public class Banana : Weapon{
   public Banana() : base("Banana", 100){

   }
}
```

Question: How can you fix above issue?

##### Equip

Implement to `EquipTo`-Method:
- First, we let the `Hand` know, what `Weapon` it is holding:
  - On the `hand`-Argument, set the `Weapon`-Property to `this`
- Then, we need to keep track of the `Hand` that we are currently equipped to, so `UnEquip` knows later, what `Hand` to remove the `Weapon` from:
  - Assign the `hand`-Argument to the `Weapon`'s `EquippedTo`-Property

Question: What happens, if you try to equip the same weapon to two different Hands?

```cs
IHand left = new Hand();
IHand right = new Hand();
IWeapon banana = new Banana();

banana.EquipTo(left);
banana.EquipTo(right);

Console.WriteLine($"Left Hand carries {left.Weapon}.");
Console.WriteLine($"Right Hand carries {right.Weapon}.");
Console.WriteLine($"Banana is equipped to {banana.EquippedTo}.");

public class Hand : IHand{
  public IWeapon Weapon {get; set;}
}
public class Banana : Weapon{
   public Banana() : base("Banana", 100){

   }
}
```

Question: How can you fix above issue?

#### Implement IHand

The following approach is a very simple one, where every `Unit` can only hold one `Weapon` at a time:

- Have the `Unit` implement `IHand`
  - Now, let's remove the `Weapon`-Property of type `Weapon` from class `Unit`
  - Also remove the assignment in the `Unit`-Constructor
  - You just need to add the `Weapon`-Property of type `IWeapon` with getter and setter

You could also extend above example by having two hands:

```cs
public class Unit{
  IHand Left {get;}
  IHand Right {get;}

  public Unit(/*...*/){
    Left = new Hand();
    Right = new Hand();

    // ...
  }
}
public class Hand : IHand{
  // ...
}
```

#### Using Weapons

- Change the `Unit`'s Constructor to take a `IWeapon` Argument instead of `Weapon`.
- In the `Unit`'s Constructor, call `EquipTo` on the `weapon` argument
  - pass a reference to the `Unit` itself (`this`) as an argument to the Method.

Need Help? [Here's The Slides!](slides/README.md#14-interfaces)

## 14.1 Interfaces Part 2

### Goal

Now you might wonder, why go the extra mile of using interfaces?\
Well, because they're highly extensible.

But before we can prove that, let's clean up the Logic a bit more.

Right now, the `Unit` has the `Attack` logic in its `Attack`-Method:

```cs
public void Attack(Unit target){
  Console.WriteLine($"Unit #{id}: {name} uses {Weapon.Name} to attack unit #{target.id}: {target.Name} for {Weapon.Power} damage.");
  target.TakeDamage(Weapon.Power);
}
```

But it makes more sense to put that logic in the `IWeapon`-Interface:

```cs
public interface IWeapon{
  void Attack(Unit attacker, Unit target);
  // ...
}
```

This will allow us to make way more diverse weapons than we can right now. You'll see!

### Instructions

#### Change IWeapon interface

We will add a new Method to the `IWeapon` `interface` to make sure that every `Weapon` can Attack:

- Add the Method named `Attack` with two arguments to `IWeapon`:
  - `attacker` of type `Unit`
    - We use this to keep track of who is attacking.
  - `target` of type `Unit`
    - We use this to keep track of who is being attacked.

#### Change Weapon Class

Since the `IWeapon` interface changed and `Weapon` implements `IWeapon`, we need to change the `Weapon` class:

- Add the missing Method to the `Weapon` base class
- Move the logic from `Unit.Attack` to `Weapon.Attack`
  - The Method is now part of the `Weapon`, not the attacking `Unit` anymore. So we can't access the Unit's `id` and `name` fields anymore. We need to fix this. We will change the `Unit` class later. For now:
    - Change `id` to `attacker.Id`. We will add the Property to the `Unit` class.
    - Change `name` to `attacker.Name`. The Property already exists on `Unit` class.
  - The Method is now part of the `Weapon`, not the `Unit` holding the `Weapon`, so we need to fix:
    - Change `Weapon.Name` to `Name`. The Method is part of the Weapon. So it can access itself.
    - You can remove `this.`, but it might make the change more clear to you.
    - Change `Weapon.Power` to `this.Power`. Same as above. You can remove `this.`
  - We don't need to make changes to `target` because the `target` was passed to the `Unit`'s Attack Method and will then be passed on to the `Weapon`'s Attack Method.
- Invoke `Weapon.Attack` Method from within `Unit.Attack` Method
  - Pass `this` as `attacker` Argument, since the `Unit` that the `Attack` Method is invoked on is the one attacking.
  - Pass on the `target` Argument from `Unit.Attack` to `Weapon.Attack`

#### Change Unit Class

In above code, we required the `Unit`'s private field `int id`. But we can't access it. Let's make it accessible through a public Property containing only a `get` Accessor:

- Add a `public` Property of type `int` named `Id`.
  - In the `get` accessor, return the value of the private field named `id`.

Test your changes! Everything should still be working!

## 14.2 Interfaces Part 3

### Goal

This time, we're gonna do some awesome stuff!\
Not all equippable Weapons must be physical objects. Let's make a Mind Controller!

### Instructions (Mind Control)

- Add a new class named `MindControl`. It implements the interface `IWeapon`
  - It has 0 Power
    - so its `Power` Property should just always return `0`
  - It needs an `EquippedTo` Property
    - with a public `get`
    - and private `set`
  - `EquipTo` Method
    - we always assign `this` to `hand.Weapon`
  - `UnEquip` Method
    - here, we do nothing. `MindControl` can not be unequipped.
  - In `Attack` Method
    - First, we print some info to the console: `Console.WriteLine($"Unit #{attacker.Id}: {attacker.Name} uses {Name} on unit #{target.Id}");`
    - Then, we do something really cool: We just invoke the `Attack` Method on the `target`, passing `target` itself as an argument (Stop hitting tourself!)

- Add a new class named `BigBrain`
  - It inherits from class `Unit`
  - Add a `health` stat of your liking
  - Pass `new MindControl()` as a weapon to the base class.

Adjust your `SpawnNewUnit` Method to be able to randomly spawn a `BigBrain` as well as other units.
- Consider givin `BigBrain` a 100% Spawn Chance for now in order to more easily test, whether it's working.

## 14.3 Interfaces Part 4

### Goal

Another awesome class. We have the `IHand` interface for hands and `IWeapon` for equippable Weapon. But what if our `Weapon` has a `Hand` or even two?

### Instructions

- Add a new class named `RobotArm`
  - It implements the interface `IHand`
  - It has a public Property named `Weapon` of type `IWeapon` with `get` and `set` Accessor.

- Add a new class named `RobotArms`
  - It implements the interface `IWeapon`
    - We will implement `Power` later, since this Weapon's Power adds up! :)
    - It needs an `EquippedTo` Property with a public `get` and private `set`
    - It has a `EquipTo` method, which looks the same as `Weapon`'s `EquipTo` Method. Not great, but let's keep it simple for now.
    - It has a `UnEquip` method, which also looks the same as `Weapon`'s `UnEquip` Method.
    - We will implement `Attack` later, since this Weapon  will have multiple Weapon's! :)
  - It has two Properties of Type `IHand`
    - Read-Only Property named `Left` of type `IHand`
    - Read-Only Property named `Right` of type `IHand`
  - It has a Constructor that takes two `IWeapon` as arguments!
    - Constructor parameter `left` of Type `IWeapon`
    - Constructor parameter `right` of Type `IWeapon`
    - Assign `new RobotArm` to `Left` Property
    - Assign `new RobotArm` to `Right` Property
    - Invoke `EquipTo` on `IWeapon left` passing `IHand Left` as an argument
    - Invoke `EquipTo` on `IWeapon right` passing `IHand Right` as an argument
  - Now, let's implement the `Power` Property:
    - Add a `get` Accessor that returns the sum of `Left.Weapon.Power` and `Right.Weapon.Power`
  - And also the `Attack` Method:
    - First, we print some info to the console: `Console.WriteLine($"Unit #{attacker.Id}: {attacker.Name} uses {Name} on unit #{target.Id}");`
    - Now, we invoke the `Attack` Method on `Left.Weapon` and pass the `attacker` and `target` Arguments through.
    - And then, we invoke the `Attack` Method on `Right.Weapon` and pass the `attacker` and `target` Arguments through.

- Add a new class named `Tinker`
  - It inherits from class `Unit`
  - Add a `health` stat of your liking
  - Pass `new RobotArms()` as a weapon to the base class.
    - Pass two Weapons of your liking to the constructor. Play around with this.
    - Maybe, you can make a method that passes two random weapons to the constructor every time? :)

## 14.4 Interfaces Part 5

### Goal

More ideas:

- A Weapon that does not only deal damage to the `target`, but also heal the `attacker`
  - Its `Attack` Method is slightly different to a regular Weapon's `Attack` Method
- A Weapon, which is a shield, that can be carried additionally to a `Weapon` (in the same hand)
  - It implements both `IHand` and `IEquippable`
  - And the `IWeapon` passed to the constructor gets attached to itself
- A Weapon, which is really strong, but always needs one turn to charge
- A Weapon, which is really strong, but breaks after 3 usages
- A Weapon, whose damage doubles every time it attacks
- A Weapon, which deals random damage 
- A Weapon, which has a 50% chance of hitting the attacker instead of the target

## Done?
Return to the [Overview](../../../#5-game-on)
