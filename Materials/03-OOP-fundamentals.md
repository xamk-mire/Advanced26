
# Foundations of Object-Oriented Programming

_A Guided Introduction to Thinking in Objects_

Imagine you are designing a small digital world. In this world, there are people, cars, bank accounts, game characters, books, and animals. Each of these things has **data** (what they are like) and **behavior** (what they can do).

Object-Oriented Programming (OOP) is a way of organizing code so that it models the real world more naturally. Instead of writing long lists of instructions, we create **objects** that represent real or conceptual things, and we let those objects manage their own data and actions.

This approach makes programs easier to understand, extend, and maintain—especially as they grow larger.

Let’s walk step-by-step through the core building blocks:

- Classes
    
- Objects
    
- Methods
    
- Private Fields
    
- Encapsulation
    
- Properties
    
- Constructor

---

# 🧱 Classes: The Blueprints of Object-Oriented Design

Before there are objects, before there is encapsulation, before there are private fields and properties, there are **classes**.

A **class** is a definition. It is a blueprint, a plan, a template for creating objects.

A class answers the question:

> _What does it mean to be one of these?_

It defines:

- What data an object will have
    
- What behaviors an object will support
    
- What rules govern that kind of thing
    

A class is not a real thing in your program.  
It is a description of what real things _will be like_.

Just as an architect’s drawing is not a house, a class is not an object.  
It is the idea of a house.

---

## 1) Classes Describe Kinds, Not Individuals

Think in categories:

- A **Car**
    
- A **Student**
    
- A **Player**
    
- A **BankAccount**
    
- A **Book**
    

Each of these is a _kind_ of thing.

A class represents the kind.  
An object represents a specific individual.

```csharp
class Student
{
    public string Name;
    public int StudentId;
}
```

This does not create a student.  
It defines what _all students_ will have.

---

## 2) Classes Define Structure and Capability

A class defines both:

- **Structure** → what data exists
    
- **Capability** → what actions are possible
    

```csharp
class Door
{
    public bool IsOpen;

    public void Open()
    {
        IsOpen = true;
    }

    public void Close()
    {
        IsOpen = false;
    }
}
```

The class is saying:

> “Every Door has an `IsOpen` state, and every Door knows how to open and close itself.”

This is powerful: behavior lives with data.

---

## 3) Classes Are Contracts

A class is a contract with the rest of the program.

It promises:

- These members exist
    
- These operations are supported
    
- These names and types are stable
    

Other code can rely on this.

This means once a class is published and used:

- Many parts of the program may depend on it
    
- Changing it carelessly can break many things
    

This is why class design matters.

---

## 4) Classes Create a Shared Language in Code

Classes are not just technical — they are conceptual.

They become part of the _vocabulary_ of your system.

Instead of saying:

> “That struct with three ints and two strings…”

You say:

> “That Player.”

Instead of:

> “That decimal with rules…”

You say:

> “That BankAccount.”

Classes turn raw data into meaningful concepts.

---

## 5) Follow-along Example: A Simple Class Grows

### Version 1: Bare definition

```csharp
class Counter
{
    public int Value;
}
```

This defines what a Counter _has_.

---

### Version 2: Adding behavior

```csharp
class Counter
{
    public int Value;

    public void Increment()
    {
        Value++;
    }

    public void Reset()
    {
        Value = 0;
    }
}
```

Now the class defines what a Counter _does_.

The class is becoming more than a data bag.  
It is becoming an active concept.

---

## 6) Classes and Responsibility

Each class should have a clear responsibility.

A good class can usually be described in one sentence:

- “A Player represents a controllable character in the game.”
    
- “A BankAccount manages money and enforces financial rules.”
    
- “A Thermostat controls and reports temperature.”
    

If a class cannot be described simply, it may be doing too much.

This is a key design principle.

---

## 7) Classes Are Where Rules Begin

Even before encapsulation, classes are where rules belong conceptually.

For example:

- What makes a valid Player?
    
- What makes a valid BankAccount?
    
