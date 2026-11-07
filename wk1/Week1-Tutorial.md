# Week 1 Tutorial: C# Fundamentals (Sessions 1 and 2)

Keep this file open on one side of VS Code and your terminal on the other.
**Type every example yourself.** Typing is how your fingers learn the syntax. Pasting skips that.

Every code sample in this file was compiled and run on .NET 10 before you got it. If something fails, the cause is almost always a typo, and finding it is good practice.

---

## Part 0: Get your folders in order (10 min)

### 0.1 Open a terminal in VS Code

Press **Ctrl + `** (the backtick key, left of `1`). A terminal opens at the bottom, already inside your repo folder `.NET_Tutorial`.

The terminal is **PowerShell**. Commands you'll use all the time:

| Command | What it does |
|---|---|
| `pwd` | **P**rint **w**orking **d**irectory: shows which folder you are in |
| `ls` | Lists the files and folders in the current folder |
| `cd wk1` | **C**hange **d**irectory: moves into the `wk1` folder |
| `cd ..` | Moves up one folder (`..` always means "parent folder") |
| `mkdir name` | **M**a**k**e **dir**ectory: creates a new folder |
| `cls` | Clears the screen |
| **Tab** key | Auto-completes folder and file names. Use it constantly |
| **↑** key | Brings back your previous command |

### 0.2 Move `Playground` into `wk1`

Right now your repo looks like this:

```
.NET_Tutorial/
├── Playground/      ← created at the top level
├── wk1/             ← empty
└── ...
```

Each week's work should live in its own folder, so move it:

```powershell
Move-Item Playground wk1\
```

- `Move-Item` is PowerShell's "move" command (`mv` also works as a shortcut).
- `Playground` is the folder being moved.
- `wk1\` is the destination. The trailing `\` means "put it inside wk1".

Check it worked:

```powershell
ls wk1
```

You should see `Playground` listed.

### 0.3 Add a `.gitignore` (important!)

When you build a .NET project, it creates `bin/` and `obj/` folders full of generated files. Those should **never** go into Git, because they are big, change on every build, and are useless to anyone else. Fix that once, at the repo root:

```powershell
dotnet new gitignore
```

- `dotnet new` creates something from a **template**.
- `gitignore` is the template name. It writes a `.gitignore` file that already knows about `bin/`, `obj/`, `.vs/` and other junk.

### 0.4 What did `dotnet new console` give you?

Go into the project:

```powershell
cd wk1\Playground
ls
```

You'll see two files (plus `bin`/`obj` if you've already run it):

**`Playground.csproj`**: the project file. Open it:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>
</Project>
```

| Line | Meaning |
|---|---|
| `OutputType Exe` | Build a runnable program (as opposed to a library other code uses) |
| `TargetFramework net10.0` | Run on .NET 10 |
| `ImplicitUsings enable` | Automatically imports common namespaces like `System` and `System.Collections.Generic`, so you can write `Console` and `List` without `using` lines at the top |
| `Nullable enable` | The compiler warns you when something might be `null`. Covered in Step 9 |

**`Program.cs`**: your code. It contains a single line:

```csharp
Console.WriteLine("Hello, World!");
```

- `Console` is a built-in class representing the terminal window.
- `.WriteLine(...)` prints the text in the brackets, then moves to a new line.
- `"Hello, World!"` is a **string**: text inside double quotes.
- `;` ends every statement in C#. Forgetting it is the #1 beginner error.

There's no `class Program` or `static void Main` as in older tutorials. This style is called **top-level statements**: the compiler wraps your code in a `Main` method for you. Older C# tutorials show the full wrapper, and both styles are valid.

### 0.5 Run it

```powershell
dotnet run
```

