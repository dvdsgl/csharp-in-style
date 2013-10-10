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

## Specific Guides

* [Comments](Comments.md)
* [Lambdas](Lambdas.md)

## General Guidelines

### using Statements

Group using statements by common prefix, with shorter namespaces coming before longer ones, creating neat clusters of statements separated by single empty lines.

Namespaces should be ordered from platform-neutral through platform-specific, with .NET namespaces first, then library or component namespaces, then Xamarin namespaces, then application namespaces:

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

```csharp
class MyClass : BaseClass, IDoesThis
{
}
```

* Put a space before and after `:`.

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

### Type Inference

Use it. Less typing is almost always better than more typing, with some important exceptions.

Use `var` when the type is repeated on the right-hand side of the assignment:

```csharp
// Perfect!
var users = new Dictionary<UserId, User> ();

// Bloated.
Dictionary<UserId, User> users = new Dictionary<UserId, User> ();
```

Don't use `var` for capturing the return type of a method or property when the type is not evident:

```csharp
// Horrendous.
var things = Interpret (data);

// Much better.
HashMap<Thing> things = Interpret (data);
```

Omit the type when using array initializers:

```csharp
// Could be better:
database.UpdateUserIds (new int[] { 1, 2, 3 });

// Better:
database.UpdateUserIds (new [] { 1, 2, 3 });
```

### Object and Collection Initializers

Use them.

For simple initializers, you may do a one-liner:

```csharp
// Perfect.
var person = new Person ("Vinny") { Age = 50 };

// Acceptable.
var person = new Person ("Vinny") {
	Age = 50,
};
```

Omit the `()` when using parameterless constructors:

```csharp
// Perfect.
var person = new Person { Name = "Bob", Age = 75 };

// Wrong.
var person = new Person () { Name = "Bob", Age = 75 };
```

Every expression should be on a separate line, and every line should end with a comma `,`:

```csharp
// Very nice collection initializer.
var entries = new Dictionary<string, int> {
	{ "key1", 1 },
	{ "key2", 2 },
};

// Bad – multiple entries on one line.
var entries = new Dictionary<string, int> {
	{ "key1", 1 }, { "key2", 2 },
};
```

List initializers in increasing order of line length to create a neat pyramid shape:

```csharp
// Very nice pyramid shape.
var contact = new Person {
	Name = "David Siegel",
	SocialSecurityNumber = 123456789,
	Address = "1234 Montgomery Circle Drive East",
};

// Wrong - inverted pyramids are unstable.
var contact = new Person {
	Address = "1234 Montgomery Circle Drive East",
	SocialSecurityNumber = 123456789,
	Name = "David Siegel",
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

### Where to put spaces

Always put a space before every opening parenthesis, left square bracket, and left curly brace, even when calling methods or indexing. This is perhaps the most controversial style guideline in these documents, but we believe you will grow to like this style--it gives code plenty of room to breathe, especially on lines with many method calls.

```csharp
// Beautiful. Try it, you'll like it.
Initialize (database);	
products [i];

// Don't do this:
Initialize(database);	
products[i];
```

Do not put a space before the left angle bracket in a generic type:

```csharp
// Perfect.
var scores = new List<int> ();

// Incorrect.
var scores = new List <int> ();
```

Do not put spaces inside parentheses, square brackets, or angle brackets:

```csharp
// Wrong - spaces inside.
Initialize ( database );	
products [ i ];
new List < int > ();
```

Separate type parameters to generic types by a space:

```csharp
// Excellent.
var users = new Dictionary<UserId, User> ();

// Worthless.
var users = new Dictionary<UserId,User> ();
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
	someone.SetFree ();
}

// Wrong.
if (you.Love (someone))
{
	someone.SetFree ();
}
```

Omitting braces for single line if statements is fine, however braces are always acceptable:

```csharp
// Lovely.
if (you.Like (it))
	it.PutOn (ring);

// Acceptable.
if (you.Like (it)) {
	it.PutOn (ring);
}
```

Very short statements may be one-liners, especially when the body is a `return`:

```csharp
// Lovely.
if (condition) return;

// Wrong - too complex for a single line:
if (people.All (p => p.IsAdmin)) return new AdminPage ();
```

Always use braces with nested or multi-line conditions:

```csharp
// Perfect.
if (a) {
	if (b) {
		code ();
	}
}

// Acceptable.
if (a) {
	if (b)
		code ();
}

// Wrong.
if (a)
	if (b)
		code ();
```

When defining a method, use the C style for brace placement, that means, use a new line for the brace, like this:

good:

```csharp
// Correct.
void LaunchRockets ()
{
}

// Wrong.
void LaunchRockets () {
}
```

Properties and indexers are an exception, keep the brace on the same line as the property declaration. This makes it visually simple to distinguish them.

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
		return people.Average (p => p.Age);
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
void EmptyMethod ()
{
}

// These are wrong.
void EmptyMethod () {}
void EmptyMethod () {
}
void EmptyMethod () 
{}
```

