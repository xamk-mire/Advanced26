
## What is LINQ?

You can read more about **LINQ** here: [# Language Integrated Query (LINQ)][https://learn.microsoft.com/en-us/dotnet/csharp/linq/]

**LINQ** stands for **Language Integrated Query**. It’s a set of C# features + .NET APIs that let you write **query-like operations** over data using a consistent syntax.

You can use LINQ to query:

- **In-memory collections**: `List<T>`, arrays, `Dictionary<TKey,TValue>` (via `IEnumerable<T>`)
    
- **Databases**: via providers like **Entity Framework** (via `IQueryable<T>`)
    
- **XML**: LINQ to XML
    
- **Other data sources**: anything that can be exposed as an `IEnumerable<T>` or `IQueryable<T>`
    

### What LINQ is used for

LINQ is mainly used to:

- **Filter** data (`Where`)
    
- **Project/transform** data (`Select`)
    
- **Sort** data (`OrderBy`, `ThenBy`)
    
- **Group** data (`GroupBy`)
    
- **Join** data sets (`Join`, `GroupJoin`)
    
- **Aggregate** values (`Count`, `Sum`, `Average`, `Min`, `Max`)
    
- **Check conditions** (`Any`, `All`, `Contains`)
    
- **Get single items** (`First`, `Single`, `FirstOrDefault`, etc.)
    

### Two LINQ syntaxes

LINQ can be written in two equivalent styles:

#### 1) Method syntax (most common in modern C#)

```csharp
var adults = people
    .Where(p => p.Age >= 18)
    .OrderBy(p => p.LastName)
    .Select(p => p.FirstName);
```

#### 2) Query syntax (SQL-ish, rarely used, found in legacy code)

```csharp
var adults =
    from p in people
    where p.Age >= 18
    orderby p.LastName
    select p.FirstName;
```

Under the hood, **query syntax compiles into method syntax** calls like `Where`, `Select`, etc.

---

## What is a lambda?

You can read more about **lambda** here: [# Lambda expressions and anonymous functions][https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions]

A **lambda expression** is a compact way to write an **anonymous function**—a function without a name.

In C#, lambdas are typically used wherever you need a **delegate** or **expression tree**, such as:

- LINQ operators (`Where`, `Select`, `OrderBy`, …)
    
- Events and callbacks
    
- `Task` continuations
    
- `List<T>.Sort`, `FindAll`, etc.
    

### Lambda syntax basics

General form:

```csharp
(parameters) => expression_or_block
```

- Parameters are used in the expression_or_block section 

Examples:

**One parameter, expression body**

```csharp
x => x * 2
```

- Here  _x_ parameter could be a number in a list (e.g. x = 2, so the expression would result into 4)

**Multiple parameters**

```csharp
(a, b) => a + b
```


**Statement (block) body**

```csharp
x =>
{
    var y = x * 2;
    return y + 1;
}
```

- Statement blocks are used when we want to perform more complex operations or to increase readability of the code

### What lambdas are used for (in practice)

Lambdas are used to:

- Define “what to do” **inline** without writing a separate named method (anonym functions)
    
- Pass logic into APIs that accept delegates
    
- Write concise, readable transformations and filters—especially with LINQ
    

---

## How LINQ and lambdas relate

LINQ methods like `Where` and `Select` **need a function** that describes the condition or transformation.

- `Where` needs: “given an item, should it be kept?” → `Func<T, bool>`
    
- `Select` needs: “given an item, what should it become?” → `Func<T, TResult>`
    

Lambdas are the natural way to provide those functions:

```csharp
var people = new List<Person>
{
    new("Aino", 17, "Helsinki"),
    new("Mika", 22, "Espoo"),
    new("Sara", 35, "Helsinki"),
};

people.Where(p => p.Age >= 18)     // p => bool, Mika, Sara
people.Select(p => p.Name)         // p => string, [Aino, Mika, Sara]
people.OrderBy(p => p.City)    // p => key, Aino, Sara, Mika
```

---

## Key LINQ concepts you should know early

### 1) Deferred execution (important!)

Most LINQ operators (like `Where`, `Select`) don’t run immediately. They build a query pipeline that runs **when you enumerate** it (e.g., `foreach`, `ToList()`, etc.).

```csharp
var q = people.Where(p => p.Age >= 18); // not executed yet
var list = q.ToList();                 // executed here
// now the list would contain Mika and Sara
```

Why it matters:

- Efficient pipelines, can avoid unnecessary work -> only perform the operations when needed
    
- But results may change if the source collection changes before enumeration, which is why it's important to keep track of the code
    

### 2) `IEnumerable<T>` vs `IQueryable<T>`

- **`IEnumerable<T>`**: runs in memory (LINQ to Objects)
    
- **`IQueryable<T>`**: can be translated to another query language (like SQL in EF)
    

Example difference:

```csharp
// In EF, this can become SQL:
db.People.Where(p => p.Age >= 18)
```

- Here the **db** would refer to the database connected to the backend, and **People** refers to the table existing in the database
- This query now, should return all people from the database whose age 18 or older

This is why some lambdas might be restricted in EF queries: the provider must be able to translate your expression into runnable query.

---

## Common LINQ methods, mapped to “what they mean”

- `Where`: filter items
    
- `Select`: transform items
    
- `SelectMany`: flatten nested sequences
    
- `OrderBy` / `ThenBy`: sort
    
- `GroupBy`: group by key
    
- `Join`: match items from two sequences
    
- `Any` / `All`: existence / universal checks
    
- `First` / `Single`: pick one (with different rules)
    
- `ToList` / `ToArray`: materialize (execute and store)
    
- `Aggregate`, `Sum`, `Count`: reduce
    

---

## A small end-to-end example

Imagine:

```csharp
record Person(string Name, int Age, string City);

var people = new List<Person>
{
    new("Aino", 17, "Helsinki"),
    new("Mika", 22, "Espoo"),
    new("Sara", 35, "Helsinki"),
};
```

Query (What we want to get): “Get names of adults in Helsinki, sorted by age”

```csharp
var result = people
    .Where(p => p.Age >= 18 && p.City == "Helsinki")
    .OrderBy(p => p.Age)
    .Select(p => p.Name)
    .ToList();
```

- The expected result would be a string list of names containing only "Mika" & "Sara" because of ".Select(p => p.name)" 

Here:

- LINQ provides the operators (`Where`, `OrderBy`, `Select`)
    
- Lambdas define the logic passed into them (`p => p.Age >= 18 ...`, `p => p.Age`, `p => p.Name`)
    
- `ToList()` executes the query
    

---

## Example sample data 

```csharp
public record Person(int Id, string Name, int Age, string City, string Department, decimal Salary);

var people = new List<Person>
{
    new(1, "Aino", 17, "Helsinki", "Engineering", 0),
    new(2, "Mika", 22, "Espoo", "Engineering", 4200),
    new(3, "Sara", 35, "Helsinki", "Design", 5100),
    new(4, "Olli", 29, "Tampere", "Engineering", 4700),
    new(5, "Noora", 41, "Espoo", "HR", 4600),
    new(6, "Leo", 22, "Helsinki", "Design", 3900),
};
```

---

## 1) Predicates, anonymous functions (lambdas), delegates

### 1.1 Predicate with `Func<T, bool>`

```csharp
Func<Person, bool> isAdult = p => p.Age >= 18;

var adults = people.Where(isAdult).Select(p => p.Name).ToList();
```

**How it works**

- `Func<Person, bool>` is a delegate type: it represents a function that takes a `Person` and returns `bool`.
    
- `isAdult` is a predicate (because it returns `bool`).
    
- `Where(isAdult)` filters items where the predicate returns `true`.
    
- `Select(p => p.Name)` projects filtered `Person` objects into strings (names).
    
- `ToList()` executes the query and materializes the results.
    

**Expected result**

```text
["Mika", "Sara", "Olli", "Noora", "Leo"]
```

---

### 1.2 Same predicate as a named method (method group)

```csharp
bool IsAdult(Person p) => p.Age >= 18;

var adults = people.Where(IsAdult).Select(p => p.Name).ToList();
```

**How it works**

- `IsAdult` is a normal method, but C# can pass it as a delegate automatically (a “method group”).
    
- LINQ treats it the same as a lambda predicate.
    

**Expected result**

```text
["Mika", "Sara", "Olli", "Noora", "Leo"]
```

---

## 2) Filtering (Where)

### 2.1 Filter by multiple conditions

```csharp
var helsinkiAdults = people
    .Where(p => p.City == "Helsinki" && p.Age >= 18)
    .Select(p => p.Name)
    .ToList();
```

**How it works**

- `Where` evaluates the lambda for each person.
    
- Only persons who match both conditions stay in the sequence.
    
- Then we project to names.
    

**Expected result**

```text
["Sara", "Leo"]
```

---

### 2.2 “Find the first match”: `First` vs `FirstOrDefault`

```csharp
var firstEngineer = people.First(p => p.Department == "Engineering");
var maybeFinance = people.FirstOrDefault(p => p.Department == "Finance");
```

**How it works**

- `First(...)` returns the first matching item; throws if none match.
    
- `FirstOrDefault(...)` returns the first matching item, or `default` if none match (`null` for reference types; for records/classes, it’s `null`).
    

**Expected result**

- `firstEngineer` → `Person(Id=1, Name="Aino", ...)` (Aino is the first Engineering person in the list)
    
- `maybeFinance` → `null`
    

---

## 3) Projection (Select)

### 3.1 Project to a single property (names)

```csharp
var names = people.Select(p => p.Name).ToList();
```

**How it works**

- `Select` transforms each `Person` into `p.Name`.
    
- The output sequence type becomes `IEnumerable<string>`.
    

**Expected result**

```text
["Aino", "Mika", "Sara", "Olli", "Noora", "Leo"]
```

---

### 3.2 Project (shape) into an anonymous object

```csharp
var cards = people
    .Select(p => new { p.Name, p.City, IsAdult = p.Age >= 18 })
    .ToList();
```

**How it works**

- `new { ... }` creates an anonymous type (compiler-generated).
    
- Each element becomes an object with `Name`, `City`, `IsAdult`.
    

**Expected result (conceptually)**

```text
[
  { Name="Aino",  City="Helsinki", IsAdult=false },
  { Name="Mika",  City="Espoo",    IsAdult=true  },
  { Name="Sara",  City="Helsinki", IsAdult=true  },
  { Name="Olli",  City="Tampere",  IsAdult=true  },
  { Name="Noora", City="Espoo",    IsAdult=true  },
  { Name="Leo",   City="Helsinki", IsAdult=true  }
]
```

---

### 3.3 Index-aware projection

```csharp
var numbered = people
    .Select((p, index) => $"{index}: {p.Name}")
    .ToList();
```

**How it works**

- This `Select` overload supplies both the item and its index in the enumeration.
    
- We build a string per item.
    

**Expected result**

```text
["0: Aino", "1: Mika", "2: Sara", "3: Olli", "4: Noora", "5: Leo"]
```

---

## 4) Sorting (OrderBy / ThenBy)

### 4.1 Sort by age ascending

```csharp
var byAge = people.OrderBy(p => p.Age).Select(p => p.Name).ToList();
```

**How it works**

- `OrderBy` sorts using the key returned by the lambda (`Age`).
    
- Then we project to names so the output is easy to read.
    

**Expected result**

```text
["Aino", "Mika", "Leo", "Olli", "Sara", "Noora"]
```

---

### 4.2 Multi-level sort: city asc, then age desc

```csharp
var sorted = people
    .OrderBy(p => p.City)
    .ThenByDescending(p => p.Age)
    .Select(p => $"{p.City} - {p.Name} ({p.Age})")
    .ToList();
```

**How it works**

- `OrderBy` sorts by `City` first.
    
- `ThenByDescending` sorts within the same city group by age descending.
    

**Expected result**

```text
[
  "Espoo - Noora (41)",
  "Espoo - Mika (22)",
  "Helsinki - Sara (35)",
  "Helsinki - Leo (22)",
  "Helsinki - Aino (17)",
  "Tampere - Olli (29)"
]
```

---

## 5) Grouping (GroupBy)

### 5.1 Group by city and compute summary stats

```csharp
var cityStats = people
    .GroupBy(p => p.City)
    .Select(g => new
    {
        City = g.Key,
        Count = g.Count(),
        AvgAge = g.Average(p => p.Age)
    })
    .ToList();
```

**How it works**

- `GroupBy` creates groups: each group is an `IGrouping<TKey, TElement>`.
    
- `g.Key` is the city name.
    
- `Count()` counts members in the group.
    
- `Average(p => p.Age)` averages ages in that group.
    

**Expected result**

```text
[
  { City="Helsinki", Count=3, AvgAge=24.6666666667 },
  { City="Espoo",    Count=2, AvgAge=31.5 },
  { City="Tampere",  Count=1, AvgAge=29 }
]
```

> Note: ordering of groups follows first appearance of each key in LINQ to Objects.

---

### 5.2 Group by composite key (City + Department)

```csharp
var grouped = people
    .GroupBy(p => new { p.City, p.Department })
    .Select(g => new
    {
        g.Key.City,
        g.Key.Department,
        Count = g.Count()
    })
    .ToList();
```

**How it works**

- The key is an anonymous object `{ City, Department }`.
    
- People are grouped by exact key equality (same city AND same department).
    

**Expected result**

```text
[
  { City="Helsinki", Department="Engineering", Count=1 }, // Aino
  { City="Espoo",    Department="Engineering", Count=1 }, // Mika
  { City="Helsinki", Department="Design",      Count=2 }, // Sara, Leo
  { City="Tampere",  Department="Engineering", Count=1 }, // Olli
  { City="Espoo",    Department="HR",          Count=1 }  // Noora
]
```

---

## 6) Aggregation (Count, Sum, Average, Min, Max, Aggregate)

### 6.1 Total salary paid to Engineering

```csharp
var engineeringTotal = people
    .Where(p => p.Department == "Engineering")
    .Sum(p => p.Salary);
```

**How it works**

- Filter to engineering.
    
- `Sum` adds the salary selector across remaining items.
    

**Expected result**

```text
8900
```

(because `0 + 4200 + 4700 = 8900`)

---

### 6.2 Max age and min salary

```csharp
var maxAge = people.Max(p => p.Age);
var minSalary = people.Min(p => p.Salary);
```

**How it works**

- `Max(selector)` finds the maximum selector value.
    
- `Min(selector)` finds the minimum selector value.
    

**Expected result**

```text
maxAge = 41
minSalary = 0
```

---

### 6.3 `Aggregate` (custom reduction)

```csharp
var nameLine = people
    .Select(p => p.Name)
    .Aggregate((a, b) => $"{a}, {b}");
```

**How it works**

- `Aggregate` repeatedly combines items:
    
    - start with first two → combine → combine with next → etc.
        
- Here it builds a comma-separated string.
    

**Expected result**

```text
"Aino, Mika, Sara, Olli, Noora, Leo"
```

---

## 7) Quantifiers (Any / All) + Contains

### 7.1 `Any` and `All`

```csharp
var hasZeroSalary = people.Any(p => p.Salary == 0);
var allAdultsPaid = people
    .Where(p => p.Age >= 18)
    .All(p => p.Salary > 0);
```

**How it works**

- `Any` returns true if at least one matches.
    
- `All` returns true only if every element matches.
    
- We filter to adults before checking their salaries.
    

**Expected result**

```text
hasZeroSalary = true
allAdultsPaid = true
```

---

### 7.2 `Contains` with a list of allowed cities

```csharp
var allowed = new[] { "Helsinki", "Espoo" };

var inAllowedCities = people
    .Where(p => allowed.Contains(p.City))
    .Select(p => p.Name)
    .ToList();
```

**How it works**

- For each person, it checks if their city is in the `allowed` array.
    
- Keeps only those in Helsinki or Espoo.
    

**Expected result**

```text
["Aino", "Mika", "Sara", "Noora", "Leo"]
```

---

## 8) Distinct and set-like operations

### 8.1 Distinct cities

```csharp
var uniqueCities = people.Select(p => p.City).Distinct().ToList();
```

**How it works**

- Projects to city names.
    
- `Distinct` removes duplicates (keeps first occurrence order for LINQ to Objects).
    

**Expected result**

```text
["Helsinki", "Espoo", "Tampere"]
```

---

## 9) Materialization and conversions

### 9.1 ToDictionary by Id

```csharp
var byId = people.ToDictionary(p => p.Id);
```

**How it works**

- Creates a dictionary where key = `Id`, value = the `Person`.
    
- Throws if duplicate keys exist.
    

**Expected result**

- `byId[3].Name` is `"Sara"`
    
- keys: `1,2,3,4,5,6`
    

---

## 10) SelectMany (flattening)

### 10.1 Flatten characters from all names

```csharp
var chars = people
    .SelectMany(p => p.Name)
    .ToList();
```

**How it works**

- `p.Name` is a `string`, which is `IEnumerable<char>`.
    
- `SelectMany` takes each name’s characters and flattens them into one long sequence.
    

**Expected result (first ~20 chars shown)**

```text
['A','i','n','o','M','i','k','a','S','a','r','a','O','l','l','i','N','o','o','r', ...]
```

---

## 11) Join (common in data work)

### 11.1 Join people with department managers

```csharp
public record Department(string Name, string Manager);

var depts = new List<Department>
{
    new("Engineering", "Kari"),
    new("Design", "Elina"),
    new("HR", "Sanna"),
};

var joined = people.Join(
    depts,
    p => p.Department,   // key from people
    d => d.Name,         // key from departments
    (p, d) => new { p.Name, Department = d.Name, d.Manager }
).ToList();
```

**How it works**

- Matches elements where `p.Department == d.Name`.
    
- Produces a new object for each match containing person name + manager.
    

**Expected result**

```text
[
  { Name="Aino",  Department="Engineering", Manager="Kari"  },
  { Name="Mika",  Department="Engineering", Manager="Kari"  },
  { Name="Sara",  Department="Design",      Manager="Elina" },
  { Name="Olli",  Department="Engineering", Manager="Kari"  },
  { Name="Noora", Department="HR",          Manager="Sanna" },
  { Name="Leo",   Department="Design",      Manager="Elina" }
]
```

---