`dotnet run` does two things:
1. **Builds**: compiles your `.cs` files into a `.dll` inside `bin\Debug\net10.0\`.
2. **Runs**: starts that compiled program.

Output: `Hello, World!`

Related commands:

| Command | What it does |
|---|---|
| `dotnet build` | Compiles only, without running. Use it to check for errors |
| `dotnet run` | Builds if needed, then runs |
| `dotnet watch run` | Runs, then **re-runs automatically every time you save**. Great for this week. Press Ctrl+C to stop |
| `dotnet clean` | Deletes the build output, for when things get weird |

> **How this tutorial works:** for each step, **replace everything in `Program.cs`** with the step's code, run it, read the output, then do the exercise. Before moving to the next step, commit (see Part 3). Git then keeps every step, so nothing is lost when you overwrite the file.

---

## Session 1

## Step 1: Variables and types (40 min)

A **variable** is a named box that holds a value. In C#, every box has a fixed **type**, and it can only hold that kind of value. That is what "statically typed" means.

Replace `Program.cs` with:

```csharp
// Whole numbers
int age = 21;
long worldPopulation = 8_100_000_000;

// Numbers with decimals
double temperature = 31.5;
decimal price = 19.99m;

// Other basics
bool isStudent = true;
char grade = 'A';
string name = "Joshua";

Console.WriteLine(age);
Console.WriteLine(worldPopulation);
Console.WriteLine(temperature);
Console.WriteLine(price);
Console.WriteLine(isStudent);
Console.WriteLine(grade);
Console.WriteLine(name);
```

Run `dotnet run`.

**Reading a declaration:** `int age = 21;` means "make a box of type `int`, name it `age`, put `21` in it."

| Type | Holds | Example | Notes |
|---|---|---|---|
| `int` | Whole numbers up to about ±2.1 billion | `21` | Your default for counting |
| `long` | Very big whole numbers | `8_100_000_000` | `_` is just a visual separator, which the compiler ignores |
| `double` | Fast decimal numbers, slightly imprecise | `31.5` | Science, measurements |
| `decimal` | Exact decimal numbers | `19.99m` | **Money.** The `m` suffix is required |
| `bool` | `true` or `false` | `true` | Conditions |
| `char` | One character | `'A'` | **Single** quotes |
| `string` | Text | `"Joshua"` | **Double** quotes |

`//` starts a **comment**. The compiler ignores everything after it on that line.

### Why `decimal` for money? See for yourself

Add to the bottom:

```csharp
double d = 0.1 + 0.2;
decimal m = 0.1m + 0.2m;
Console.WriteLine(d);   // 0.30000000000000004  ← wrong!
Console.WriteLine(m);   // 0.3                  ← exact
```

`double` stores numbers in binary, and 0.1 has no exact binary form, just as 1/3 has no exact decimal form. Over thousands of transactions those tiny errors add up. `decimal` stores numbers in base 10, so it is exact for money.

### Integer division trap

```csharp
Console.WriteLine(7 / 2);     // 3    ← int / int throws away the remainder
Console.WriteLine(7 / 2.0);   // 3.5  ← if either side is a decimal type, you get decimals
Console.WriteLine(7 % 2);     // 1    ← % gives the remainder ("modulo")
```

### `var`: let the compiler figure out the type

```csharp
var count = 10;        // compiler sees 10, decides int
var message = "hi";    // decides string
var total = 4.5m;      // decides decimal

Console.WriteLine(count.GetType());    // System.Int32
Console.WriteLine(message.GetType());  // System.String
Console.WriteLine(total.GetType());    // System.Decimal
```

**`var` is still static typing.** The type is decided once at compile time and never changes. Try adding `count = "hello";` and run `dotnet build`. You'll get **error CS0029: Cannot implicitly convert type 'string' to 'int'**. Delete that line afterwards.

> **When to use `var`:** when the type is obvious from the right side (`var list = new List<string>();`). Write the type explicitly when it isn't obvious, or when you're learning and want to see it.

### `const`: a value that never changes

```csharp
const decimal TaxRate = 0.06m;
Console.WriteLine(price * TaxRate);   // 1.1994
```

### ✏️ Exercise 1
Declare variables for a product: name, unit price (decimal), quantity (int), and whether it's in stock (bool). Print the total cost as `quantity * unitPrice`. Then deliberately put `19.99` without the `m` into a `decimal` and read the compiler error.

---

## Step 2: Strings (35 min)

Replace `Program.cs` with:

```csharp
string raw = "   Ada Lovelace   ";
string trimmed = raw.Trim();
Console.WriteLine($"[{raw}]");
Console.WriteLine($"[{trimmed}]");
Console.WriteLine(trimmed.ToUpper());
Console.WriteLine(trimmed.ToLower());
Console.WriteLine(trimmed.Length);
```

Output:
```
[   Ada Lovelace   ]
[Ada Lovelace]
ADA LOVELACE
ada lovelace
12
```

| Member | What it does |
|---|---|
| `.Trim()` | Removes spaces from both ends. Use it on every bit of user input |
| `.ToUpper()` / `.ToLower()` | Changes case. Useful for case-insensitive comparisons |
| `.Length` | Number of characters. No `()`, because it's a **property**, not a method |

### String interpolation: `$"..."`

Put `$` before the opening quote, and anything inside `{ }` gets evaluated and inserted:

```csharp
decimal total = 1234.5m;
Console.WriteLine($"Total: {total}");       // Total: 1234.5
Console.WriteLine($"Total: {total:F2}");    // Total: 1234.50
Console.WriteLine($"Total: {total:N2}");    // Total: 1,234.50
Console.WriteLine($"Total: {total:C}");     // Total: RM1,234.50
Console.WriteLine($"Half: {total / 2:F2}"); // Half: 617.25  (expressions work too)
```

The part after `:` is a **format specifier**:

| Specifier | Meaning |
|---|---|
| `F2` | **F**ixed-point, 2 decimal places |
| `N2` | **N**umber with thousands separators, 2 decimals |
| `C` | **C**urrency, using your Windows region settings. Your machine is set to Malaysia, so it prints `RM` |

### Splitting and joining

```csharp
string csv = "food,transport,rent";
string[] parts = csv.Split(',');
Console.WriteLine(parts.Length);   // 3
Console.WriteLine(parts[0]);       // food   ← indexes start at 0
Console.WriteLine(parts[2]);       // rent
Console.WriteLine(string.Join(" | ", parts));  // food | transport | rent
```

- `.Split(',')` cuts the string at every comma and returns a `string[]`, an **array** (a fixed-size list) of strings.
- `parts[0]` reads the first item. Asking for `parts[3]` crashes with `IndexOutOfRangeException`, because there are only indexes 0, 1 and 2.
- `string.Join(separator, items)` does the reverse and glues items into one string.

### Checking for empty input

```csharp
Console.WriteLine(string.IsNullOrWhiteSpace("   "));  // True
Console.WriteLine(string.IsNullOrWhiteSpace(""));     // True
Console.WriteLine(string.IsNullOrWhiteSpace(null));   // True
Console.WriteLine(string.IsNullOrWhiteSpace("hi"));   // False
```

This is the standard "did the user actually type something?" check.

### Strings are immutable ⚠️

```csharp
string s = "hello";
s.ToUpper();             // creates "HELLO" and throws it away!
Console.WriteLine(s);    // hello
s = s.ToUpper();         // store the result back
Console.WriteLine(s);    // HELLO
```

String methods **never change the original**. They return a new string, and you must store it.

### ✏️ Exercise 2
Start with `string input = "  apple, Banana ,CHERRY  ";`. Split it on commas, then trim and lowercase each piece, and print them joined with `" / "`. Expected: `apple / banana / cherry`. (Hint: for now, process `parts[0]`, `parts[1]` and `parts[2]` one by one. Loops come in Step 4.)

---

## Step 3: Reading input, and `Parse` vs `TryParse` (30 min)

Replace `Program.cs` with:

```csharp
Console.Write("What is your name? ");
string? name = Console.ReadLine();

if (string.IsNullOrWhiteSpace(name))
{
    name = "stranger";
}
Console.WriteLine($"Hello, {name.Trim()}!");

Console.Write("How old are you? ");
string? ageText = Console.ReadLine();

if (int.TryParse(ageText, out int age))
{
    Console.WriteLine($"Next year you will be {age + 1}.");
}
else
{
    Console.WriteLine($"'{ageText}' is not a whole number.");
}
```

Run it and type answers. Run it again and type `abc` for the age.

