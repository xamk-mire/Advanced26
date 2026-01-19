# Unit Testing: a detailed introduction 

Imagine you’re building a machine with hundreds of tiny gears. Each gear might be perfect on its own, but if even one is slightly wrong, the whole machine can wobble, grind, or stop entirely.

Software is like that machine.

A *unit test* is your way of holding one gear in your hand—isolating it from the rest—spinning it, poking it, and confirming it behaves exactly as you expect.

When we say “unit,” we usually mean something small and focused: a method, a function, or a tiny piece of logic.
When we say “test,” we mean a repeatable, automated check that answers a simple question:

> Given this input, do I get the output I expect?

That’s the heart of unit testing.

---

## What unit tests are (and what they aren’t)

A unit test is not meant to prove your entire application works. It’s meant to prove that a *single component* behaves correctly.

If a unit test fails, it should feel like a clear message from your code:

> “This exact behavior is broken. Here’s where. Here’s how.”

Unit tests are usually fast. Really fast.
Hundreds or thousands of them should run in seconds.

* **A unit test checks a small piece of logic in isolation**
* **A unit test should be deterministic (same input → same result)**
* **A unit test should not depend on external systems** like databases, file systems, network calls, random time, etc.

---

## Why people take unit tests seriously (unfortunatelly many dont)

At first, tests feel like extra work—until you’ve worked on a project without them.

Without unit tests, every code change becomes a nervous act: you tweak something, then wonder what else you accidentally broke.

With unit tests, change becomes calmer. You rewrite internals, improve structure, optimize performance… and your test suite stays like a loyal guardian at the gate.

* **They protect you during refactoring**
* **They prevent regressions (old bugs coming back)**
* **They serve as “living documentation” for your code**
* **They improve design by encouraging smaller, testable units**

---

# The structure of a good unit test (AAA)

A good unit test is a short journey with three clear steps.

It begins by **building a tiny world**, just big enough for the rule you want to verify.
Then it performs **one meaningful action** inside that world.
Finally, it stops and asks a simple question:

> Did reality match expectation?

This is the rhythm known as **AAA**:

**Arrange → Act → Assert**

It’s not just a tradition. It’s a readability tool.
AAA makes a test understandable even when you return to it months later, when the codebase has changed and your memory has not kept up.

* **Arrange** creates the conditions for the test.
* **Act** triggers the behavior you want to verify.
* **Assert** checks the result is correct.

And most importantly: the three steps help you focus.
A unit test is supposed to be small. AAA gently enforces that smallness.

---

## Arrange: the scene-setting

The **Arrange** step is where you prepare everything the test needs, and nothing it doesn’t.

This might mean:

* creating the object you’re testing
* creating input values
* setting up a fake or mock dependency
* building minimal test data
* preparing special conditions (“this customer is premium”, “today is Saturday”, etc.)

The goal is not to mirror the entire production environment.
The goal is to create a controlled miniature version of it.

### Example: arrange some input

```csharp
[Fact]
public void ApplyDiscount_WhenDiscountIs10Percent_ReturnsReducedPrice()
{
    // Arrange
    var price = 100m;
    var discount = 10m;

    // Act
    var result = PriceCalculator.ApplyDiscount(price, discount);

    // Assert
    Assert.Equal(90m, result);
}
```

**What’s happening in Arrange?**

* We choose `100` and `10` because they’re easy to reason about.
* Tests should often prefer “friendly numbers” so the intent is obvious.
* This arrangement is minimal: it introduces only what is required for the behavior.

**Expected result**

* Not calculated yet—Arrange never *proves* anything. It only prepares the stage.

---

## Act: the single decisive move

The **Act** step is where you do exactly one meaningful thing.

Not three calls. Not a long chain of actions.
Just one action that triggers the behavior under test.

That one action might be:

* calling a method
* executing a command
* saving a value
* performing a computation
* invoking a service

### Example: act by calling the method under test

```csharp
// Act
var result = PriceCalculator.ApplyDiscount(price, discount);
```

**How it works**

* This is the moment the test is actually testing something.
* Everything before it was setup. Everything after it will be evaluation.

**A small but powerful rule**
If your Act step feels too large, that’s often a design signal:
your method might be doing too many things, or your test might be trying to test too much at once.

* **One test should usually have one main Act**
* **If Act is complicated, consider splitting responsibilities or writing separate tests**

---

## Assert: the verdict

The **Assert** step is where you state what must be true.

A test without a good assertion is like a lock without a key.
It’s shaped like security, but it doesn’t secure anything.

Assertions usually come in two forms:

* **State assertions**: “this value should be X”
* **Behavior/interaction assertions**: “this dependency should have been called”

### Example: assert on state

```csharp
// Assert
Assert.Equal(90m, result);
```

**How it works**