- What makes a valid Order?
    

The class is the natural home for these rules.

Later, with encapsulation, we enforce them more strongly.  
But the class is where they are _defined_.


---
# 🧍 Objects: Living Instances of a Class Blueprint

If a class is a blueprint, then an **object** is a real building.

If a class is a species, then an **object** is an individual organism.

A class describes what _could exist_.  
An object is what _does exist_.

An **object** is a concrete instance created from a class. It represents a specific, living entity inside your program — with its own data, its own identity, and its own place in memory.

Objects are where OOP becomes real.

---

## 1) From Definition to Existence

A class alone does nothing.  
Only when you create an object does your program gain something it can use.

```csharp
class Lamp
{
    public bool IsOn;
}
```

At this point, there is no lamp.

Now:

```csharp
Lamp deskLamp = new Lamp();
```

Now a lamp exists.

You have moved from _definition_ to _existence_.

---

## 2) Objects Have Identity, Not Just Data

Two objects can have the same data and still be different objects.

```csharp
Lamp lamp1 = new Lamp();
Lamp lamp2 = new Lamp();

lamp1.IsOn = true;
lamp2.IsOn = true;
```

Both are on.  
But they are **not the same lamp**.

They:

- Live at different memory locations
    
- Have separate identities
    
- Can change independently
    

This is a core OOP idea:  
Objects are more than just values.

---

## 3) Objects Carry Their Own State

An object’s **state** is the collection of values stored in its fields and properties at a given moment.

For example:

```csharp
class Player
{
    public string Name;
    public int Health;
}
```

```csharp
Player p1 = new Player();
p1.Name = "Alex";
p1.Health = 100;
```

The state of `p1` is:

- Name = "Alex"
    
- Health = 100
    

Another object:

```csharp
Player p2 = new Player();
p2.Name = "Jordan";
p2.Health = 50;
```

Same class.  
Different state.  
Different individual.

---

## 4) Objects Receive Messages (Method Calls)

In OOP, you do not just run functions.  
You tell **objects** to do things.

```csharp
player.TakeDamage(20);
```

This reads like a sentence:

> “Player, take damage.”

This is intentional.

Objects:

- Own their data
    
- Own their behavior
    
- Decide how to respond
    

This makes programs more natural to read and reason about.

---

## 5) Objects as Miniature Managers

Each object manages its own internal world.

For example:

```csharp
class Counter
{
    public int Value;

    public void Increment()
    {
        Value++;
    }
}
```

When you write:

```csharp
counter.Increment();
```

You are not manipulating `Value` directly.

You are asking the counter to manage itself.

This idea becomes critical once encapsulation is introduced.


---

## 6) Many Objects, One Class

One class can produce:

- 1 object
    
- 10 objects
    
- 10,000 objects
    

Each with its own independent state.

```csharp
List<Player> players = new List<Player>();

players.Add(new Player("Alex"));
players.Add(new Player("Jordan"));
players.Add(new Player("Sam"));
```

One definition.  
Many individuals.

This is how systems scale.

---

## 8) Objects and Collaboration

Objects rarely exist alone.

They work together.

```csharp
class Order
{
    public Customer Customer;
    public decimal Total;
}
```

Here, one object can _contain references to other objects_.

This forms object graphs — networks of cooperating instances.

Large systems are built from these collaborations.

---

## 9) Objects as Responsibility Holders

Each object should be responsible for something specific.

Good objects:

- Own a clear piece of the problem
    
- Protect their own state
    
- Provide meaningful behavior
    

Bad objects:

- Are just data bags
    
- Have no real behavior
    
- Are manipulated from the outside
    

Encapsulation will strengthen this later.

---

# ⚙️ Methods: Giving Objects the Power to Act

If objects are the _actors_ in your program, then **methods** are their _actions_.

A method is how an object does something.  
It is how an object expresses behavior.  
It is how an object responds when the program speaks to it.

Without methods, objects would be silent containers of data.  
With methods, objects become active participants in your system.

Methods are where logic lives.

---

