## Introduction

These documents contain guidelines for writing consistent, lucid, enticing, modern C#.

If you take issue with anything here, please open a pull request with your recommended changes and include an argument for *and against* their adoption; explain the benefits of your proposed change, and also any drawbacks.

## Guiding Principles

* Be consistent.
* Don't rewrite existing code to follow this guide.
* Don't violate a guideline without a good reason.
* A reason is good when you can convince a teammate, not just when you like it.
* Assume your reader knows C# and English.
* Prefer clarity to 'performance'.
* Prefer clarity to .NET dogma.
* Write comments that people want to read, with correct spelling and grammar.

## The Rundown

* Indent with tabs.
* Max line length is 100 columns.
* Use spaces and empty lines precisely.
* Braces generally go on their own lines.
* Never put a space before `[`.
* Always put a space before `{`.
* Always put a space before `(` except for method invocations or when following another `(`.

## Specific Guides

* [Comments](Comments.md)
* [Naming](Naming.md)
* [Lambdas](Lambdas.md)

## General Guidelines

### File Layout

Layout your `.cs` files like this:

```
File Header

Using Directives

Namespace Declaration

	Type Declaration
		Constants
	
		Static Fields
	
		Static Auto-Properties
	
		Static Constructor
		
		Complex Static Properties
	
		Static Methods
	
		Fields
		
		Auto-Properties
	
		Constructors
		
		Destructor
		
		Complex Properties
		
		Methods
```

An exception to this layout is manual properties with a backing field used exclusively via the property; these members should occur in the file together in the properties section. If your backing field is accessed anywhere other than inside the property definition, stick to normal layout rules.

```csharp
string name;
public string Name {
	get { return name; }
	set { name = value; }
}
```

### using Directives

Group using directives by common prefix, with shorter namespaces coming before longer ones, creating neat clusters of statements separated by single empty lines.

Namespaces should be ordered in increasing order of platform specificity, with .NET namespaces first, then library or component namespaces, then Xamarin namespaces, then application namespaces:

```csharp
// Beautiful:
using System;
using System.Linq;
using System.Collections.Generic;

using MyLib;
using MyLib.Extensions;

using MonoTouch.UIKit;
using MonoTouch.Foundation;

using MyApp;

// Disaster:
using MyLib.Extensions;
using MonoTouch.Foundation;
using System.Collections.Generic;
using System;
using System.Linq;
using MonoTouch.UIKit;
using MyLib;
```

Prune redundant namespaces aggressively.

### Declaring Types

Leave an empty line between every type definition:

```csharp
// Perfect.
namespace MyApp
{
	enum Direction { Left, Right }
	
	class ImportantThing
	{
		...
	}
}

// Wrong - missing and empty line between type definitions.
namespace MyApp
{
	enum Direction { Left, Right }
	class ImportantThing
	{
		...
	}
}

// Wrong - more than one empty line.
namespace MyApp
{
	enum Direction { Left, Right }
	
	
	class ImportantThing
	{
		...
	}
}
```

Put a space before and after `:` when listing base classes and interfaces.

```csharp
// Perfect.
class MyClass : BaseClass, IDoesThis
{
}

// Wrong.
class MyClass: BaseClass, IDoesThis
{
}
```

#### Enums

Simple enums may be defined on a single line:

```csharp
enum Edge { Left, Right, Bottom, Top }
```

Larger enums should list entries on separate lines and always end in a comma:

```csharp
enum StringSplitOptions
{
	None = 0,
	RemoveEmptyEntries = 1,
}
```

## Member Decalarations

Leave an empty line before every method, property, indexer, constructor, and destructor:

```csharp
class Person
{
	string name;
	
	public Person(string name)
	{
		this.name = name;
	}
}
```

Automatic properties don't need to be preceeded by an empty line:

```csharp
class Person
{
	string Name { get; set; }
	int Age { get; set; }
	
	...
}
```

### Methods