* `Assert.Equal(expected, actual)` is the classic state check.
* We don’t care how the method calculated it.
* We care that the outcome matches the contract.

**Expected result**

* ✅ Passes if `result` is `90`
* ❌ Fails if `result` is anything else

**Why this assertion is good**

* It checks the *observable behavior*.
* It doesn’t depend on the method’s internal implementation.

---

# AAA in a slightly more “real” example

Let’s test a rule that has branching behavior.

### Production code

```csharp
public static class AgeRules
{
    public static bool CanAccessAdultArea(int age) => age >= 18;
}
```

### Unit test

```csharp
[Fact]
public void CanAccessAdultArea_WhenAgeIs18_ReturnsTrue()
{
    // Arrange
    var age = 18;

    // Act
    var result = AgeRules.CanAccessAdultArea(age);

    // Assert
    Assert.True(result);
}
```

**Arrange**

* We choose `18` because it’s the exact boundary.

**Act**

* We call the method once.

**Assert**

* We confirm the rule behaves correctly at the edge.

**Expected result**

* ✅ `True`

This test is tiny, but incredibly meaningful: it guards the boundary where mistakes are common.

---

# The “hidden value” of AAA: it makes failures readable

A test fails at one of two moments:

1. It throws during Arrange or Act (unexpected crash)
2. It fails an assertion (expected behavior didn’t happen)

AAA tells you where to look.

* If it fails during **Arrange**, your setup is wrong or your dependencies are messy.
* If it fails during **Act**, the method under test is throwing unexpectedly.
* If it fails during **Assert**, the method ran but produced the wrong behavior.

Even without debugging, AAA acts like signposts.

---

# Common mistakes (and how AAA helps you avoid them)

### 1) Mixing Arrange and Act

People sometimes hide actions inside Arrange:

```csharp
// ❌ bad: doing work in Arrange that belongs in Act
var result = PriceCalculator.ApplyDiscount(100m, 10m);
```

This is not catastrophic, but it blurs the structure.
AAA prefers that the “moment of truth” remains obvious.

* **If the core action is hidden, the test reads less like a story**
* **Clear structure matters more as projects grow**

---

### 2) Multiple Acts in one test

A test with multiple big actions becomes confusing:

```csharp
// ❌ bad: too many things being tested
var r1 = PriceCalculator.ApplyDiscount(100m, 10m);
var r2 = PriceCalculator.ApplyDiscount(100m, 20m);

Assert.Equal(90m, r1);
Assert.Equal(80m, r2);
```

This test is doing two scenarios at once.
If it fails, you lose clarity.

Better options:

* Split into two tests, or
* Use a `[Theory]` with `[InlineData]` (parameterized test)

---

### 3) Weak asserts

Sometimes tests don’t verify meaningful outcomes:

```csharp
// ❌ weak assert: doesn’t prove the rule
Assert.NotNull(result);
```

A good assertion should match the *purpose* of the test.
“Not null” is rarely the real requirement.

* **Assert the rule, not the existence**
* **The assertion should be as specific as your intent**

---

# AAA with dependencies: when Arrange becomes more important

When dependencies exist (repositories, clocks, services), Arrange becomes the craft.

### Production code

```csharp
public interface IClock
{
    DateTime Now { get; }
}

public class CalendarService
{
    private readonly IClock _clock;

    public CalendarService(IClock clock) => _clock = clock;

    public bool IsWeekend() =>
        _clock.Now.DayOfWeek is DayOfWeek.Saturday or DayOfWeek.Sunday;
}
```

### Unit test with a fake clock

```csharp
public class FakeClock : IClock
{
    public DateTime Now { get; set; }
}

public class CalendarServiceTests
{
    [Fact]
    public void IsWeekend_WhenSaturday_ReturnsTrue()
    {
        // Arrange
        var clock = new FakeClock { Now = new DateTime(2026, 1, 17) }; // Saturday
        var service = new CalendarService(clock);

        // Act
        var result = service.IsWeekend();

        // Assert
        Assert.True(result);
    }
}
```

**Arrange**

* We create a fake clock and freeze time.
* We create the service using the fake dependency.

**Act**

* One call: `IsWeekend()`.

**Assert**

* We verify the exact expected boolean.

**Expected result**

* ✅ `true`

* **AAA shines brightest when dependency control matters**

* **Arrange is where you make the test deterministic**

---

# A deeper refinement: “Given / When / Then”

AAA is often expressed in more narrative terms:

* **Given** (Arrange)
* **When** (Act)
* **Then** (Assert)

That’s why AAA reads so naturally.
It’s the same pattern humans use to explain cause and effect.

Example:

```csharp
// Given a Saturday
// When I check if it is weekend
// Then it should return true
```

* **AAA is structure**
* **Given/When/Then is storytelling**
* **Both describe the same idea**