## 1) Methods Turn Data into Behavior

A class without methods is like a machine with no buttons.

```csharp
class Counter
{
    public int Value;
}
```

This object has data, but no way to act.

Now:

```csharp
class Counter
{
    public int Value;

    public void Increment()
    {
        Value++;
    }

    public void Reset()
    {
        Value = 0;
    }
}
```

Now the counter can _do things_.

The object is no longer passive.  
It can change itself.

---

## 2) Methods Belong to Objects

In OOP, methods are not just free-floating functions.  
They belong to a class and are called on objects.

```csharp
counter.Increment();
```

This reads as:

> “Counter, increment yourself.”

This reinforces a key idea:

> Objects are responsible for their own behavior.

---

## 3) Methods Define What an Object Can Do

A method is part of the object’s public identity.

When you look at a class’s methods, you can often understand its purpose.

```csharp
class BankAccount
{
    public void Deposit(decimal amount) { ... }
    public void Withdraw(decimal amount) { ... }
    public decimal GetBalance() { ... }
}
```

Even without implementation, you understand what this object is for.

Methods communicate intent.

---

## 4) Parameters: Giving Methods Information

Methods often need information to do their work.

This information is passed in as **parameters**.

```csharp
public void TakeDamage(int amount)
{
    Health -= amount;
}
```

Here:

- `amount` is a parameter
    
- It represents information from the outside world
    
- The method uses it to update internal state
    

Parameters are how the outside world _negotiates_ with an object.

---

## 5) Return Values: Giving Information Back

Some methods return values.

```csharp
public int GetScore()
{
    return score;
}
```

Now:

```csharp
int currentScore = player.GetScore();
```

The object is speaking back to the program.

This is a two-way conversation:

- Parameters → information in
    
- Return value → information out
    

---

## 6) Methods Protect and Enforce Rules

Methods are often the _only_ way to change important state.

```csharp
public void Withdraw(decimal amount)
{
    if (amount <= 0)
        throw new ArgumentException();

    if (amount > balance)
        throw new InvalidOperationException();

    balance -= amount;
}
```

This method:

- Validates input
    
- Enforces business rules
    
- Protects the object
    

This is how methods become guardians, not just operations.

---

## 7) Private vs Public Methods

Not all methods are meant for the outside world.

### Public methods

Part of the object’s interface.

```csharp
public void Heal(int amount)
```

### Private methods

Internal helpers.

```csharp
private bool IsCritical()
{
    return health < 20;
}
```

Private methods:

- Break complex logic into readable pieces
    
- Hide internal algorithms
    
- Improve maintainability
    

They support encapsulation just like private fields.

---

## 8) Methods Express Responsibility

Well-designed methods answer:

> “What is this object responsible for doing?”

Bad design:

```csharp
player.Health -= 10;
```

Better design:

```csharp
player.TakeDamage(10);
```

Now:

- Rules can live in one place
    
- Side effects are controlled
    
- Meaning is clear
    

Methods turn raw math into meaningful actions.

---

## 9) Follow-along Example: A Living Object

```csharp
class Door
{
    private bool isOpen;

    public void Open()
    {
        isOpen = true;
    }

    public void Close()
    {
        isOpen = false;
    }

    public bool IsOpen()
    {
        return isOpen;
    }
}
```

Usage:

```csharp
door.Open();
if (door.IsOpen())
{
    Console.WriteLine("The door is open.");
}
```

This reads like a story:

> Open the door.  
> Ask the door if it is open.

The object manages itself.

---

## 10) Methods and Object Identity

Each object has its own copy of method logic, but operates on its own state.

```csharp
Door frontDoor = new Door();
Door backDoor = new Door();

frontDoor.Open();
backDoor.Close();
```

Same class.  
Same methods.  
Different objects.  
Different outcomes.

Methods always act on _this object_.

---



# 🔐 Private Fields: Hiding and Protecting an Object’s Inner State

In Object-Oriented Programming, not all data should be visible to the outside world.