Line by line:
- `Console.Write(...)` is like `WriteLine` but **doesn't** add a new line, so the cursor waits right after the question.
- `Console.ReadLine()` waits for the user to press Enter and returns what they typed **as a string, always**. Even `21` comes back as the text `"21"`.
- `string?` means "a string, **or null**". `ReadLine` can return `null` (for example, if input is closed), so its type is `string?`. More on this in Step 9.
- `if (...) { } else { }` runs one block or the other (proper coverage in Step 4).

### `Parse` vs `TryParse`: why `TryParse` wins

To turn the text `"21"` into the number `21`, there are two options:

```csharp
int age = int.Parse(ageText);      // returns the number, or CRASHES on bad input
```

Try it: temporarily replace the `if (int.TryParse...)` block with `int age = int.Parse(ageText); Console.WriteLine(age);`, run it, and type `abc`. The program dies with **`System.FormatException: The input string 'abc' was not in a correct format.`** Users type garbage all the time, so a crash is unacceptable. Undo the change.

```csharp
if (int.TryParse(ageText, out int age))
```

`TryParse` **never crashes**. It does two things at once:
1. It **returns a `bool`**: `true` if parsing worked, `false` if not. That's why it fits inside an `if`.
2. It **writes the number into `age`** through the `out` parameter. `out int age` declares a new variable right there and lets the method fill it in. If parsing failed, `age` is `0`.

Every number type has both: `decimal.TryParse`, `double.TryParse`, and so on. **Rule: use `TryParse` for anything a user typed.** Use `Parse` only when bad input is truly impossible.

### ✏️ Exercise 3
Ask for a price (decimal) and a quantity (int). If both parse, print the total formatted with `:C`. If either fails, print which one was invalid.

---

## Step 4: Control flow (45 min)

Replace `Program.cs` with:

### `if` / `else if` / `else`

```csharp
int score = 72;

if (score >= 80)
{
    Console.WriteLine("Excellent");
}
else if (score >= 50)
{
    Console.WriteLine("Pass");
}
else
{
    Console.WriteLine("Fail");
}
```

C# checks the conditions top to bottom and runs the **first** block whose condition is `true`, then skips the rest.

| Operator | Meaning |
|---|---|
| `==` `!=` | equal, not equal (`=` is assignment, `==` is comparison!) |
| `<` `>` `<=` `>=` | comparisons |
| `&&` | AND: both sides must be true |
| `\|\|` | OR: at least one side must be true |
| `!` | NOT: flips true/false |

### `switch` expressions: pick a value based on a value

Add:

```csharp
string grade = score switch
{
    >= 80 => "A",
    >= 65 => "B",
    >= 50 => "C",
    _ => "F"
};
Console.WriteLine($"Grade: {grade}");   // Grade: B

string day = "sat";
string kind = day switch
{
    "sat" or "sun" => "weekend",
    _ => "weekday"
};
Console.WriteLine(kind);                // weekend
```

- Read `score switch { ... }` as "match `score` against these patterns".
- Each line is `pattern => result`, checked top to bottom, and the first match wins.
- `_` is the **discard**, meaning "anything else". Always include it, or the compiler warns that some values aren't handled.
- It ends with `};` because the whole thing is one expression assigned to a variable.

Use `switch` expressions when you're **choosing a value**, and `if` when you're **running different actions**.

### `for`: repeat a known number of times

```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine($"Count {i}");
}
```

The three parts inside `( )`, separated by `;`:
1. `int i = 1` runs **once** at the start.
2. `i <= 5` is checked **before each loop**. When it's false, the loop stops.
3. `i++` runs **after each loop**. `i++` means "add 1 to i".

### `foreach`: go through every item in a collection

```csharp
string[] fruits = { "apple", "banana", "cherry" };
foreach (string fruit in fruits)
{
    Console.WriteLine(fruit.ToUpper());
}
```

Read it as "for each `fruit` in `fruits`, do this". You don't need to manage an index. **Prefer `foreach` whenever you don't need the index number.**

### `while`: repeat until something happens

The classic use is to keep asking until the input is valid:

```csharp
int number;
while (true)
{
    Console.Write("Enter a number from 1 to 10: ");
    if (int.TryParse(Console.ReadLine(), out number) && number >= 1 && number <= 10)
    {
        break;
    }
    Console.WriteLine("That is not a number from 1 to 10. Try again.");
}
Console.WriteLine($"You picked {number}.");
```

- `while (condition) { }` repeats as long as the condition is true. `while (true)` loops forever…
- …until `break` jumps out of the loop.
- `continue` (not used here) skips the rest of the current loop body and starts the next round.
- `&&` short-circuits: if `TryParse` fails, C# doesn't bother checking the range.

**This "loop until valid" pattern is exactly what your expense tracker needs.**

### ✏️ Exercise 4: FizzBuzz
Print the numbers 1 to 15, but print `Fizz` for multiples of 3, `Buzz` for multiples of 5, and `FizzBuzz` for multiples of both. Do it with `if`/`else if` first. Then (bonus) try a `switch` expression on the pair `(i % 3, i % 5)` with patterns like `(0, 0) => "FizzBuzz"` and `(0, _) => "Fizz"`.

**🎉 End of Session 1.** Commit (Part 3) and take a break.

---

## Session 2

## Step 5: `List<T>`, a resizable list (40 min)

Arrays (`string[]`) have a fixed size. A `List<T>` grows and shrinks. `T` is a placeholder for the item type: `List<string>`, `List<int>`, `List<decimal>`. The angle brackets are called **generics**, which week 2 covers in depth.

Replace `Program.cs` with:

```csharp
List<string> todos = new List<string>();

todos.Add("Buy milk");
todos.Add("Study C#");
todos.Add("Gym");

Console.WriteLine($"I have {todos.Count} todos");  // 3
Console.WriteLine(todos[0]);                        // Buy milk
Console.WriteLine(todos[todos.Count - 1]);          // Gym (the last item)

todos[2] = "Gym at 6pm";          // replace by index
todos.Remove("Buy milk");         // remove by value
Console.WriteLine(todos.Contains("Buy milk"));      // False
todos.Insert(0, "Wake up");       // insert at a position

for (int i = 0; i < todos.Count; i++)
{
    Console.WriteLine($"{i + 1}. {todos[i]}");
}

todos.RemoveAt(0);                // remove by index

foreach (string todo in todos)
{
    Console.WriteLine($"- {todo}");
}
```

| Member | What it does |
|---|---|
| `new List<string>()` | Creates an empty list. Shorter forms: `new()` or `[]` |
| `.Add(x)` | Appends to the end |
| `.Count` | How many items (a property, so no `()`) |
| `list[i]` | Reads or writes the item at index `i` (starting at 0) |
| `.Remove(x)` | Removes the first item equal to `x`. Returns `true`/`false` |
| `.RemoveAt(i)` | Removes the item at index `i` |
| `.Insert(i, x)` | Inserts `x` at index `i`, shifting the rest right |
| `.Contains(x)` | `true` if `x` is in the list |

**Showing numbers to users:** people count from 1, but lists count from 0. Display `i + 1`, and when the user types a number, subtract 1 to get the index. Your expense tracker's **Delete** option needs exactly this.

### Summing a list

```csharp
List<decimal> amounts = [12.50m, 8.00m, 30.25m];   // collection expression: create + fill
decimal sum = 0;
foreach (decimal a in amounts)
{
    sum += a;      // shorthand for: sum = sum + a;
}
Console.WriteLine($"Sum: {sum:F2}");   // Sum: 50.75
```

### Two crashes to know
- `todos[10]` when there are only 3 items → **`ArgumentOutOfRangeException`**. Always check that `index >= 0 && index < list.Count` before using an index a user gave you.
- Calling `todos.Remove(...)` **inside** a `foreach` over `todos` → **`InvalidOperationException: Collection was modified`**. Don't change a list while you're looping over it with `foreach`.

### ✏️ Exercise 5
Build a `List<string>` of 5 names. Print them numbered from 1. Ask the user for a number, remove that person (convert from 1-based to 0-based, and validate the range!), and print the list again.