---

# The “one breath” rule (a practical guideline)

Many strong unit tests follow an unspoken principle:

> You should be able to read the entire test in one breath.

Not literally, but emotionally.

When a test becomes too long, it stops being a unit test and starts becoming a miniature integration test.

* **Short tests are easier to trust**
* **Short tests fail for clearer reasons**
* **Short tests are easier to maintain**

---

# Tooling in C#: xUnit + FluentAssertions (and why people love them)

In modern .NET, the most common unit testing frameworks are:

* **xUnit**
* **NUnit**
* **MSTest**

I’ll use **xUnit** in examples because it’s widely used and clean.

For assertions, I’ll show both:

* normal assertions (`Assert.Equal`)
* *FluentAssertions* (more readable like English)

---

# Example 1: testing a pure function (the easiest kind)

Let’s begin with the purest kind of code: logic with no side effects.

### Production code

```csharp
public static class PriceCalculator
{
    public static decimal ApplyDiscount(decimal price, decimal discountPercent)
    {
        if (price < 0) throw new ArgumentOutOfRangeException(nameof(price));
        if (discountPercent is < 0 or > 100) throw new ArgumentOutOfRangeException(nameof(discountPercent));

        var factor = 1 - (discountPercent / 100m);
        return decimal.Round(price * factor, 2);
    }
}
```

This method doesn’t touch files, databases, or time.
It only takes inputs and returns output. That makes it perfect for unit testing.

### Unit tests (xUnit)

```csharp
using Xunit;

public class PriceCalculatorTests
{
    [Fact]
    public void ApplyDiscount_WhenDiscountIs10Percent_ReturnsReducedPrice()
    {
        // Arrange
        var price = 100m;
        var discount = 10m;

        // Act
        var result = PriceCalculator.ApplyDiscount(price, discount);

        // Assert
        Assert.Equal(90m, result);
    }

    [Fact]
    public void ApplyDiscount_WhenDiscountIs100Percent_ReturnsZero()
    {
        var result = PriceCalculator.ApplyDiscount(49.99m, 100m);
        Assert.Equal(0m, result);
    }
}
```

### How this works

The test doesn’t “run the whole application.”
It calls a single method directly and verifies the returned value.

* The first test ensures **10% discount reduces 100 to 90**
* The second ensures **100% discount reduces any price to 0**

**Expected results**

* Test 1 passes because `100 * 0.9 = 90`

* Test 2 passes because `49.99 * 0 = 0`

* **Fact means “this test is always true”**

* **Tests should describe behavior, not implementation details**

* **Good test names read like sentences**

---

# Example 2: testing edge cases (where bugs usually live)

Many real bugs hide at the edges: negative inputs, zeros, out-of-range values.

So we test them on purpose.

```csharp
[Fact]
public void ApplyDiscount_WhenPriceIsNegative_Throws()
{
    Assert.Throws<ArgumentOutOfRangeException>(
        () => PriceCalculator.ApplyDiscount(-1m, 10m)
    );
}

[Fact]
public void ApplyDiscount_WhenDiscountIsOver100_Throws()
{
    Assert.Throws<ArgumentOutOfRangeException>(
        () => PriceCalculator.ApplyDiscount(100m, 101m)
    );
}
```

### How this works

* `Assert.Throws<T>(...)` confirms the code throws the correct exception.
* The lambda `() => ...` prevents it from executing immediately, so xUnit can catch the exception.

**Expected results**

* Both tests pass because the method rejects invalid inputs.

* **Edge-case tests are not “extra” — they are often the most valuable**

* **When tests cover input validation, production bugs drop sharply**

---

# Example 3: using `[Theory]` + `[InlineData]` for multiple cases

Sometimes you want to test a pattern, not a single example.

xUnit gives you `[Theory]`, which means:

> “This test should work for multiple sets of values.”

```csharp
[Theory]
[InlineData(100, 10, 90)]
[InlineData(200, 25, 150)]
[InlineData(80, 50, 40)]
public void ApplyDiscount_WorksForManyValues(decimal price, decimal discount, decimal expected)
{
    var result = PriceCalculator.ApplyDiscount(price, discount);
    Assert.Equal(expected, result);
}
```

### How this works

This one test runs 3 times with different inputs.

**Expected results**

* 100 with 10% → 90

* 200 with 25% → 150

* 80 with 50% → 40

* **Theories are great when you want coverage without repeating code**

* **They keep your test suite smaller and cleaner**

---

# Unit testing “real-world code”: dependencies and isolation

So far, we’ve tested pure logic. That’s the easy road.

But most application code talks to other things:

* databases
* web APIs
* email senders
* clocks
* file systems

And here’s where beginners often stumble.

A good unit test does not depend on the outside world.

The trick is: **separate your logic from your dependencies**, and replace those dependencies during tests with something you control.