Some information belongs strictly _inside_ an object. This internal data represents the object’s true state — the details that make the object what it is. To protect this state, OOP uses **private fields**.

A **private field** is a variable that:

- Belongs to a class
    
- Stores internal data
    
- Cannot be accessed directly from outside the class
    
- Is meant to be used only by the class’s own methods and properties
    

Private fields are the foundation of **encapsulation**.

They are how an object says:

> “This is my internal business. If you want to interact with me, use my public interface.”

---

## 1) Fields vs. Private Fields

A **field** is simply a variable declared inside a class.

```csharp
class Car
{
    public int Speed;
}
```

Here, `Speed` is a **public field**. Anyone can change it:

```csharp
car.Speed = -500;   // 🚨 unrealistic and dangerous
```

This breaks the idea that a car’s speed should make sense.

---

## 2) Making a Field Private

Now let’s make the field private:

```csharp
class Car
{
    private int speed;
}
```

Now this is **not allowed**:

```csharp
car.speed = 100;   // ❌ compile-time error
```

Only code _inside_ the `Car` class can see or change `speed`.

This creates a protective wall around the object’s internal state.

---

## 3) Private Fields as the Object’s “Memory”

Think of private fields as the object’s memory or internal organs.

They:

- Hold the real data
    
- Should not be touched directly by outsiders
    
- Are manipulated only through controlled operations
    

Example:

```csharp
class Counter
{
    private int count;

    public void Increment()
    {
        count++;
    }

    public int GetCount()
    {
        return count;
    }
}
```

Here:

- `count` is private storage
    
- `Increment()` is the safe way to change it
    
- `GetCount()` is the safe way to read it
    

The outside world cannot corrupt `count`.

---

## 4) Why Private Fields Matter (Real Problems They Prevent)

### Problem 1: Invalid states

```csharp
class HealthBar
{
    public int Health;
}
```

Now:

```csharp
healthBar.Health = -999;
```

Your game character is now in a broken state.

### Fixed with private fields:

```csharp
class HealthBar
{
    private int health;

    public int Health
    {
        get { return health; }
        set
        {
            if (value < 0) health = 0;
            else health = value;
        }
    }
}
```

The private field ensures the rules are always enforced.

---

## 5) Private Fields + Properties = The Standard Pattern

This is one of the most important patterns in OOP:

```csharp
class Person
{
    private string name;

    public string Name
    {
        get { return name; }
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Name required.");

            name = value;
        }
    }
}
```

Pattern:

- Private field → stores data
    
- Public property → controls access
    
- Validation → enforces rules
    

This is the **canonical encapsulation pattern**.

---

## 6) Private Fields and Class Invariants

A **class invariant** is a rule that must always be true for an object.

Examples:

- A bank balance must not be negative
    
- Health must be between 0 and max
    
- Age must be ≥ 0
    
- Temperature must be within safe limits
    

Private fields are how you enforce invariants.

```csharp
class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
        private set
        {
            if (value < 0)
                throw new InvalidOperationException("Balance cannot be negative.");

            balance = value;
        }
    }

    public void Withdraw(decimal amount)
    {
        Balance -= amount;   // invariant enforced here
    }
}
```

The invariant lives inside the class — nowhere else.

---

## 7) Why Not Just Trust the Programmer?

Because large systems have:

- Many developers
    
- Many call sites
    
- Many future changes
    

Private fields prevent accidental misuse.

They turn bugs into **compile-time errors** instead of silent logic errors.

This:

```csharp
account.balance = -1000000;
```

Is impossible.

The compiler becomes your ally.

---

## 8) Private Fields Enable Internal Refactoring

One powerful benefit of private fields:

You can change them without breaking external code.

Example: change internal representation

```csharp
// Version 1
private int speed;
```

Later:

```csharp
// Version 2
private double speedInMetersPerSecond;
```

As long as the public properties and methods stay the same, the outside world doesn’t care.

This is a huge advantage for long-term maintainability.

---

## 9) Follow-along Example: A Secure Player Class

