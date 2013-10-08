# Lambdas

Lambdas are written with a single space before and after the `=>`:

```csharp
// Great.
Func<int, int> square = i => i * i;

// Terrible.
Func<int, int> square = i=>i * i;
```

If your lambda takes a single argument, omit the parentheses around the argument list:

```csharp
// Great!
var admins = Users.Select (user => user.IsAdministrator);

// Silly.
var admins = Users.Select ((user) => user.IsAdministrator);
```

Whenever possible, omit types from lambda argument lists, and use simple names:

```csharp
// Great:
list.OnScroll += (sender, e) => {
	...
};

// Passé:
list.OnScroll += (object sender, EventArgs e) => {
	...
};

// No! Parameter name is needlessly complex:
sqlDatabaseAdaptors.Select (sqlDatabaseAdaptor => sqlDatabaseAdaptor.Id);

// Much better. We have enough context from the larger identifier:
sqlDatabaseAdaptors.Select (adaptor => adaptor.Id);
```

When the body of a lambda is a simple statement or expression, don't use a block:

```csharp
// Excellent!
var averageSalary = employees.Average (employee => employee.Salary);

// Inconceivable!
var averageSalary = employees.Average (employee => { return employee.Salary; });
```

When the body of the lambda is a block, put the opening brace on the same line as the `=>`, indent the body of the block,
and close the block at the same level of indentation as the line containing the opening brace:

```csharp
// Ideal:
people.ForEach (person => {
	person.BrushTeeth ();
	person.CallMom ();
	person.RegisterToVote ();
});

// No! Improperly positioned opening brace:
people.ForEach (person =>
{
	person.BrushTeeth ();
	person.CallMom ();
	person.RegisterToVote ();
});

// No! Improperly positioned closing brace:
people.ForEach (person => {
	person.BrushTeeth ();
	person.CallMom ();
	person.RegisterToVote ();
	}
);

// No! Bad indentation:
people.ForEach (person => { person.BrushTeeth ();
                            person.CallMom ();
                            person.RegisterToVote ();
                          });
```

Always prefer lambdas, `Func<>`, and `Action<>` types to `delegate`. The only recommended use of `delegate` is when the body of your anonymous method doesn't reference any of its arguments:

```csharp
thing.EventWithSenderAndEventArgs += delegate {
	Console.WriteLine ("EventWithSenderAndEventArgs raised.");
};
```

It is acceptable to use single-character argument names in lambdas if the receiver is an `IEnumerable` and is named in such a way as to make the lambda argument obvious, and the lambda argument name is the first character of the receiver's identifier:

```csharp
// Acceptable:
var averageSalary = employees.Average (e => e.Salary);

// Acceptable:
var averageSalary = employees.Average (employee => employee.Salary);
```