That “something” is usually a **mock**, **fake**, or **stub**.

* **Mock**: “I verify you were called this way.”
* **Stub**: “I return this data when asked.”
* **Fake**: “A simple working version (often in memory).”

---

# Example 4: testing code that depends on external services (mocking)

Let’s say we have a service that gives a bonus to employees, but we must confirm the employee exists first.

### Production code

```csharp
public interface IEmployeeRepository
{
    Person? FindById(int id);
}

public class BonusService
{
    private readonly IEmployeeRepository _repo;

    public BonusService(IEmployeeRepository repo)
    {
        _repo = repo;
    }

    public decimal CalculateBonus(int employeeId)
    {
        var employee = _repo.FindById(employeeId);
        if (employee is null)
            throw new InvalidOperationException("Employee not found");

        // Bonus rule: 10% of salary, but only if salary > 0
        return employee.Salary > 0 ? employee.Salary * 0.10m : 0m;
    }
}
```

The `BonusService` is not fully isolated because it depends on a repository.

So in a unit test, we replace the repository with a controlled fake.

### Unit test using a simple Fake (no mocking library required)

```csharp
public class FakeEmployeeRepository : IEmployeeRepository
{
    private readonly Dictionary<int, Person> _employees = new();

    public void Add(Person person) => _employees[person.Id] = person;

    public Person? FindById(int id)
        => _employees.TryGetValue(id, out var employee) ? employee : null;
}
```

Now the actual tests:

```csharp
public class BonusServiceTests
{
    [Fact]
    public void CalculateBonus_WhenSalaryIsPositive_Returns10Percent()
    {
        // Arrange
        var repo = new FakeEmployeeRepository();
        repo.Add(new Person(1, "Mika", 22, "Espoo", "Engineering", 4200));

        var service = new BonusService(repo);

        // Act
        var bonus = service.CalculateBonus(1);

        // Assert
        Assert.Equal(420m, bonus);
    }

    [Fact]
    public void CalculateBonus_WhenEmployeeDoesNotExist_Throws()
    {
        var repo = new FakeEmployeeRepository();
        var service = new BonusService(repo);

        Assert.Throws<InvalidOperationException>(() => service.CalculateBonus(999));
    }
}
```

### How this works

* We avoid a real database.
* The fake repository is just a dictionary in memory.
* This keeps the test fast, deterministic, and isolated.

**Expected results**

* Salary 4200 → bonus is 420

* Missing employee → exception thrown

* **Dependency injection is what makes this testable**

* **Fakes are often simpler than mocks**

* **Unit tests should never require a database**

---

# Example 5: the “time problem” (and how to fix it)

Time breaks unit tests because the clock moves.

### Bad design (hard to test)

```csharp
public bool IsWeekend()
{
    return DateTime.Now.DayOfWeek is DayOfWeek.Saturday or DayOfWeek.Sunday;
}
```

This test might pass today and fail tomorrow.

So we inject time instead.

### Good design (testable)

```csharp
public interface IClock
{
    DateTime Now { get; }
}

public class SystemClock : IClock
{
    public DateTime Now => DateTime.Now;
}

public class CalendarService
{
    private readonly IClock _clock;

    public CalendarService(IClock clock)
    {
        _clock = clock;
    }

    public bool IsWeekend()
    {
        return _clock.Now.DayOfWeek is DayOfWeek.Saturday or DayOfWeek.Sunday;
    }
}
```

### Tests (with fake clock)

```csharp
public class FakeClock : IClock
{
    public DateTime Now { get; set; }
}

public class CalendarServiceTests
{
    [Fact]
    public void IsWeekend_WhenSaturday_ReturnsTrue()
    {
        var clock = new FakeClock { Now = new DateTime(2026, 1, 17) }; // Saturday
        var service = new CalendarService(clock);

        var result = service.IsWeekend();

        Assert.True(result);
    }

    [Fact]
    public void IsWeekend_WhenMonday_ReturnsFalse()
    {
        var clock = new FakeClock { Now = new DateTime(2026, 1, 19) }; // Monday
        var service = new CalendarService(clock);

        var result = service.IsWeekend();

        Assert.False(result);
    }
}
```

### How this works

* We replaced “real time” with “controlled time.”
* Tests become stable and repeatable.

**Expected results**

* Saturday → `true`

* Monday → `false`

* **If your code uses DateTime.Now directly, it’s harder to unit test**

* **Injecting time makes logic predictable**

---

# What makes a unit test “good”

A good unit test feels like a small poem: short, clear, meaningful.

It checks one thing, and it reads like a statement about behavior.

* **One test should verify one behavior**
* **The name should describe the rule**
* **Avoid testing private methods directly**
* **Don’t assert on implementation details**
* **Prefer predictable inputs over complex setups**