```csharp
public async Task<string[]> Query<TDatabase>(User user, TDatabase database, Role role = Role.Admin)
	: where TDatabase : IDatabase
{
}
```

### Properties

Declare automatic properties on a single line with the exact spacing shown below:

```csharp
// Perfect.
string Name { get; set; }
```

Simple properties may define `get` and `set` on a single line each, with `get` first:

```csharp
// Perfect.
string Name {
	get { return name; }
	set { name = value; }
}
```

Also note the single spaces before and after `{`, and the space before `}`.

Complex properties go like this:

```csharp
// Perfect.
string Name {
	get {
		return name;
	}
	set {
		name = value;
	}
}
```

### Type Inference

Use it. Less typing is almost always better than more typing, with some important exceptions.

Use `var` when the type is repeated on the right-hand side of the assignment:

```csharp
// Perfect!
var users = new Dictionary<UserId, User>();

// Bloated.
Dictionary<UserId, User> users = new Dictionary<UserId, User>();
```

Don't use `var` for capturing the return type of a method or property when the type is not evident:

```csharp
// Horrendous.
var things = Interpret(data);

// Much better.
HashMap<Thing> things = Interpret(data);

// Even better.
var things = InterpretAs<Thing>(data);

```

Omit the type when using array initializers:

```csharp
// Could be better:
database.UpdateUserIds(new int[] { 1, 2, 3 });

// Better:
database.UpdateUserIds(new [] { 1, 2, 3 });
```

### Object and Collection Initializers

Use them.

For simple initializers, you may do a one-liner:

```csharp
// Perfect.
var person = new Person("Vinny") { Age = 50 };

// Acceptable.
var person = new Person("Vinny") {
	Age = 50,
};
```

Omit the `()` when using parameterless constructors:

```csharp
// Perfect.
var person = new Person { Name = "Bob", Age = 75 };

// Wrong.
var person = new Person() { Name = "Bob", Age = 75 };
```

In general, each expression should be on a separate line, and every line should end with a comma `,`:

```csharp
// Very nice collection initializer.
var entries = new Dictionary<string, int> {
	{ "key1", 1 },
	{ "key2", 2 },
};

// Very nice object initializer.
var contact = new Person {
	Name = "David Siegel",
	SocialSecurityNumber = 123456789,
	Address = "1234 Montgomery Circle Drive East",
};

// Bad collection initializer – multiple entries on one line.
var entries = new Dictionary<string, int> {
	{ "key1", 1 }, { "key2", 2 },
};
```

### Indentation

`switch` statements have the case at the same indentation as the `switch`:

```csharp
switch (x) {
case 'a':
	...
case 'b':
	...
}
```

### Where to put spaces[1]

We prefer to put a space before an open parenthesis only in control flow statements, but not in normal method/delegate/lambda calls, or expressions. This makes method invocations stand out from simple logical groupings. For example, this is good:

```csharp
// Flow control...
if (awesome) ...
foreach (var foo in foos) ...
while (hazMonkeys) ...

// Logical grouping...
var result = b * (4 + i);

// Method invocation.
Foo(database);
Debug.Assert(5 + (3 * 4) && "laws of math are failing me");

// Consider
A = result ?? (int) compute (foo (b + 1));

// At first glance it Looks very similar to:
A = result ?? (int) compute (foo) (b + 1);


// Whereas:
A = result ?? (int) compute(foo(b + 1));

// Looks more immediately distinct from
A = result ?? (int) compute(foo)(b + 1);
```
The reason for doing this is not completely arbitrary. This style makes control flow operators stand out more, and makes expressions flow better. The function call operator binds very tightly as a postfix operator. In some cases, such as when C# is embedded in Razor markup, inserting a space before an opening parenthesis will cause compilation to fail.

[1] Adapted from http://llvm.org/docs/CodingStandards.html#spaces-before-parentheses

Do not put a space before the left angle bracket in a generic type:

```csharp
// Perfect.
var scores = new List<int>();

// Incorrect.
var scores = new List <int>();
```