---

## Step 6: `Dictionary<TKey, TValue>`, look things up by key (45 min)

A dictionary stores **key → value** pairs. You look up a value by its key instantly, without searching. Keys are **unique**.

Real-world mapping: category name → total spent. That's your expense tracker's "Total by category".

Replace `Program.cs` with:

```csharp
Dictionary<string, decimal> totals = new Dictionary<string, decimal>();

totals.Add("food", 25.50m);
totals.Add("rent", 800m);
totals["transport"] = 12m;                   // add OR overwrite
totals["food"] = totals["food"] + 4.50m;     // read, change, write back

Console.WriteLine(totals["food"]);   // 30.00
Console.WriteLine(totals.Count);     // 3
```

**Two ways to add, with an important difference:**
- `totals.Add("food", 10m)` **crashes** with `ArgumentException` if `"food"` already exists.
- `totals["food"] = 10m` **adds** if the key is missing and **overwrites** if it exists. It never crashes.

**But reading a missing key with `[]` crashes:** `totals["gym"]` gives **`KeyNotFoundException`**. So use one of these safe checks:

```csharp
if (totals.TryGetValue("gym", out decimal gymCost))
{
    Console.WriteLine($"Gym: {gymCost}");
}
else
{
    Console.WriteLine("No gym spending recorded.");
}

if (totals.ContainsKey("rent"))
{
    Console.WriteLine("Rent is tracked.");
}
```

`TryGetValue` follows the same pattern as `TryParse`: it returns a `bool` and fills an `out` variable. Prefer it over `ContainsKey` followed by `[]`, because it looks the key up once instead of twice.

### Looping over a dictionary

Each item is a `KeyValuePair<TKey, TValue>` with `.Key` and `.Value`:

```csharp
foreach (KeyValuePair<string, decimal> pair in totals)
{
    Console.WriteLine($"{pair.Key}: {pair.Value:F2}");
}
```

A shorter way unpacks ("deconstructs") the pair straight into two variables:

```csharp
foreach (var (category, amount) in totals)
{
    Console.WriteLine($"{category,-10} {amount,8:F2}");
}
```

`{category,-10}` pads to 10 characters, left-aligned. `{amount,8:F2}` pads to 8 characters, right-aligned, with 2 decimals. Together they make neat columns:

```
food          30.00
rent         800.00
transport     12.00
```

Remove a key with `totals.Remove("rent");`.

### The key pattern: counting or totalling by category

This is the most important thing in this step. Add it and study it:

```csharp
string[] purchases = { "food", "transport", "food", "rent", "food" };
Dictionary<string, int> counts = new();

foreach (string category in purchases)
{
    if (counts.TryGetValue(category, out int current))
    {
        counts[category] = current + 1;   // seen before → increase
    }
    else
    {
        counts[category] = 1;             // first time → start at 1
    }
}

foreach (var (category, n) in counts)
{
    Console.WriteLine($"{category}: {n}");
}
// food: 3
// transport: 1
// rent: 1
```

Change `int` to `decimal` and `+ 1` to `+ amount`, and you have the expense tracker's "Total by category".

### `List` or `Dictionary`?

| Use a `List<T>` when… | Use a `Dictionary<K,V>` when… |
|---|---|
| Order matters | You look things up by a name or ID |
| You access items by position | Each key must be unique |
| Duplicates are fine | You're grouping or totalling by category |

### ✏️ Exercise 6
Ask the user to type words separated by spaces. Count how often each word appears (lowercase them first so `The` and `the` count as one) and print the counts.

---

## Step 7: Methods (40 min)

A **method** is a named, reusable block of code. As your expense tracker grows, every menu option should become its own method.

Replace `Program.cs` with:

```csharp
Greet("Joshua");
Greet();

int sum = Add(3, 4);
Console.WriteLine(sum);                        // 7

Console.WriteLine(FormatMoney(19.5m));         // $19.50
Console.WriteLine(FormatMoney(19.5m, "RM"));   // RM19.50

if (TryReadAmount("12.50", out decimal amount))
{
    Console.WriteLine($"Valid amount: {amount}");
}
if (!TryReadAmount("-3", out decimal bad))
{
    Console.WriteLine($"Rejected, amount was set to {bad}");
}

int x = 5;
TryToChange(x);
Console.WriteLine(x);                          // still 5!

// ---------- Methods go below the top-level code ----------

static void Greet(string name = "friend")
{
    Console.WriteLine($"Hello, {name}!");
}

static int Add(int a, int b)
{
    return a + b;
}

static string FormatMoney(decimal value, string symbol = "$") => $"{symbol}{value:F2}";

static bool TryReadAmount(string? text, out decimal amount)
{
    if (decimal.TryParse(text, out amount) && amount > 0)
    {
        return true;
    }
    amount = 0;
    return false;
}

static void TryToChange(int n)
{
    n = n * 2;
}
```

### Anatomy of a method

```
static   int   Add  (int a, int b)
  │       │     │         │
  │       │     │         └─ parameters: inputs, each with a type and a name
  │       │     └─ name: PascalCase by convention (Add, not add)
  │       └─ return type: what comes back. void = nothing comes back
  └─ modifier (explained below)
```

- **`return`** sends a value back to the caller and exits the method immediately.
- **`void`** methods do something (like printing) but give nothing back.
- **Default parameters**: `string name = "friend"` makes the argument optional. Default parameters must come **after** the required ones.
- **Expression-bodied** methods: `=> $"{symbol}{value:F2}";` is shorthand for a method whose body is a single `return`.
- **`out` parameters**: `TryReadAmount` copies the `TryParse` pattern. It returns success as a `bool` and hands the result back through `out`. The method **must** assign every `out` parameter before it returns, which is why `amount = 0;` is there.
- **Pass by value**: `TryToChange(x)` receives a **copy** of `x`. Changing `n` inside doesn't affect `x` outside. That's why methods should *return* results rather than try to change their inputs.

### What does `static` mean?

In this file, `static` on a local method means **"this method can only use its own parameters, not variables from the code above it."** That's good discipline: everything a method needs comes in through its parameters, so it's easy to understand and test.

In week 2, when you write classes, `static` gets its full meaning: "belongs to the class itself, not to a particular object". `Console.WriteLine` and `int.TryParse` are static methods. You call them on the type name, not on an object.

> **Rule for top-level files:** all your loose code goes at the top, and method declarations go below it. (Classes, from week 2, must come after both.)