Generic method type parameter constraints are one separate lines, one line per type parameter, before the opening brace:

```csharp
static bool TryParse<TEnum> (string value, out TEnum result)
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

Namespaces, types, and methods all put braces on their own lines:

```csharp
// Correct.
namespace N
{
	class X
	{
		...
	}
}

Wrong - opening braces are not on their own lines.
namespace N {
	class X {
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

### Multiline Parameters

When you need to write down parameters in multiple lines, indent the parameters to be below the previous line parameters and indented two tab stops, like this:

Good:

```csharp
WriteLine (format, foo,
		bar, baz);
```

If you do not want to have parameters in the same line as the method invocation because you ar running out of space, you can indent the parameters in the next line, like this:

Good:

```csharp
WriteLine (
		format, moved, too, long);
```

Comma separators go at the end, like a good book, never at the beginning:

Good:

```csharp
WriteLine (foo,
		bar,
		baz);
```

Atrocious:

```csharp
WriteLine (foo
		, bar
		, baz);
```

Use whitespace for clarity

Use white space in expressions liberally, except in the presence of parenthesis:

good:

```csharp
if (a + 5 > method (blah () + 4))
```

bad:

```csharp
if (a+5>method(blah()+4))
```

### File headers

For any new files, please use a descriptive introduction, like this:

```csharp
//
// System.Comment.cs: Handles comments in System files.
//
// Author:
//   Juan Perez (juan@address.com)
//
// Copyright (C) 2002 Address, Inc (http://www.address.com)
//
```

If you are modyfing someone else's code, and your contribution is significant, please add yourself to the Authors list.

### Casing

Argument names should use the camel casing for identifiers, like this:

good:

```csharp
void Method (string myArgument)
```

bad:

```csharp
void Method (string lpstrArgument)
void Method (string my_string)
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
class Foo {
	int bar;
 
	void Update (int newValue)
	{
		bar = newValue;
	}
 
	void Clear ()
	{
		Update ();
	}
}
```

Bad:

```csharp
class Foo {
	int bar;
 
	void Update (int newValue)
	{
		this.bar = newValue;
	}
 
	void Clear ()
	{
		this.Update ();
	}
}
```

An exception is made for "this" when the parameter name is the same as an instance variable, this happens sometimes in constructors or if naming is difficult:

Good:

```csharp
class Message {
	char text;
 
	public Message (string text)
	{
		this.text = text;
	}
}
```

### Line length and alignment

Line length: The line length for C# source code is 80 columns.

If your function declaration arguments go beyond this point, please place them on new lines, indenting by two tab stops (preferred), or align with the opening brace (acceptable). ( **Rationale** : By not aligning to the opening brace, you can refactor/rename the method in the future without "screwing up" the indentation of all remaining parameters.)

When aligning to the opening brace, use the same number of tabs used on the first line followed by enough spaces to align the arguments. This ensures that the arguments will remain aligned when viewed with a different tabsize. In the following example, the line that declares argc is indented with 2 tabs and 14 spaces:

```csharp
namespace N {
	class X {
		// Preferred
		void Method1 (int arg, string argb,
				int argc)
		{
		}

		// acceptable
		void Method2 (int arg, string argb,
		              int argc)
		{
		}
	}
}
```

When a _condition_ for a branch or loop construct requires multiple lines, each line should be indented by two tab stops. ( **Rationale** : it keeps the condition from looking like it's part of the body.) Complicated and nested expressions should use spaces to align with the originating expression. Boolean operators should be at the end of the line.

```csharp
void M ()
{
	if (string.IsNullOrEmpty (DataHost) &&
			string.IsNullOrEmpty (DataPort) &&
			string.IsNullOrEmpty (DataScheme))
		DoSomething ();
	if (!method.IsConstructor &&
			method.Name == "SomeMethod" &&
			(method.IsVirtual ||
			 method.IsAbstract))
		DoSomethingElse ();
}
```

When invoking functions, the rule is different, the arguments are not aligned with the previous argument, instead they begin at two tab stops, like this. When "chaining" method calls, each "chain" should be on a new line (within reason) and each sub-expression should be indented one tab stop.

```csharp
void M ()
{
	MethodCall ("Very long string that will force",
			"Next argument on the 8-tab pos",
			"Just like this one");
	IEnumerable<int> items = Enumerable.Range (0, 100)
		.Where (e => (e % 2) == 0)
		.Select (e => e*2);
}
```

## Credits

This guide was adapted from the [Mono coding guidelines](http://www.mono-project.com/Coding_Guidelines) with inspiration from thoughtbot's excellent [guide for programming in style](https://github.com/thoughtbot/guides).