Do not put spaces inside parentheses, square brackets, or angle brackets:

```csharp
// Wrong - spaces inside.
Initialize( database );	
products[ i ];
new List< int >();
```

Separate type parameters to generic types by a space:

```csharp
// Excellent.
var users = new Dictionary<UserId, User>();

// Worthless.
var users = new Dictionary<UserId,User>();
```

Put a space between the type and the indentifier what casting:

```csharp
// Great.
var person = (Person) sender;

// Bad.
var person = (Person)sender;
```

### Where to put braces

Inside a code block, put the opening brace on the same line as the statement:

```csharp
// Lovely.
if (you.Love (someone)) {
	someone.SetFree();
}

// Wrong.
if (you.Love (someone))
{
	someone.SetFree();
}
```

Omitting braces for single line if statements is fine, however braces are always acceptable:

```csharp
// Lovely.
if (you.Like (it))
	it.PutOn(ring);

// Acceptable.
if (you.Like (it)) {
	it.PutOn(ring);
}
```

Very short statements may be one-liners, especially when the body is a `return`:

```csharp
// Lovely.
if (condition) return;

// Acceptable, but a little complex for a one-liner.
if (people.All(p => p.IsAdmin)) return new AdminPage();

// Wrong - too complex for a single line:
if (people.Where(p => p.IsAdmin).Average(p => p.Age) > 21) return DrinkDispenser.FireWater;
```

Always use braces with nested or multi-line conditions:

```csharp
// Perfect.
if (a) {
	if (b) {
		code();
	}
}

// Acceptable.
if (a) {
	if (b)
		code();
}

// Wrong.
if (a)
	if (b)
		code ();
```

When defining a method, put the opening brace on its own line:

```csharp
// Correct.
void LaunchRockets()
{
}

// Wrong.
void LaunchRockets() {
}
```

When defining a property, keep the opening brace on the same line:

```csharp
// Perfect.
double AverageAge {
	get {
		return people.Average (p => p.Age);
	}
}


// Wrong.
double AverageAge
{
	get {
		return people.Average(p => p.Age);
	}
}
```


Notice how  `get` keeps its brace on the same line.

For very small properties, you can compress things:


```csharp
// Preferred.
int Property {
	get { return value; }
	set { x = value; }
}

// Acceptable.
int Property {
	get {
		return value;
	}
	set {
		x = value;
	}
}
```

Empty methods should have the body of code using two lines, in consistency with the rest:

```csharp
// Good.
void EmptyMethod()
{
}

// These are wrong.
void EmptyMethod() {}

void EmptyMethod() 
{}
```

Generic method type parameter constraints are on separate lines, one line per type parameter, indented once:

```csharp
static bool TryParse<TEnum>(string value, out TEnum result)
	where TEnum : struct
{
	...
}
```

If statements with else clauses are formatted like this:

good:

```csharp
if (dingus) {
        ...
} else {
        ... 
}
```

bad:

```csharp
if (dingus) 
{
        ...
} 
else 
{
        ... 
}
```

bad:

```csharp
if (dingus) {
        ...
} 
else {
        ... 
}
```

Namespaces, types, and methods all put braces on their own line:

```csharp
// Correct.
namespace MyApp
{

	class FluxCapacitor
	{
		...
	}
}

// Wrong - opening braces are not on their own lines.
namespace MyApp {
	class FluxCapacitor {
		...
	}
}
```

To summarize:

| Statement	                 | Brace position |
|--------------------------------|----------------|
| Namespace                      | new line |
| Type                           | new line |
| Methods                        | new line |
| Constructors                   | new line |
| Destructors                    | new line |
| Properties                     | same line |
| Control blocks (if, for...)    | same line |
| Anonymous types and methods    | same line |

### Long Argument Lists

When your argument list grows too long, split your method invocation across multiple lines, with the first argument on a new line after the opening parenthesis of the method invocation, the closing parenthesis of the invocation on its own line at the same indentation level as the line with the opening parenthesis. This style works especially well for methods with named parameters.