```csharp
class Player
{
    private int health;
    private int maxHealth;

    public int Health
    {
        get { return health; }
        private set
        {
            if (value < 0) health = 0;
            else if (value > maxHealth) health = maxHealth;
            else health = value;
        }
    }

    public Player(int maxHealth)
    {
        this.maxHealth = maxHealth;
        Health = maxHealth;
    }

    public void TakeDamage(int amount)
    {
        Health -= amount;
    }

    public void Heal(int amount)
    {
        Health += amount;
    }
}
```

What this shows:

- `health` is private → true internal state
    
- `Health` property enforces limits
    
- All changes go through controlled logic
    
- The player can never be in an invalid health state
    

---
# 🧳 Encapsulation: Building Protective Boundaries Around Objects

Encapsulation is one of the central pillars of Object-Oriented Programming.

At its core, **encapsulation** means:

> Bundling data and behavior together, and hiding internal details so that objects can only be used through a safe, well-defined interface.

Encapsulation answers a critical design question:

**Who is allowed to change this data — and how?**

Instead of letting any part of the program reach in and manipulate raw values, encapsulation says:

> “If you want something from me, ask me. Don’t reach inside me.”

This transforms objects from passive data containers into **self-protecting, self-managing units**.

---

## 1) Life Without Encapsulation: Fragile Objects

Without encapsulation, objects become fragile and easy to break.

```csharp
class BankAccount
{
    public decimal Balance;
}
```

Now any code can do:

```csharp
account.Balance = -5000000;
```

The object is now in an impossible state. The class has no way to defend itself.

This is like allowing anyone to directly modify a company’s financial database table by hand.

---

## 2) Encapsulation Introduces a Boundary

Encapsulation draws a clear boundary:

- Inside the object → private, protected, internal details
    
- Outside the object → public interface (methods + properties)
    

```csharp
class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
        private set
        {
            if (value < 0)
                throw new InvalidOperationException("Balance cannot be negative.");

            balance = value;
        }
    }

    public void Deposit(decimal amount)
    {
        Balance += amount;
    }

    public void Withdraw(decimal amount)
    {
        Balance -= amount;
    }
}
```

Now:

- The data is hidden
    
- The rules live inside the class
    
- All interaction is mediated
    

This is encapsulation in action.

---

## 3) Encapsulation = Ownership of Rules

With encapsulation, the class becomes the **owner of its rules**.

Rules such as:

- “Balance cannot be negative”
    
- “Health must stay between 0 and max”
    
- “Age must be realistic”
    
- “Temperature must stay in a safe range”
    

These rules should not be scattered across the program.

They should live **inside the object that owns the data**.

This creates a powerful guarantee:

> If the object exists, its rules are being followed.

---

## 4) Encapsulation Is More Than Just `private`

Encapsulation is not just a keyword.

It is a **design philosophy**:

- Hide what doesn’t need to be seen
    
- Expose only what is safe to use
    
- Make misuse impossible or difficult
    

You can technically have `private` fields and still design poorly.

True encapsulation means:

- Thoughtful public methods
    
- Intentional properties
    
- Clear responsibilities
    

---

## 5) Follow-along Example: Encapsulating Health Logic

### Without encapsulation (fragile):

```csharp
class Player
{
    public int Health;
    public int MaxHealth;
}
```

Now many parts of the program might do:

```csharp
player.Health -= 50;
player.Health = 9999;
player.Health = -300;
```

Rules are scattered. Bugs are inevitable.

---

### With encapsulation (robust):

```csharp
class Player
{
    private int health;
    private int maxHealth;

    public int Health
    {
        get { return health; }
        private set
        {
            if (value < 0) health = 0;
            else if (value > maxHealth) health = maxHealth;
            else health = value;
        }
    }

    public Player(int maxHealth)
    {
        this.maxHealth = maxHealth;
        Health = maxHealth;
    }

    public void TakeDamage(int amount)
    {
        Health -= amount;
    }

    public void Heal(int amount)
    {
        Health += amount;
    }
}
```

Now:

- Health rules live in exactly one place
    
- The object is always valid
    
- External code cannot break invariants
    
- The class protects itself
    

---

## 6) Encapsulation Enables Reasoning and Trust

With encapsulation, you can reason locally:

If you trust the `Player` class, then:

- Any `Player` object is always in a valid state
    
- You don’t have to audit the entire codebase
    
- You only need to inspect the class itself
    

This massively reduces mental load in large systems.

---

## 7) Encapsulation Enables Change Without Fear

Encapsulation allows internal changes without external breakage.

Example:

### Version 1 (simple)

```csharp
private int health;
```

### Version 2 (more complex)

```csharp
private int baseHealth;
private int bonusHealth;
```

As long as `Health` property behaves the same, outside code is unaffected.

Encapsulation is what makes **refactoring safe**.

---

## 8) Encapsulation vs. Convenience

Beginners often prefer convenience:

> “It’s faster to just make it public.”

But convenience today creates bugs tomorrow.

Encapsulation trades a small amount of typing for:

- Fewer bugs
    
- Safer code
    
- Clearer design
    
- Easier maintenance
    

Professional code heavily favors encapsulation.

---

## 9) Encapsulation Turns Objects into Guardians

Encapsulated objects are not just data holders.

They are:

- Guardians of their own state
    
- Enforcers of their own rules
    
- Authorities over their own invariants
    

This is why good OOP objects feel _alive_:

You don’t tell them what to do internally.  
You **request behavior**, and they handle the details.

---
# 🪟 Properties : `get`, `set`, validation, and auto-properties

A **property** is a controlled “window” into an object’s data.

It _looks_ like a field when you use it:

```csharp
player.Health = 80;
Console.WriteLine(player.Health);
```

…but behind the scenes it can run logic, enforce rules, or block access.

A property is usually built from two **accessors**:

- **`get`** → returns the value (read access)
    
- **`set`** → updates the value (write access)
    

---

## 1) `get` and `set`: what they really mean

### A simple property with explicit backing field

```csharp
class Lamp
{
    private bool isOn;   // backing field (private storage)

    public bool IsOn     // property (public window)
    {
        get { return isOn; }          // read the private field
        set { isOn = value; }         // write to the private field
    }
}
```

### How it behaves

```csharp
Lamp lamp = new Lamp();

lamp.IsOn = true;                 // calls set { isOn = value; }
Console.WriteLine(lamp.IsOn);     // calls get { return isOn; }
```

Important detail: inside `set`, the keyword **`value`** is the incoming value being assigned.

So when you do:

```csharp
lamp.IsOn = true;
```

…`value` is `true`.

---

## 2) Why not just use public fields?

Because fields cannot protect themselves.

This is risky:

```csharp
class Student
{
    public int Age;
}
```

Now someone can do:

```csharp
student.Age = -200;
```

Properties allow you to **prevent invalid states**.

---

## 3) Validation in `set`: enforcing rules

Validation means: _“Only accept values that keep the object in a valid state.”_

### Example: prevent negative ages

```csharp
class Student
{
    private int age;

    public int Age
    {
        get { return age; }
        set
        {
            if (value < 0)
                throw new ArgumentException("Age cannot be negative.");

            age = value;
        }
    }
}
```

Usage:

```csharp
Student s = new Student();
s.Age = 18;           // OK
s.Age = -3;           // throws an exception
```

This is a big OOP win: the class **guards its own data**.

---

## 4) Different validation strategies

There are a few common ways to handle invalid values. You’ll see all of these in real code.

### Strategy A: Throw an exception (strict)

Best when invalid input is truly an error.

```csharp
set
{
    if (value < 0) throw new ArgumentException("Must be >= 0");
    age = value;
}
```

### Strategy B: Clamp the value (gentle)

Best when you want the object to “self-correct” rather than crash.

```csharp
set
{
    if (value < 0) age = 0;
    else age = value;
}
```

### Strategy C: Ignore invalid updates (silent)

Sometimes used in UI scenarios, but can hide bugs if overused.