### ✏️ Exercise 7
Write `static int ReadIntInRange(string prompt, int min, int max)`. It should keep asking (Step 4's `while` loop) until the user types a valid int between `min` and `max`, then return it. Use it to ask for an age (0–120) and a menu choice (1–5). **You'll reuse this method in your expense tracker.**

---

## Step 8: Nullable warnings (20 min)

First, see the warnings. Replace `Program.cs` with:

```csharp
string name = Console.ReadLine();
Console.WriteLine(name.Length);
```

Run `dotnet build`. You get two **warnings** (yellow, not red, so it still builds):

```
warning CS8600: Converting null literal or possible null value to non-nullable type.
warning CS8602: Dereference of a possibly null reference.
```

Translated:
- **CS8600:** "`ReadLine()` can return `null`, but you put it in a plain `string`, which promises never to be null."
- **CS8602:** "You're calling `.Length` on something that might be null. If it is, the program crashes with `NullReferenceException`."

`<Nullable>enable</Nullable>` in your `.csproj` turns these warnings on. They catch the most common runtime crash in .NET **before** you run the program. **Treat every nullable warning as a bug to fix.**

Now the fixes:

```csharp
string? maybe = Console.ReadLine();     // 1. be honest: it might be null

if (maybe is not null)                  // 2. check before use
{
    Console.WriteLine(maybe.Length);    //    the compiler knows it's safe in here
}

Console.WriteLine(maybe?.Length);       // 3. ?. means "if null, give null instead of crashing"

string safe = maybe ?? "";              // 4. ?? means "if null, use this instead"
Console.WriteLine(safe.Length);
```

| Syntax | Meaning |
|---|---|
| `string?` | This variable may hold `null` |
| `x is not null` | Null check. Inside the `if`, the compiler treats `x` as safe |
| `x?.Length` | Null-conditional: returns `null` instead of crashing if `x` is null |
| `x ?? "default"` | Null-coalescing: uses the right side if `x` is null |

Common pattern for input: `string input = (Console.ReadLine() ?? "").Trim();`

### ✏️ Exercise 8
Go back to your code from Exercises 3 and 7 and run `dotnet build`. Fix every nullable warning until the build shows **0 Warning(s)**.

---

## Part 2: Debugging in VS Code (20 min)

Debugging means pausing your program mid-run to look inside it. You'll use it every day.

1. Put the Step 6 dictionary code back in `Program.cs` (it doesn't need keyboard input, which keeps debugging simple).
2. In VS Code, open `Program.cs` and click in the **gutter**, the empty space left of the line numbers, on the line `counts[category] = current + 1;`. A **red dot** appears. That's a **breakpoint**.
3. Press **F5**. If asked, choose **C#**, then the **Playground** project.
4. The program pauses on your breakpoint (the line is highlighted yellow and has **not run yet**).
5. Look at the **Variables** panel on the left. Expand `counts` to see the dictionary's contents. Check `category` and `current`.
6. Use the controls:

| Key | Name | What it does |
|---|---|---|
| **F10** | Step Over | Runs the current line and moves to the next |
| **F11** | Step Into | If the line calls one of your methods, goes inside it |
| **Shift+F11** | Step Out | Finishes the current method and returns to the caller |
| **F5** | Continue | Runs until the next breakpoint |
| **Shift+F5** | Stop | Ends debugging |

7. Press **F5** repeatedly and watch the `food` count go 1 → 2 → 3 in the Variables panel.
8. Bonus: hover over any variable in the editor while paused to see its value.

---

## Part 3: Committing your work

Do this at the end of every step, so each step is saved before you overwrite `Program.cs`. Run these from the repo root (`cd` back to `.NET_Tutorial` first):

```powershell
git status
git add .
git commit -m "wk1: step 4 control flow"
git push
```

| Command | What it does |
|---|---|
| `git status` | Shows which files changed. **Check it every time.** If you see `bin/` or `obj/`, your `.gitignore` is missing (Part 0.3) |
| `git add .` | **Stages** all changes in the current folder and below, meaning "include these in the next commit" (`.` = current folder) |
| `git commit -m "..."` | Saves a snapshot with a message describing what changed |
| `git push` | Uploads your commits to GitHub |

To look at an earlier step later: `git log --oneline` lists your commits, and `git show <commit-id>:wk1/Playground/Program.cs` prints that file as it was in that commit.

> Tip: if you want to keep exercise solutions as files, save them as plain text such as `wk1/exercises/ex4-fizzbuzz.cs.txt`. A project builds every `.cs` file in its folder, and two files with top-level statements cause an error.

---

## Session 1 and 2 checklist

Before starting Session 3 (the expense tracker), you should be able to answer these without looking:

- [ ] What do `dotnet new console -n X`, `dotnet build`, `dotnet run` and `dotnet watch run` each do?
- [ ] Why `decimal` for money and not `double`?
- [ ] What does `7 / 2` print, and why?
- [ ] Is `var` dynamic typing? (No. Why not?)
- [ ] Why does `s.ToUpper();` on its own do nothing useful?
- [ ] Why `TryParse` instead of `Parse` for user input? What does `out` do?
- [ ] When do you use a `switch` expression vs `if`?
- [ ] `List` vs `Dictionary`: give one use case for each.
- [ ] What's the difference between `dict.Add(k, v)` and `dict[k] = v`?
- [ ] What do `string?`, `?.` and `??` mean?
- [ ] You've set a breakpoint, pressed F5, and inspected a variable.
- [ ] Your work is committed and pushed.

**Next: Session 3, the expense tracker.** You now have every building block it needs: a `while` loop menu, `ReadIntInRange` from Exercise 7, a `List` of expenses, a `Dictionary` for category totals, and methods for each menu option.