```csharp
// Lovely.
Console.WriteLine(
	"Connect to {0} via {1} with extra data: {2} {3}",
	database.Address,
	database.ConnectionMethod.Description,
	data.FirstPart,
	data.SecondPart
);
```

It's also acceptable to put multiple arguments on a single line when they belong together:

```csharp
// Acceptable.
Console.WriteLine(
	"Connect to {0} via {1} with extra data: {2} {3}",
	database.Address,
	database.ConnectionMethod.Description,
	data.FirstPart, data.SecondPart
);
```

When chaining method calls, each method call in the chain should be on a separate line indented once:

```csharp
void M() {
	IEnumerable<int> items = Enumerable.Range(0, 100)
		.Select(e => e * 2);
}
```

Use single spaces in expressions liberally:

good:

```csharp
// Good.
if (a + 5 > method(blah() + 4))

// Bad.
if (a+5>method(blah()+4))
```

### Casing

Argument names should use the camel casing for identifiers, like this:

good:

```csharp
// Good.
void Method(string myArgument)

// Bad.
void Method(string lpstrArgument)
void Method(string my_string)
```


### Instance Fields

Don't use  `m_` or `_` as prefixes for instance fields. Just use normal parameter naming conventions:

```csharp
// Perfect.
class Person
{
	string name;
}

// Wrong.
class Person
{
	string m_name;
}
```

Don't write `private` for private members, as this is the default visibility in C#:

```csharp
// Perfect.
class Person
{
	string name;
}

// Wrong.
class Person
{
	private string name;
}
```

An exception to this rule is serializable classes. In this case, if we desire to have our serialized data be compatible with Microsoft's, we must use the same field name.

### `this`

The use of "this." as a prefix in code is discouraged, it is mostly redundant. In general, since internal variables are lowercase and anything that becomes public starts with an uppercase letter, there is no ambiguity between what the "Foo" and "foo" are. The first is a public property or field, the second is internal property or field.

Good:

```csharp
class Foo
{
	int bar;
 
	void Update(int newValue)
	{
		bar = newValue;
	}
 
	void Clear()
	{
		Update();
	}
}
```

Bad:

```csharp
class Foo
{
	int bar;
 
	void Update(int newValue)
	{
		this.bar = newValue;
	}
 
	void Clear()
	{
		this.Update();
	}
}
```

An exception is made for `this` when the parameter name is the same as an instance variable, this happens sometimes in constructors or if naming is difficult:

Good:

```csharp
class Message
{
	char text;
 
	public Message(string text)
	{
		this.text = text;
	}
}
```

### Enumerations

Don't use the [Flags] attribute, instead use a set of the enumeration. This adheres to the general principle that when all else is equal, prefer typed over untyped code.

Bad:
```csharp
[Flags]
enum Inventory
{
	Hat,
	Sword,
	BigSword
}

class Player
{
	Inventory Items
	{
		get;
		set;
	}

	int CalculateDamage()
	{
		int result = 0;
		if (Items.HasFlag(Inventory.Sword))
			result += 2;
		if (Items == Inventory.BigSword) //Was the omission of HasFlag intended?
			result += 1;
		return result;
	}
}
```

Good:
```csharp
	enum Item
	{
		Hat,
		Sword,
		BigSword
	}

	class Player
	{
		ISet<Item> Items
		{
			get;
			set;
		}

	int CalculateDamage()
	{
		int result = 0;
		if (Items.Contains(Item.Sword))
			result += 2;
		if (Items.SetEquals(new [] { Item.BigSword })) //Intention is clear.
			result += 1;
		return result;
	}
}
```

## Credits

This guide was adapted from the [Mono coding guidelines](http://www.mono-project.com/Coding_Guidelines) with inspiration from thoughtbot's excellent [guide for programming in style](https://github.com/thoughtbot/guides) and [The LLVM Coding Standards](http://llvm.org/docs/CodingStandards.html).