```csharp
set
{
    if (value >= 0)
        age = value;
}
```

---

## 5) Read-only and write-restricted properties

You can control access by limiting which accessor is public.

### Read-only from the outside (`private set`)

```csharp
class BankAccount
{
    public double Balance { get; private set; }

    public void Deposit(double amount)
    {
        if (amount <= 0) throw new ArgumentException("Deposit must be positive.");
        Balance += amount;
    }
}
```

Now this is **not allowed**:

```csharp
account.Balance = 1000000; // ❌ won't compile (private set)
```

But this is allowed:

```csharp
Console.WriteLine(account.Balance); // ✅ get is public
```

This is very common: “everyone can read it, only the class can change it.”

---

## 6) Auto-properties: clean syntax for simple cases

If you _don’t need validation or custom logic_, C# lets you write an **auto-property**.

### Auto-property example

```csharp
class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
}
```

C# secretly creates a backing field for you.

Usage:

```csharp
Book b = new Book();
b.Title = "Dune";
Console.WriteLine(b.Title);
```

Auto-properties are perfect for simple “store and retrieve” data.

---

## 7) Auto-properties with restricted setters

You can combine auto-properties with encapsulation:

```csharp
class GameCharacter
{
    public string Name { get; }
    public int Level { get; private set; } = 1;

    public GameCharacter(string name)
    {
        Name = name;
    }

    public void GainLevel()
    {
        Level++;
    }
}
```

- `Name` is set only in the constructor (no `set`)
    
- `Level` can be read publicly, but changed only by the class
    
- `= 1` gives a default starting value
    

---

## 8) When you _need_ a backing field vs auto-property

### Use an auto-property when:

- You just need storage
    
- No validation, no side effects
    

```csharp
public string Email { get; set; }
```

### Use a backing field when:

- You want validation
    
- You want transformation (e.g., trimming whitespace)
    
- You want side effects (e.g., logging, triggering events)
    

Example (trim + validate):

```csharp
class User
{
    private string username;

    public string Username
    {
        get { return username; }
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Username required.");

            username = value.Trim();
        }
    }
}
```

---

## 9) A “follow-along” mini example: property validation in action

```csharp
class Thermostat
{
    private double temperature;

    public double Temperature
    {
        get { return temperature; }
        set
        {
            if (value < 5) temperature = 5;
            else if (value > 30) temperature = 30;
            else temperature = value;
        }
    }
}
```

Usage:

```csharp
Thermostat t = new Thermostat();

t.Temperature = 100;
Console.WriteLine(t.Temperature); // 30 (clamped)

t.Temperature = -20;
Console.WriteLine(t.Temperature); // 5 (clamped)
```

The object stays realistic, even if the caller behaves badly.

---

## 🧠 How These Concepts Work Together

Let’s see a complete, realistic example:

```csharp
class Player
{
    private int health;

    public string Name { get; set; }

    public int Health
    {
        get { return health; }
        private set
        {
            if (value < 0)
                health = 0;
            else
                health = value;
        }
    }

    public Player(string name)
    {
        Name = name;
        Health = 100;
    }

    public void TakeDamage(int amount)
    {
        Health -= amount;
    }
}
```

### What concepts are here?

|Concept|Example in Code|
|---|---|
|Class|`Player`|
|Object|`new Player("Alex")`|
|Method|`TakeDamage()`|
|Encapsulation|`private health`|
|Property|`Health`|

### Using the class:

```csharp
Player hero = new Player("Alex");
hero.TakeDamage(30);

Console.WriteLine(hero.Health); // 70
```

The object protects itself. You cannot directly set `Health` to a negative value. The class enforces its own rules.

---

# 🏗️ Constructors: Bringing Objects to Life

If a class is a blueprint and an object is a living instance, then a **constructor** is the moment of birth.

A constructor is a special method that runs **when an object is created**.  
It is responsible for setting up the object so that it starts its life in a valid, meaningful state.

Constructors answer a critical question:

> What must be true the moment this object comes into existence?

They ensure that no object is born broken.

---

## 1) The Role of a Constructor

Without constructors, objects are created empty and unprepared.

```csharp
class Player
{
    public string Name;
    public int Health;
}
```

```csharp
Player p = new Player();
```

At this point:

- Name = null
    
- Health = 0
    

Is that a valid player? Probably not.

The constructor exists to prevent this kind of incomplete birth.

---

## 2) A Basic Constructor

A constructor:

- Has the same name as the class
    
- Has no return type
    
- Runs automatically on `new`
    

```csharp
class Player
{
    public string Name;
    public int Health;

    public Player()
    {
        Name = "Unknown";
        Health = 100;
    }
}
```

Now:

```csharp
Player p = new Player();
```

The object is born with:

- A name
    
- Full health
    

The constructor establishes a **starting contract**.

---

## 3) Constructors Enforce Class Invariants at Birth

A **class invariant** is a rule that must always be true.

Examples:

- Health must be ≥ 0
    
- Balance must not be negative
    
- MaxHealth must be set
    
- A User must have a username
    

Constructors are the first and strongest place to enforce these.

```csharp
class BankAccount
{
    private decimal balance;

    public BankAccount()
    {
        balance = 0;
    }
}
```

Now it is impossible to create a BankAccount with an uninitialized balance.

---

## 4) Parameterized Constructors: Requiring Information

Often, an object cannot exist meaningfully without some information.

```csharp
class Player
{
    public string Name;
    public int MaxHealth;
    public int Health;

    public Player(string name, int maxHealth)
    {
        Name = name;
        MaxHealth = maxHealth;
        Health = maxHealth;
    }
}
```

Now:

```csharp
Player hero = new Player("Alex", 100);
```

The constructor _forces_ the caller to supply required data.

This prevents half-built objects.

---

## 5) Constructors as Gates, Not Just Setup

Constructors are not just for assigning values.

They are **gatekeepers**.

They decide:

- Whether construction is allowed
    
- Whether inputs are valid
    
- Whether the object can exist at all
    

```csharp
public Player(string name, int maxHealth)
{
    if (string.IsNullOrWhiteSpace(name))
        throw new ArgumentException("Name required.");

    if (maxHealth <= 0)
        throw new ArgumentException("MaxHealth must be positive.");

    Name = name;
    MaxHealth = maxHealth;
    Health = maxHealth;
}
```

Now invalid objects cannot be created.

This is powerful encapsulation at birth.

---

## 6) Overloaded Constructors: Multiple Ways to Be Born

A class can have multiple constructors.

This is called **constructor overloading**.

```csharp
class User
{
    public string Username;
    public bool IsGuest;

    public User(string username)
    {
        Username = username;
        IsGuest = false;
    }

    public User()
    {
        Username = "Guest";
        IsGuest = true;
    }
}
```

Now:

```csharp
User u1 = new User("Alice");
User u2 = new User();
```

Same class.  
Different birth stories.

---

## 7) Constructor Chaining: Reusing Initialization Logic

Constructors can call other constructors.

```csharp
class User
{
    public string Username;
    public bool IsGuest;

    public User(string username, bool isGuest)
    {
        Username = username;
        IsGuest = isGuest;
    }

    public User() : this("Guest", true)
    {
    }
}
```

This prevents duplication and keeps rules centralized.

---

## 8) Constructors and Private Fields

Constructors are where private fields are safely initialized.

```csharp
class Thermostat
{
    private double temperature;

    public Thermostat(double initialTemp)
    {
        if (initialTemp < 5 || initialTemp > 30)
            throw new ArgumentException();

        temperature = initialTemp;
    }
}
```

This guarantees:

- The private field is always valid
    
- No object can exist with an illegal temperature
    

---

## 9) Constructors and Encapsulation

Encapsulation is not just about hiding data later.

It begins at construction.

Constructors:

- Control object creation
    
- Prevent invalid initial state
    
- Establish invariants immediately
    

An object that starts valid is easier to keep valid.
